---
layout: post
title: Looking at Abstract Algebra in a rather Mind-boggling Way
published-on: February 16, 2021
---

# Looking at Abstract Algebra in a rather Mind-boggling Way

Probably one of the most interesting mathematics course I have taken during my computer science degree is Abstract Algebra. Interestingly though, computer science students in my university are only required to take a few calculus courses, a few discrete mathematics courses, and a linear algebra course for their lower-division requirements, and Abstract Algebra was not required. I took it because I like mathematics. 

Side note: sometimes computer science is actually considered a branch of applied mathematics. Today we are looking at pure mathematics. 

The first thing to look at is the word "abstract". The word actually has a lot of similarities with the word "abstraction" in the world of computer science. In computer science, we always look for a way to generalize a procedure or an object so that we achieve greater abstraction, the importance is to look at similarities in the procedures and objects and see if repetition can be resolved. In Abstract Algebra, we would look at a way to generalize a bunch of sets of numbers. We will look at this in a bit more detail. 

In Abstract Algebra, every little thing has to be carefully defined: even the definition of addition and multiplication can vary, but let's not worry about that for a moment, let's look at a set of numbers: the natural numbers, from 1, 2, 3, to infinity. 

Sounds simple, right? Unfortunately, here comes the mind-boggling part. 

There is an axiom called "Peano Axiom", and it lays out the algebraic structure of the natural numbers. 

I want you to consider this: the numbers we use: 0 to 9 are essentially just symbols, what do they actually mean? You might say they mean 0 items or 5 items. This would be explaining it from a cardinal sense, we need to look at it in a more "algebraic" sense. 

The first thing about Peano Axiom is that, we define the set of natural numbers to have an initial element: we call it 1. 1 here simply denotes that this is the starting element, it does not say that there is one item. 

The next part is a bit tricky to grasp: instead of having 2 followed by 1, we would have the "successor" of 1, essentially the next element after 1. Denoted by `S(1)`.

With the axiom above, every element in the set of natural numbers would arise: 2 would be `S(1)`, 3 would be `S(2)`, which is `S(S(1))`, and so on. 

Why do we define it like that? With this definition above. We can define addition quite easily. Remember that although we clarified what numbers there are in the set, we haven't defined anything about the operations within this set. 

Now, let's look at how we do something like `a + 1`. We would be looking at the successor of `a`: `S(a)`. `a + 2` would be `S(S(a))`, `a + 3` is `S(S(S(a)))`. and so on. Addition here is defined recursively. 

This kind of gives you a taste of what abstract algebra looks like: we don't care about specific numbers, we do, however, looks at the structure of a system and try to generalize it. An algebraic structure not only includes the numbers themselves, but also the operations within. 

To talk about operations, we first look at the idea of "Groups". 

The set of integers **and** the addition operation is a group. Why? First, for two random numbers in the set of integers, the result of addition between them is also in the set of integers. That is to say: for two numbers `a` and `b` in the set of integers, `a + b` must be in the set of integers. 

For far so good, next we say that the addition operation is associative: for any numbers `a`, `b`, and `c`, `a + (b + c) = (a + b) + c`.

Now here is the interesting part. We say that the element "0" is the zero element, or called the "additive identity". Because for any number `a`, `a + 0 = 0 + a = a`. Additive identity is unique here, there is no second "0". 

Note, however, the number `0` doesn't mean "zero items", it means that it's the additive identity. In some other systems, the additive identity might not even be the number `0`. 

This brings us to the final piece of the puzzle: for every element `a` in the set of integers, we can always find a number such that `a + (-a) = 0`. This means that every element has an inverse such that the sum between one element and its inverse equals to the additive identity. 

These concepts are pretty abstract: 0 is not 0 here, it's the additive identity. What's even more confusing is that sometimes, the operations might be different. 

Consider the power set of `A = {a, b, c}`, this means that we have a set with 2^3 = 8 elements:

1. `{}`
2. `{a}`
3. `{b}`
4. `{c}`
5. `{a, b}`
6. `{b, c}`
7. `{a, c}`
8. `{a, b, c}`

Let's denote this set as `P(A)`. Is this a group? This question cannot be answered, because we haven't defined an operation yet. 

A few paragraphs back I bolded the word "**and**" in "the set of integers **and** the addition operation". Defining an operation is also very important in terms of whether a set is a group or not. 

Let's define our operation as the intersection between two sets, and check whether this set paired with this operation is a group or not. 

First of all, the result of any two sets in the power set intersected belongs in the power set. Second, set intersection is associative. 

Is there a zero element in this set? Certainly we cannot find "0" in the 8 elements. Remember that the definition of a zero element: for any number `a`, `a ?? 0 = 0 ?? a = a`. Here, `??` is the operation we paired with the set, in this case, intersection. So the question becomes: for any element `a`, is there an intersection such that a set `0` has `a ∩ 0 = 0 ∩ a = a`? Turns out we do: it's `{a, b, c}`. This is one of the examples that the zero element might not be the `0` we use in everyday life. 

Finally, let's check if every element has an inverse such that `a ∩ -a = -a ∩ a = {a, b, c}`. The answer, unfortunately, is no. For example, for element `{a}`, we cannot find another element in the above 8 elements such that the intersection becomes `{a, b, c}`. 

Therefore, `(P(A), ∩)` is not a group. 

Abstract algebra isn't just about the numbers, it's about the numbers and the operations, and their properties. Abstract Algebra, in my opinion, is quite a refreshing way to look at our numbers and an abstracted entity. 
