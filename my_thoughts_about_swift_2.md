---
layout: post
title: My Thoughts About Swift - 2
published-on: February 18, 2021
---

# My Thoughts About Swift - 2

This post continues the first part of the series. 

Last time I was talking about the operators, and I eventually went off the track and talked about how optional is an important concept in Swift. Other than the double question mark operator, there are no other "strange" operators to worry about - these are pretty standard.

Strings are not at all weird in Swift: they are iterable, characters can be accessed via indices, strings are mutable as long as it's a `var`, which is different than Python. 

Similar to Python, there are three types of collections: array, set, and dictionary. Operations within these are quite standard as well. However, one thing we need to note about set is that the element type in the set must be Hashable. Hashable is a protocol within Swift, we will talk about this later. 

So far, Swift has some similarities to Python in terms of basic constructions: control flow, collection types, and operators. 

What is not similar to Python is the addition of switch statements. Even better: Swift does not require you to put a `break` statement at the end of a `case` clause. By default, each clause doesn't fall through.

```swift
let someNumber = 3;

switch (someNumber) {
case 1:
    print("this is one")
case 2:
    print("this is two")
case 3:
    print("this is THREE")
default:
    print("I don't know this number")
}

// this is THREE
```

Swift also provided a backup if you do need to fall through: simply insert the `fallthrough` statement at the end, which is the exact opposite of C++. 

What's cool about Swift is that, unlike C++, you can declare a variable in a `case` clause, so each clause actually works as a scope. C++ would require you to surround the clause with a pair of curly brackets. 

```swift
let someNumber = 3;

switch (someNumber) {
case 1:
    print("this is one")
case 2:
    print("this is two")
case 3:
    let x = 3
    print("this is THREE, \(x)")
default:
    print("I don't know this number")
}

// this is THREE, 3
```

One unique control flow feature about Swift is `guard`. 

```swift
func guardTest(_ cond: Bool) {
    guard cond == true else {
        print("aha.")
        return 
    }

    print("This will execute.")
}

guardTest(false)

// aha.
```

This may seem kind of useless at first, but it turns out to be a very useful way to safely unwrap optionals. The spirit behind guard is that, if `cond == true` evaluates to true, then we continue execution; if not, then we execute the else block, this block must terminate the control of the current scope: meaning that I have to insert a statement that exits this code block: `return`, `continue`, `break`, `throw` will all do the job. 

Let's look at an example:

```swift
func someFunc() {
    var someInt: Int?

    guard let unwrapped = someInt else {
        print("nothing to deal with")
        return
    }

    print("looks fine to me: \(unwrapped)")
}

someFunc()

// nothing to deal with
```

The statement `let unwrapped = someInt` evaluated to false here (not quite sure how), therefore the program transferred control to the following else block, the function is then completed. I thought this is pretty cool. 

This is pretty much it for control flow, strings, and collection type. Next I will be talking about functions and how some of the shorthands confuses me. 
