---
layout: post
title: Understanding Rust and its ownership model
published-on: March 06, 2021
---

A few days ago I had an opportunity to learn the Rust programming language. At first, I thought this is simply another language with a different syntax. Turns our Rust has this entire ownership/borrowing model that kind of confused me at first, but eventually allowed me to realize how safe Rust is. 

Rust introduced an ownership model that works a lot like C++'s reference mechanism, but there are still some interesting quirks. The `String` type in Rust, does not copy, but in C++, it does:

```cpp
// C++
#include <iostream>

int main() {
  std::string s1 = "This is a test";
  auto s2 = s1;

  s1.append(", another test");

  std::cout << s1 << std::endl; // This is a test, another test
  std::cout << s2 << std::endl; // This is a test
}
```

In Rust, this is a little bit different. 

```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1;

    println!("{}, world!", s1);
}
```

Here, `s1`'s content is moved to `s2`, therefore referencing `s1` will not work. This behavior might be a little bit perplexing. Rust's explanation is that `String` does not implement `Copy`, copying could also be very expensive. 

Stack data are sequential, which means that can be copied and it's quick, such as integers. These data have fixed length in memory. Strings, however, can be expanded when needed, so the data is stored in the heap. Copying data can be expensive in the heap since the memory management software needs to search for available space in the heap. 

Now, let's look at how the ownership model is represented when we are dealing with resources that need to travel between different scopes. 

A few posts ago I talked about how using references in a function signature could change the behavior. In Rust, things are similar but ultimately a bit different. 

In C++, we can do the following:

```cpp
#include <iostream>

void print_string(std::string s) {
    std::cout << s << std::endl;
}

int main() {
    std::string s = "This is a test";
    print_string(s); // This is a test

    std::cout << s.length() << std::endl; // 14
}
```

Interestingly, in Rust, this happens:

```rust
fn main() {
    let s1 = String::from("hello, world");
    print_string(s1); // value moved here

    println!("{}", s1.chars().count()); // ERROR!
}

fn print_string(s: String) {
    println!("{}", s);
}
```

Gee! What happened? Recall that the `String` type in Rust does not copy by default, so it must move. `s1` essentially "moved" into the scope of `print_string`, then at the end of the `print_string` scope, what `s` is referring to is dropped, and the respective memory is freed. Therefore, `s1` is no longer valid in the `main()` scope. Its ownership moved into the `print_string` scope, `s1` in `main()` no longer owns anything. 

If you are familiar with C++'s RAII model, you might recall that a `std::unique_ptr` cannot be directly passed into a function, you must "move" the unique pointer instead.

```c++
#include <memory>
#include <iostream>

void do_things(std::unique_ptr<int> int_ptr) {
    std::cout << *int_ptr << std::endl;
}

int main() {
    std::unique_ptr<int> a(new int(23));
    // INSTEAD OF THIS:
    // do_things(a);

    // DO THIS:
    do_things(std::move(a));

    std::cout << (a == nullptr) << std::endl; // 1 -> true
}
```

In C++, we must explicitly specify that I want to transfer the control of the resource to another scope. What I meant by this is that, first, declaring this unique pointer specifies that `a` takes control of this memory resource that has the number "23". `do_things(std::move(a));` means that, with the use of `std::move()`, `a` must cease control of the resource and let `int_ptr` to control this resource. This "control of resource" and "ownership" are quite similar.

One thing you should know that I could still copy a string:

```rust
fn main() {
    let s1 = String::from("hello, world");
    print_string(s1.clone()); // value CLONED here

    println!("{}", s1.chars().count()); // OK
}

fn print_string(s: String) {
    println!("{}", s);
}
```

The content of `s1.clone()` is essentially a different piece of memory resource than what `s1` controls. `s1` isn't moved anywhere, so using `s1` after the `print_string` function would work. 

This part covers the ownership aspect of Rust, next I will be talking about borrowing and referencing in Rust. 
