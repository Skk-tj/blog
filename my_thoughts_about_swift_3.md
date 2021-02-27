---
layout: post
title: My Thoughts About Swift - 3
published-on: February 26, 2021
---

# My Thoughts About Swift - 3

This post continues the second part of the series. 

Now we talk about functions, there are some stuff that really confused me when I was first learning Swift. This post will mostly show how I understood those concepts. 

A simple function looks like this:

```swift
func getTimesTwo(number: Int) -> Int {
    return number * 2
}
```

Pretty standard, one nice thing is that it allows you to specify the output type. 

Calling this function would look like this:

```swift
getTimesTwo(number: 13)
```

A weird thing is that you have to include the label ("number: ") when you are calling this function. To allow us to omit that label. We can change the function definition:

```swift
func getTimesTwo(_ number: Int) -> Int {
    return number * 2
}

getTimesTwo(13)
```

This introduces the concepts of "argument label" and "parameter label". We often use argument and parameter interchangeably during programming, but there's a difference in Swift. We can look at the following code:

```swift
func getTimesTwo(original number: Int) -> Int {
    return number * 2
}

getTimesTwo(original: 13)
```

Here, `original` is the argument label. `number` is the parameter label. Parameter label is used inside the function, while the argument label is used when we need to call the function. Swift language guide says:

> The use of argument labels can allow a function to be called in an expressive, sentence-like manner, while still providing a function body that’s readable and clear in intent.

I totally don't think so. For people who learn Swift, they probably already have some programming experience. Looking at this new concept is too confusing. 

By the way, this code would also work:

```swift
func getTimesTwo(original number: Int) -> Int { number * 2 }
```

This is a shorthand that Swift uses. Don't worry, we'll have a lot more shorthands coming. 

Sometimes we may have to do something like swapping two values. Swift, by default, doesn't want you to change the parameters, but if you must, you can use the `inout` keyword.

```swift
func swap(_ a: inout Int, _ b: inout Int) {
    let temp = a
    b = a
    b = temp
}

var someInt = 3
var anotherInt = 107
swap(&someInt, &anotherInt)
```

Surprisingly, the syntax is a lot more like C++. Although, I still prefer Python's one-liner solution:

```python
a, b = b, a
```

Isn't this just beautiful? 

I want to talk about closures next but there are a lot to talk/rant about. I'll leave closures for the next post. So this is just gonna be a short one. 