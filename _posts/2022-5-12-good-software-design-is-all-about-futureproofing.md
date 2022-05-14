---
layout: post
title: Good Software Design is all about Futureproofing
published: true
---

"Good software design" is a pretty overloaded term. It might mean a lot of things in a lot of contexts. Off the top of my head, here are some of the things I associate with good software design - 

1. Design Patterns
2. SOLID Design Principles
3. Interfaces and Ploymorphism
4. Architecture
5. Writing scalable, extendable, readable, and maintainable code

etc.

Every single one of these topics appears to be compeltely distinct from the others, and this diversity might prove detimental for the beginners who are trying to understand or visualize -  what exactly is meant my good software design?

That's where I come in.

I will give you a mantra that distils the essence of good software design, and unifies all the diverse subtopics under a single theme - 

> Good Software Design just means Futureproofing your code!

1. **Design Patterns and Principles** - They are just ways of writing your code in such a way that if in future, the client raises a new requirement, you should be able to incorporate it without disturbing the existing structure of your code.
2. **Interfaces and Polymorphism** - They allow you to decouple your code from specific implementations of the classes so that new classes can be added in the future without modifying the existing architecture of your codebase.
3. **Scalability** - In future, the number of users will increase. Will your code break when the number of users will go beyond a certain threshold?
4. **Readability** - In future, you might have to hand over your codebase to someone else. Or you might continue to work on it, but since human memory is fickle, your understanding of the codebase you yourself wrote might start to fade away. What do you do then?
5. **Extendibility** - In future, the client comes up with a new requirement that was unthought of when the initial codebase was being written. Would you be able to accomodate this new requirement without overhauling the architecture, or worse still, forking the entire repository?

**You see, the common theme here is Futureproofing!**

You will no doubt study and perfect each of these topics as you journey deeper into this world. But you don't need to be a master of all these topics straightaway to be a good architect.

To be a good architect and software designer, all you need to do is this - As you're writing your codebase, just think of all the ways in which your code can break in the future. And then try to futureproof your code against all those contingencies.

That is the underlying theme of good software design.
