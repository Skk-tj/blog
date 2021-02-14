---
layout: post
title: C++ is more than what I thought
published-on: February 14, 2021
---

# C++ is more than what I thought

A while ago I have a few interviews with some companies as a co-op student. Some of the positions I applied for are C++ programming position. I have to say, these interviews really opened my eyes about C++. 

A little bit of background: I have just entered third third year of my Computing Science degree. Usually in the first two years, the university would normally teach you a lot of theoretical stuff: calculus, sorting, searching, trees, and graphs. Lots of these concepts are very abstract and mathematical. At least in my university, they don't teach you basic software development stuff until you are halfway through your second-year. 

The "basic" software development knowledge they teach you happens to be incredibly basic. They teach you how to use Git, how to work in a group, and so on. These basic knowledge tend to disappoint your employers: they wanted much more when you apply for a co-op. I, as a co-op student, have disappointed a lot of my interviewers on that ground, especially my C++ skills. 

I used C++ extensively in my first two years of my degree. I mostly used this amazing language in my data structure class and my compiler class. I initially thought that I must be very experienced in C++, right? Hell, I even wrote myself a compiler in C++, what else can't I do? 

As you have probably figured out, I'm more wrong than ever ("more wrong", huh). 

C++ has a lot of modern features that most Computer Science students don't know about. For instance, when I was in my compiler class, one assignment was to use LLVM to translate ASTs to LLVM interim code, so manipulating pointers of AST objects was unavoidable. After we turned in this assignment, our TA kindly told us about the existence of "smart pointers": `std::unique_ptr`, `std::shared_ptr`, or as "the industry" would like to call: RAII. 

RAII: Resource Acquisition is Initialization, is considered as a safe-guard feature of C++ by many of us. We make use of `std::unique_ptr` and `std::shared_ptr` to make sure that a certain is resource available at the right time. It also guarantees that the resources are properly released when it is no longer in use. Note, however, "resource" doesn't just mean memory resource. We could also be talking about processing resources (`std::lock_guard`). This function essentially prevents some of the null pointers errors but prompting them at compile time. 

More specifically, `std::unique_ptr` restricts the ownership of a piece memory resource. Transferring control is not allowed.

```cpp
auto p = make_unique<int[]>(10);
auto p_copy = p; // ERROR!!

some_function(p); // ERROR!!
```

It would also restrict you from passing a smart pointer to a function because transferring control to another function (scope) is not allowed. 

It wasn't until the third year of my university have I noticed this nice feature. Imagine the number of hours of more sleep I will get had I known it so I don't have to painstakingly read every line of code of my data structure assignment. Everyone hates those evil "Segmentation fault", two words, zero information, hours of bug-finding. But guess what? Even if I had known this feature, I wouldn't have been allowed to use it. 

As many of us would perhaps attest to, my university is a strange place. Don't get me wrong here, I'm not laying blames on my university for not teaching us this - we, as students, should take the initiative and teach ourselves. On the other hand, knowing the basics: like using pointers, releasing the memory, is just as important as knowing the convenience part. 

Another interesting but less significant modern feature of C++ is the keyword `auto`, it essentially saves you a little bit of time from writing a long type name. For example:

```cpp
// long
for (std::vector<int>::iterator i = v.begin(); i != v.end(); ++i) {
    std::cout << *i << std::endl;
}

// much nicer
for (auto i = v.begin(); i != v.end(); ++i) {
    std::cout << *i << std::endl;
}

// or even
for (auto i : v) {
    std::cout << i << std::endl;
}
```

In terms of a more abstract concept in OOP, there are "rule of three/five/zero". In essence, when an object is set to acquire a piece of resource (either memory or computation) of the computer, some member functions in a class must be defined. Rule of three suggests that there are three member functions you should define: 

1. destructor that properly releases the resource
2. copy constructor
3. copy assignment operator

Rule of five simply includes two more functions, this applies from C++11 and on:

1. Move constructor
2. Move assignment operator

Another concept I learnt was inversion of control, I'll talk about this a bit later, since it's a bit lengthy and has nothing to do with managing resources. 

My operating system class taught me that managing resource is a difficult task in general. From these lessons here, I learned that C++ is great at managing resources of a computer efficiently. Also, I was able to learn these concepts thanks to these interviews. The co-op experience does help with your learning, whether you failed an interview or not. 