---
layout: post
title: My Thoughts About Swift - 4
published-on: February 28, 2021
---

# My Thoughts About Swift - 4

This post continues the third part of the series. 

This time we are gonna talk about closures. Closures are similar to lambdas in Python. Not exactly sure why they call them "closures". 

First few things are pretty standard across the board: you can pass in the name of a function to a function:

```swift
func someProcedure() {
    // code here
}

// then use it like...
anotherFunction(useClosure: someProcedure)
```

Like JavaScript, we can also write the procedure inline. The syntax, however, is a little bit weird. Worse, confusion piles up quickly too.

The official guide of Swift provided an example of using a closure in the `sorted()` function:

```swift
reversedSorted = names.sorted(by: { (s1: String, s2: String) -> Bool in
    return s1 > s2
})
```

The first confusion I had is the use of word `in`. This is a rather strange word to use, but whatever. This doesn't seem so bad, but let's try simplify that. 

Sometimes, Swift can infer the type from the definition requirements of `by`, this means that we don't really have to include the type when we are using closures:

```swift
reversedSorted = names.sorted(by: { (s1, s2) in return s1 > s2 })
```

Last time we looked at functions and there was an example of implicit return, we can also use it here.

```swift
reversedSorted = names.sorted(by: { (s1, s2) in s1 > s2 })
```

Now it *really* looks simplified. It's a cool thing. But Swift allows you to simplify it further. You see, `s1` and `s2` are just some names we gave to represent two elements in comparison. So we can get around that by doing something like this:

```swift
reversedSorted = names.sorted(by: { $0 > $1 })
```

`$0` and `$1` means the first and second argument. We can look at the (documentation)[https://developer.apple.com/documentation/swift/array/2296815-sorted] to know how many arguments we can use. In this case, we have `(Element, Element)`, so two. 

Hell, we can even simplify it further. If there are two, then we don't even need `$0` and `$1`:

```swift
reversedSorted = names.sorted(by: >)
```

These are called operator methods, pretty similar to C++'s `Obj operator+(Obj lhs, Obj rhs);`.

I know right, this simplification is madness. But here is another one: trailing closures. 

If the last argument of the function you want to call is a closure, then you can write it like this:

```swift
// instead of this...
reversedSorted = names.sorted(by: { $0 > $1 })

// do this...
reversedSorted = names.sorted() { $0 > $1 }
```

I don't know why but I really don't like this. By my intuition, a block that is surrounded by a set of curly brackets means a definition of procedures, so a `{ $0 > $1 }` block that is syntactically treated as a parameter of a function would mean to apply this procedure inside this function call. But the trailing closure simply looks like we are defining the `names.sorted()` method, which doesn't look straightforward at all. Maybe I'm biased towards the JavaScript syntax, but my intuition apparently doesn't want to work with Swift. 

Closures are extensively used in asynchronous operations, when building a user-oriented application, using asynchronous procedures is a must. 

There are other concepts such as value capturing and escaping closures. These concepts are more advanced, therefore I won't talk about it here. 

This concludes my ranting about closures. 