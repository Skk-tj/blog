---
layout: post
title: Python @dataclass and ABC.abc
published-on: May 2nd, 2021
---

# Python @dataclass and ABC.abc

Semester is finally over, this will be a short one talking about some Python features that I learned. 

First of all: `@dataclass`. It's a decorator to a class that reduces boilerplate code by a lot. Let's say that I want to implement a RGB colour class. We might write this:

```python
class RGBColor():
    def __init__(self, r: int, g: int, b: int):
        self.r = r
        self.g = g
        self.b = b
```

That wasn't too much code to write. But let's say that we want to also be able to print out this class for easy debugging. We would add a `__repr__()` function, so we would write: 

```python
class RGBColor():
    def __init__(self, r: int, g: int, b: int):
        self.r = r
        self.g = g
        self.b = b
    
    def __repr__(self):
        return "RGBColor(r = {}, g = {}, b = {})".format(self.r, self.g, self.b)
```

We may also need to write a function that compares two different instances of `RGBColor`, so we would need to write a `__eq__`.

```python
class RGBColor():
    def __init__(self, r: int, g: int, b: int):
        self.r = r
        self.g = g
        self.b = b
    
    def __repr__(self):
        return "RGBColor(r = {}, g = {}, b = {})".format(self.r, self.g, self.b)

    def __eq__(self, other):
        return self.r == other.r and self.g == other.g and self.b == other.b
```

So far, this doesn't seem to bad yet, but we can see this implementation is quickly getting very repetitive. What if we want to implement ordering? We would have to implement `__ne__`, `__lt__` and other functions.  What if we want to make this object hashable? We would have to implement `__hash__`.  What if we want the attribute to be readable only? We would have to write additional `@property` methods. Worst of all, let's say that we also need to implement alpha in the class. We would have to rewrite all the methods to include all the methods to include the additional attribute.

This is when `@dataclass` comes to be useful. Instead of writing all the code above, we can simply write this:

```python
@dataclass
class RGBColor():
    r: int
    g: int
    b: int
```

That's it. 

If we want to have ordering, then we can change the decorator to `@dataclass(order=True)`. If we want all the attribute to be immutable, change to `@dataclass(frozen=True)`

Now, let's talk about `abc.ABC`. For C++ programmers, the concept of abstract class or interface is probably among the first things we learned. What if we want to do this in Python? Enter `abc.ABC`, which stands for "abstract base class". 

Consider the code below:

```python
import abc

class ABMyClass(abc.ABC):
    @abc.abstractmethod
    def a_method():
        pass

a = ABMyClass()  # ERROR!
```

`ABMyClass` is a abstract class with ab abstract method inside, therefore, we cannot instantiate this class. This behaves exactly as we wanted. Let's implement this abstract class:

```python
import abc

class ABMyClass(abc.ABC):
    @abc.abstractmethod
    def a_method():
        pass

class ImplementedABMyClass(ABMyClass):
    def a_method(self):
        print("Success")

a = ImplementedABMyClass()
a.a_method()  # Success
```

A very useful utility for OOP programming in Python.
