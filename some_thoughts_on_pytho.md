---
layout: post
title: Some Thoughts on Python Data Structures and Algorithms
published-on: February 15, 2021
---

# Some Python Data Structures and Algorithms

There are some other Python data structures in the `collections` module. I read about them in the book called "Python Cookbook", a great book indeed. These data structures are a bit more different than the lists, dictionaries, and sets we usually deal with. Although lists, dictionaries, and sets are powerful enough for our usual needs, I think it's still useful to know some other ones in case we need a more specialized data structure. 

One data structure that I find interesting is `deque`. Although in theory, a deque is mostly just a list in Python, since popping front and end, inserting front and end can all be achieved by a list. But one interesting application of a deque is to give it a `maxlen` property. It becomes useful when, for example, we need to keep a user's most recent 20 searches. the `maxlen` property essentially indicates how many of the most recent items we shall keep in the deque. 

```python
from collections import deque

history = deque([5, 9, 13], maxlen=3)
print(history)  # deque([5, 9, 13], maxlen=3)

history.appendleft(2)
print(history)  # deque([2, 5, 9], maxlen=3)
```

Apparently, deques are very fast, which makes it ideal for the application above. 

Sometimes we may want to create a mapping from one thing to a group of things. I would personally choose to use a dictionary:

```python
d = {}

d['a'] = []  # ???

d['a'].append(1)
d['a'].append(2)
d['a'].append(3)
```

The thing with the above method is that we have to create an initial value for a mapping, which is probably fine in the short term. In the long run, we probably want something that is more concise. `defaultdict` from `collections` comes to rescue. 

You can initialize `defaultdict` in two ways:

```python
from collections import defaultdict

d1 = defaultdict(list)
d2 = defaultdict(set)
```

the first one is a mapping to a list, the second one is to a set. This way, instead of having to create an initial empty list. You can simply call `append()`, which results in much cleaner code. 

```python
from collections import defaultdict

d = defaultdict(list)

d['a'].append(1)
d['a'].append(2)
d['a'].append(3)

print(d)

print(d['b'])  # []
print(d)  # defaultdict(<class 'list'>, {'a': [1, 2, 3], 'b': []})
# ???
```

As you can probably see above, there are some side-effects. When we are accessing a key that has not yet been mapped to anything, it alters the data structure, so we need to be careful of that. 

The `list` here in `d = defaultdict(list)` is an argument we supplied to `default_factory` in the `defaultdict` class. It defines the behavior when we have a missing key. What essentially happened in the above code block is that: we are first accessing `d['a']`, `d['a']` doesn't exist yet, so the data structure resorts to the `default_factory` argument we supplied. We supplied `list`, therefore an empty list is created. This would also explain why when we are accessing `d['b']`, an empty list is created. Of course, we can also alter that behavior:

```python
from collections import defaultdict

d = defaultdict(lambda: "THIS IS A TEST")

print(d['a'])  # THIS IS A TEST
print(d['b'])  # THIS IS A TEST

print(d)  # {'a': 'THIS IS A TEST', 'b': 'THIS IS A TEST'}
```

Instead of generating a list when a missing key is called, it automatically maps it to the string I supplied in the lambda. Unfortunately, you can't supply the missing key as an argument to the default factory. You'd have to subclass `defaultdict`. Another thing to note is that if you were to supply a lambda, the lambda mustn't have any arguments. However, there are [clever ways](https://docs.python.org/3/library/collections.html#defaultdict-examples) to circumvent that. 

Since we are talking about dictionaries, I think it would be interesting to know that in Python 3.9, we can use `|` operator to merge two dictionaries.

```python
d1 = {'a': 1, 'b': 788}
d2 = {'a': 23, 'c': 11}

d3 = d1 | d2  # {'a': 23, 'b': 788, 'c': 11}
d4 = d2 | d1  # {'a': 1, 'c': 11, 'b': 788}
```

The operator is not commutative. `d3` is very different from `d4`. Whatever that's on the right-hand side overwrites the left-hand side one. 

Switching gears to algorithms, the one I want to talk about is sorting. Sometimes we want to sort a list of stuff lexicographically. For example:

```python
import pprint

l = [
    [12, "Z", 'bb'],
    [1, "D", "xs"],
    [1, "A", "xs"],
    [44, "U", 'lo'],
    [23, "R", 'rt'],
    [12, "A", "bc"],
    [12, "Z", "bd"]
]

l.sort()
pprint.pprint(l)

# [[1, 'A', 'xs'],
#  [1, 'D', 'xs'],
#  [12, 'A', 'bc'],
#  [12, 'Z', 'bb'],
#  [12, 'Z', 'bd'],
#  [23, 'R', 'rt'],
#  [44, 'U', 'lo']]
```

Like that, we don't even have to supply a custom comparison function. Of course, if we, for some reason, want to supply one, we can by defining the `key` argument.

When defining key, we could use lambdas. But sometimes we may want to simply just refer to a list item or a dictionary item. We could then use `itemgetter()` and `attrgetter()` from the `operator` module.

`itemgetter()` is similar to the `[]` operator, while `attrgetter()` is similar to the `.` operator. For example:

```python
import pprint
import operator

l = [
    [12, "Z", 'bb'],
    [1, "D", "xs"],
    [1, "A", "xs"],
    [44, "U", 'lo'],
    [23, "R", 'rt'],
    [12, "A", "bc"],
    [12, "Z", "bd"]
]

l.sort(key=operator.itemgetter(1))
pprint.pprint(l)

# [[1, 'A', 'xs'],
#  [12, 'A', 'bc'],
#  [1, 'D', 'xs'],
#  [23, 'R', 'rt'],
#  [44, 'U', 'lo'],
#  [12, 'Z', 'bb'],
#  [12, 'Z', 'bd']]
```

Now, the lists are sorted by the order of the second element. But here comes the question, how do we sort the elements by the second element, then by the third element, finally by the first element? 


```python
import pprint
import operator

l = [
    [12, "Z", 'bb'],
    [1, "D", "xs"],
    [1, "A", "xs"],
    [44, "U", 'lo'],
    [23, "R", 'rt'],
    [12, "A", "bc"],
    [12, "Z", "bd"]
]

l.sort(key=operator.itemgetter(1, 2, 0))
pprint.pprint(l)

# [[12, 'A', 'bc'],
#  [1, 'A', 'xs'],
#  [1, 'D', 'xs'],
#  [23, 'R', 'rt'],
#  [44, 'U', 'lo'],
#  [12, 'Z', 'bb'],
#  [12, 'Z', 'bd']]
```

Very elegant indeed. 

This happens to remind me of something: I was joking in a Discord server, "I wish someday we could get a natural language programming language", and someone witted: "you mean Python?" 