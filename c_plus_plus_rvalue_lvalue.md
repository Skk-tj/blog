---
layout: post
title: C++ "rvalue" and "lvalue"
published-on: February 22, 2021
---

# C++ "rvalue" and "lvalue"

C++ has some of the weirdest features of a language. Today I wanna talk about "rvalue" and "lvalue". 

"rvalue" stands for "right value", "lvalue" stands for "left value". One way to understand the difference of them is that one is *usually* on the right-hand side of an operator, the other on the other side. Notice the word "usually", because there are a few anomalies. 

Right off the bat, we can see that numerical literals (integers, floats) and booleans (`true`, `false`) are "rvalues", because these are usually on the right-hand side of the equal sign. Look at the number `123` in this example:

```cpp
// you would do this:
int a = 123;

// instead of this:
123 = int a;
```

In contrast, `a` would be an "lvalue", since it usually appears on the left.

Now it's a good time to introduce the concept of "lvalue reference". This works the same as standard reference, but it only works on "lvalues", meaning that:

```cpp
// this would work
int a = 123;
int &b = a;

// this won't
int &a = 123;
```

`b` is an "lvalue reference" to `a`, because `a` is an "lvalue". "123" is an "rvalue", so `int &a = 123;` doesn't make sense.

Why do we need references? Look at the following code:

```cpp
#include <iostream>

int main() {
    int a = 123;
    int &b = a;

    b++;
    std::cout << a << std::endl; // 124
    std::cout << b << std::endl; // 124
}
```

`b` now simply references `a`. Let's compare it with the following code:

```cpp
#include <iostream>

int main() {
    int a = 123;
    int b = a;

    b++;
    std::cout << a << std::endl; // 123
    std::cout << b << std::endl; // 124
}
```

What happened here is that, the content of `a` was copied to `b`, so these two are independent from each other now.

What this implies is that, when we have a large object stored in variable `a`, if we do `VeryLargeObject b = a;`, the content of `a` is copied to `b`, and this is a very slow process. Plus, if we have a function that takes a very large object:

```cpp
void processObject(VeryLargeObject obj) {
    // some code here
}

int main() {
    VeryLargeObject newObj();

    processObject(newObj);
}
```

The `newObj` is first copied at the start of the function, and this could be a bottleneck, so it is preferred to do this:

```cpp
void processObject(VeryLargeObject &obj) {
    // some code here
}

int main() {
    VeryLargeObject newObj();

    processObject(newObj);
}
```

`obj` here is a "lvalue" reference in the function scope, it accepts an lvalue. It's like doing a `VeryLargeObject &obj = newObj` at the start of the function. This is faster because it simply refers to the object instead of doing a copy, as if doing a `VeryLargeObject obj = newObj` at the start of the function, notice the lack of ampersand. This is probably the most common use case of "lvalue reference". 

What if we want to refer to an "rvalue"? Here we introduce the concept of an "rvalue reference".

```cpp
int &&a = 123;
a++;
std::cout << a << std::endl; // 124
```

It behaved exactly as what we wanted it to. 

Let's now look at these two references in the perspective of functions. 

```cpp
void processRef(int& r) {
    r++;
}

int main() {
    processRef(123); // ERROR!
}
```

This would not work. the function `processRef` is expecting `int& r`, which is an "lvalue reference", `processRef(123);` is giving it an "rvalue". We need to change the argument to let it accept an "rvalue reference". 

```cpp
void processRef(int&& r) {
    r++;
}

int main() {
    processRef(123);
}
```

How would this be useful? Imagine this:

```cpp
#include <iostream>
#include <vector>

int getSum(std::vector<int> &&v) { // DOUBLE AMPERSAND!
    int result = 0;

    for (auto i : v) {
        result += i;
    }
  
    return result;
}

int main() {
    std::vector<int> a = {1, 2, 3, 4, 5};

    std::cout << getSum({1, 2, 3, 4, 5}) << std::endl;
    std::cout << getSum(a) << std::endl; // ERROR!
}
```

We can also overload this function so it accepts both references. 

```cpp
int getSum(std::vector<int> &v) { // lvalue reference
    int result = 0;

    for (auto i : v) {
        result += i;
    }
  
    return result;
}

int getSum(std::vector<int> &&v) { // rvalue reference
    int result = 0;

    for (auto i : v) {
        result += i;
    }
  
    return result;
}

int main() {
    std::vector<int> a = {1, 2, 3, 4, 5};

    std::cout << getSum({1, 2, 3, 4, 5}) << std::endl;
    std::cout << getSum(a) << std::endl; // Now it works
}
```

Next time I will be talking about another part of modern C++ that involves "rvalue reference": move semantics. 
