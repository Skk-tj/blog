---
layout: post
title: My Thoughts About Swift - 1
published-on: February 15, 2021
---

# My Thoughts About Swift - 1

This is probably gonna be a multi-part series, but I'll be talking about my feeling about Swift in this series. 

Swift is a protocol-oriented language. I personally have never heard of this type of programming paradigm. But the word hints that instead of being object-oriented like Java heavily does rely on, it focuses more on functionality and extensibility. So I think it's important for us to adopt a fresh new perspective when approaching this language.

Although Swift is a protocol-oriented, this doesn't mean that it does not have object-oriented features. In fact, most programming languages adopt many programming paradigms. 

Swift also boasts itself for being safe. We will look at this in more detail later. 

Swift is very flexible: unlike C++ and much like Python, you can choose to type hint your variables. 

One thing that confuses a lot of people about Swift is the use of optionals. Optionals is a way to indicate that a variable might not have a value yet. This may seem like a simple concept, but what comes after: optional binding is a little bit more confusing. 

In languages like C++, you can declare a variable without giving it an initial value, and the variable is simply gonna have a default value:

```cpp
#include <iostream>

int main() {
    int noInitial;

    std::cout << noInitial << std::endl; // prints 0;
}
```
It's a little bit different in Swift:

```swift
var op: Int?

print("\(op)") // nil
```

The variable `op` here has a completely different type than `Int`: it's `Int?`. A good way to look at this is to consider `Int?` as an object that wraps around an `Int`. Here we introduce the concept of optional unwrapping. 

The Swift code above doesn't just outputs `nil`. There are a lot more warnings in the output. To resolve them, we have to "unwrap" the `Int?`

```swift
var op: Int?

if let actual_op = op {
    print("\(actual_op)")
} else {
    print("No value yet. ")
}

// prints: No value yet. 
```

Some people might find the above code very confusing: essentially what we are doing here is: if `op` has a good value (i.e. not nil), let's put this into the `actual_op`, and do stuff with it. If not, we print out something else, or we could give a soft error in an actual application. 

So if we do:

```swift
var op: Int? = 1001001;

if let actual_op = op {
    print("\(actual_op)")
} else {
    print("No value yet. ")
}

// prints: 1001001
```
As far as GUI-based application goes, I think this is a pretty nice feature: it essentially diverts a lot of empty value errors to the compiling stage. It also encourages the developers to write much safer code. 

Another thing that I found weird about Swift is that it has a "Unary Plus" operator, which doesn't seem to do anything at all: 

```swift
var six: Int = 6
print(+six)

six = -6
print(+six)
```

The [official documentation](https://docs.swift.org/swift-book/LanguageGuide/BasicOperators.html#ID63) says it provides "symmetry in your code for positive numbers when also using the unary minus operator for negative numbers". Whatever that means.

A few paragraphs ago we talked about optionals, and we had a rather lengthy code if we simply want to unwrap an optional. Fortunately in Swift, there are many shorthands (syntactic sugar) to complete the same task. One such example is the double question mark operator.

```swift
var six: Int?

print(six ?? "I see no number") 

// prints: I see no number
```

If `six` is `nil`, then we use the right-hand side of the operator, if not, then we unwrap the variable. It's a much nicer way if we just want to display a string on a UI element, comparing to the optional unwrapping, which could be used for popping an error or some other more complication UI tasks when a variable is empty. 

I think right now is a good point to explain why we need these mechanisms. Unexpected things happen a lot, let it empty API call returns, or malformed user inputs. We also would want a user's experience to be "peaceful", i.e. lest we want the program to suddenly exit. Optionals become extremely useful in these scenarios. 

Say, we make an API call:

```swift
// note: Swift pseudo-code

data, error, status = make_call("https://somewebsite.com/v1/someapi?myname=aaa");
```

Sometimes, the `data` could be empty because the website is down, and sometimes `error` could be empty because the call correctly returned everything. Imagine you wrote this UI code and we don't have optional unwrapping:

```swift
// SwiftUI code

struct MyView : some View {
    Text(data.name)
}
```

The UI code above generates a text field and display the `name` portion to the user. If `data` is empty, then we are in big trouble. but we can do this, can't we?

```swift
// SwiftUI code

struct MyView : some View {
    if (data == nil) {
        Text("Can't display, something went wrong. ")
    } else {
        Text(data.name)
    }
}
```

That's a lot of stuff to write, plus, checking whether a variable is empty is rather not safe (I can't quite articulate why I think it's not safe). In a world with optionals, things will be much shorter and more concise. 

This concludes the first part of my thoughts on Swift. 
