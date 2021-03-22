---
layout: post
title: C++ move semantics
published-on: March 21, 2021
---

# C++ move semantics

Last time I talked about "lvalues" and "rvalues" in C++. I actually made a mistake saying that the element that is on the left side of the equal sign is an "lvalue", and the opposite is an "rvalue". Although in older C++ versions this rule may work. In modern C++ there is a much better rule. If a value is represented by a name, say, a variable, then it's likely to be an "lvalue". If it doesn't, like a literal or an expression, something more "temporary", then it's likely to be an "rvalue". Note that a string literal is actually an "lvalue". This will help us to understand one of the modern C++ features: move semantics. 

First, let's look at `std::move()`. Suppose I got two vectors:

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector v1 = {1, 2, 3, 4, 5, 6, 7};

    std::vector<int> v2{};

    v2 = std::move(v1);

    for (auto i : v2) {
        std::cout << i; // 1234567
    }

    for (auto j : v1) {
        std::cout << j << std::endl; // nothing
    }
}
```

`v1` moved into `v2`. `v1` became empty afterwards. However, there is a bit of subtlety here. Looking at the documentation of `std::move()`, it returns a `std::remove_reference<T>::type&&`. An "rvalue" reference. `v1` was originally an "lvalue", `std::move` essentially converts an "lvalue" to an "rvalue". This hints what is required for move semantics: using "rvalues". 

Another thing to note is that all built-in data structures in C++ support move. Simply use `std::move()`. This improves performance by avoiding copying. Definitely a nice feature in modern C++.

So what about my custom classes? This is the time when we need a bit more work, but it's still pretty simple. Let's look at the class `Fraction` by itself. 

```cpp
class Fraction {
private:
    int numerator;
    int denominator;

public:
    Fraction() {}
    Fraction(int n, int d) : numerator(n), denominator(d) {}
};
```

This class contains two built-in C++ types, remember this since it would be useful later. Now let's add the copy constructor and the copy assignment operator. 

```cpp
class Fraction {
private:
    int numerator;
    int denominator;

public:
    Fraction() {}

    Fraction(int n, int d) 
    : numerator(n)
    , denominator(d) 
    {}

    // Copy
    Fraction(const Fraction& f) 
    : numerator(f.numerator)
    , denominator(f.denominator) { std::cout << "Copied" << std::endl; }
    
    Fraction& operator=(const Fraction& other) {
        std::cout << "Copy assigned" << std::endl;

        numerator = other.numerator;
        denominator = other.denominator;
        
        return *this;
    }
};

int main() {
    Fraction f1(1, 3);
    Fraction f2(f1); // Copied
    Fraction f3(2, 4);
    f3 = f1; // Copy assigned
}
```

I don't really understand why we need to return a `Fraction&`, but this works pretty well at this point. There's another cool thing we can do:

```cpp
class Fraction {
private:
    int numerator;
    int denominator;

public:
    Fraction() {}

    Fraction(int n, int d) 
    : numerator(n)
    , denominator(d) 
    {}

    // Copy
    Fraction(const Fraction& f) = default;
    Fraction& operator=(const Fraction& other) = default;
};

int main() {
    Fraction f1(1, 3);
    Fraction f2(f1);
    
    Fraction f3(2, 4);
    f3 = f1;
}
```
Since `int` is a built-in type, you can rely on your compiler to generate the code for you: the compiler will know that you are declaring a move constructor and a move assignment operator, so the compiler will generate the appropriate code for you. Be aware that if one of your data members has a raw pointer to something, then you'll need to do a bit more work. But right now, using `default` is the way to go if you want clean code, C++ standard also recommends this if you are simply dealing with built-in types and no raw pointers. 

Let's look at the move constructors, one quick and dirty trick is to just mimic what we did above:

```cpp
class Fraction {
private:
    int numerator;
    int denominator;

public:
    Fraction() {}

    Fraction(int n, int d) 
    : numerator(n)
    , denominator(d) 
    {}

    // Copy
    Fraction(const Fraction& f) = default;

    Fraction& operator=(const Fraction& other) = default;

    // Move
    Fraction(Fraction&& f) noexcept = default;
    Fraction& operator=(Fraction&& other) noexcept = default;
};

int main() {
    Fraction f1(1, 3);
    Fraction f2(std::move(f1));

    // f2 = std::move(f1) would also work here
}
```

Notice the `noexcept` above, C++ standards (C.66) suggest that move operations should use `noexcept` whenever possible. Again, I used `default` here because this is simply better. If you can't use it because you are dealing with a legacy interface that has a raw pointer, I personally think you should consider updating that legacy interface to use smart pointers at some point. But in terms of implementing move semantics that involves raw pointers, Klaus Iglberger gave an excellent talk on CppCon 2019, he thoroughly explained everything and I highly recommend you to watch it. It's really easy to follow. 

This is pretty much my take on move semantics. It really isn't too hard, but there are some subtleties that are hard to follow. Personally I think Rust handles this much better with its ownership model. 
