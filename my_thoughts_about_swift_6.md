---
layout: post
title: My Thoughts About Swift - 6
published-on: March 28, 2021
---

# My Thoughts About Swift - 6

This time, let's talk about class method and subscripting in Swift. 

Methods aren't that interesting to talk about. You define an instance method with the keyword `func`. You can access `self` properties. One interesting thing I observed is that to change a property in a class in Swift, you have to use the keyword `mutating` before `func`. This is pretty much the reverse of C++ where you need to specify `const` for instance methods. This reminds me of how Rust approaches this matter. 

Other stuff is similar to other languages as well, we can define a class method (static method) using `static`, so there isn't much to talk about.

Let's look at subscripting next.

Subscript is like another "magic method" in Swift. Defining a subscript method enables you to use `[]` symbols. Much like the `init` method, you define subscripting as follows:

```swift
class SomeClass {
    subscript(index: Int) -> Int {
        get {
            // Return an appropriate subscript value here.
        }
        set(newValue) {
            // Perform a suitable setting action here.
        }
    }
}
```

Then you can use it like this:

```swift
let s = SomeClass()
print(s[1])
```

Of course, if it is required, we can also define multiple input parameters:

```swift
class Matrix {
    // properties required for a matrix

    init() {
        // setting up a matrix
    }

    subscript(row: Int, column: Int) -> Int {
        get {
            // get value at specified row column
        }
        set(newValue) {
            // set the new value at specified row column
        }
    }
}
```

Then, we can use it like this:

```swift
let m = Matrix()
print(m[2, 3]) // print the value at row 2 column 3
```

Of course, you can also use the `static` keyword before the `subscript` declaration to indicate that this is a static method inside a enum. 

Inheritance and overriding is very similar to other languages as well, you can override a base class like this:

```swift
class Circle: Shapes {
    // class def
}
```

To override a method, simply add the keyword `override` before `func`. One convenient feature in Swift is that you can also override properties, such as getters and setters, and property observers.

```swift
class Circle: Shapes {
    override var perimeter: Double {
        didSet {
            // overriding method for didSet
        }

        willSet {
            // overriding method for willSet
        }
    }

    override var area: Double { // getter here
        return 2 * pi * radius
    }
}
```