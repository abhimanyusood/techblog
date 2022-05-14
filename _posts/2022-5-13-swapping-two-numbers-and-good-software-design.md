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

That is superbly impressive! You have just swapped two numbers without using a temporary variable, which none of your peers were able to do. Your professor will be supremely impressed.

> I guarantee if you're in college, and you write the second solution, you will receive more makrs than all your peers.

> I also guaratee that if you're working in a tech company, and you write the second solution, you will be immediately fired!

Why?

This happens because Universities love cleverness. If you are an intelligent student who can provide a clever solution to a problem, you will immediately become a darling of all your professors.

But the real world, and the tech companies who live in the real world, don't give a shit about cleverness. What they care about is good software design.

Consider this scenario - the swap function you just wrote (the one without using a third variable) is a part of an enterprise product commissioned by a client. Initially, the client specified that the input data of the software would only consist of numbers, since they work in a scientific domain.

But two years later, the client decides to expand into business domain. And now, the input data of their software can either be a number or a string.

> That's what happens in the real world. Requirements change.

What happens when your "clever" swap function gets two strings as the input?

It breaks!

Because you wrote it with the assumption that the inputs will always be numbers.

And at the time of writing the code, you were perfectly justified to do that. After all, those were the exact client specifications. Nobody can fault you for what you did.

And that, ladies and gentlemen, is the fundamental difference between good software design and bad software design.

> Good software design isn't just concerned about what the current specifications are, but also how those specifications might change in the future.

> Good softaware design is all about futureproofing your code.

And the this is, the cleverst solution isn't always the most futureproof!

