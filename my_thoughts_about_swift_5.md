---
layout: post
title: My Thoughts About Swift - 5
published-on: March 13, 2021
---

# My Thoughts About Swift - 5

After being murdered by several annoying assignments, here I am with another post talking about Swift. Last time I talked about closures, this time I'll be jumping to some OOP aspects of Swift. 

Having been used to how classes work in C++, Swift is a little bit different. First, there are Structures and Classes. These are quite similar, both of them allows yuo to define methods. Classes has a little bit more functions, mainly it provides inheritance, also you can declare a destructor for a class. 

Moreover, Structures pass by value, classes pass by reference. This is important in terms of performance. 

Let's talk about properties first, there isn't really anything special about this:

```swift
class Fraction {
    var numerator: Int
    var denominator: Int
}
```

Of course, if you want any property to stay constant, change `var` to `let` does the job. 

There's a special property called lazy property. Imagine we have a class that calculates the area of a circle. this calculator requires very high precision of pi:

```swift
import Foundation

class CircleAreaCalculator {
    lazy var pi = calculatePi()

    func getArea(radius: Double) -> Double { return pi * pow(radius, 2) }
}

let c = CircleAreaCalculator() // pi here is not defined
let a = c.getArea(radius: 12); // Now we need pi, so we calculate pi. 
```

Here, let us assume that `calculatePi()` would take a pretty long time to compute to compute, since we want a crazy amount of precision. `lazy` keyword ensures that the `pi` is initialized only when it's needed, in this case `getArea()` would need `pi`, so this property will init at the last line, not at the second last line. 

There are also getters and setters built in to the classes of Swift. Nothing really to talk about, other than `newValue` in the setter which stands for the value you feed in, this part is pretty easy to understand. Using the `Fraction` class we had:

```swift
class Fraction {
    var numerator: Int = 0
    var denominator: Int = 0

    var inNumber: Double {
        get {
            return Double(numerator) / Double(denominator)
        }
    }
}

var f1 = Fraction()
f1.numerator = 3
f1.denominator = 5
print(f1.inNumber) // 0.6
```

Unfortunately I couldn't really think of a good setter example for now. Also the above example makes `inNumber` read-only. To make it settable, you need to define a setter.

So it's pretty simple. 

Property Observers are very interesting: it gives you a chance to do something before and after a property is set. Let's look at an example:

```swift
class ObserverExample {
    var someNumber: Int = 0 {
        willSet {
            print("I'm gonna change this from \(someNumber) to \(newValue)")
        }

        didSet {
            print("Changed from \(oldValue) to \(someNumber)")
        }
    }
}

var o = ObserverExample()
o.someNumber = 42

// I'm gonna change this from 0 to 42
// Changed from 0 to 42
```

You can use `newValue` in `willSet` and `oldValue` in `didSet`. But not reverse. 

There are some other stuff mentioned in the Swift language reference website such as property wrappers, I'll skip these for now because these are a bit hard to understand. 

We mentioned class methods briefly in the circle example. Next time, I'll talk about methods in detail. 