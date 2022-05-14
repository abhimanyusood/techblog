---
layout: post
title: Swapping two numbers and Good Software Design
published: true
---

When you're in college, you will, one day, without exception, be asked this question - 

> Swap two numbers, a and b

Here are two ways you can answer this question - 

If you are a basic bitch, you'll do what every other basic bitch will do - 

```python
c = a
a = b
b = c
```

But if you are an advanced bitch who is exceptionally intelligent, you'll do this - 

```python
a = a+b
b = a-b
a = a-b
```

That is superbly impressive! You have just swapped two numbers without using a temporary variable, which none of your peers who let's face it, are all basic bitches were able to do. Your prof will be supremely impressed.

> I guarantee if you're in college and you gave the second solution, you will receive more makrs than all your peers.

> I also guaratee that if you were working in a tech company and you gave the second solution, you will be immediately fired!

Why?

