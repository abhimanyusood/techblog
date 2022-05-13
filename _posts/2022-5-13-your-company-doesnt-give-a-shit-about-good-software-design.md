---
layout: post
title: Your company doesn't give a shit about good software design
published: true
---

Interfaces, Polymorphism, Design Patterns, Architecture, SOLID Design principles, writing modular, extendable, and scalable code - why do we care so much about all this stuff? 

**Because we think that good software design matters.** In our minds, it is what distinguishes mediocre software engineers from great ones. And being recognized as a great software engineer within your company will lead to both fame and fortune.

> Newsflash - Your company doesn't give a shit about good software design 

## My initial delusions

When I joined the tech world as a starry-eyed fresher, I was immediately bewildered to see that most of the engineers around me were extremely mediocre. Sure they could get a new project up and running, but their codebases resembled a bowl of freshly baked spaghetti with zero thought put into the scalability, extendability, or good architecture and design.

> Surely, this charade couldn't continue for long? Surely, one day they would get caught - their mediocrity exposed to the entire world? Right?

And, by contrast, since I put extensive thought into the design and architecuture of the code I was writing, my greatness would surely be discovered and acknowledged by the whole company one day. Right?

I wouldn't lie - **that was my biggest fantasy during my first few years in the tech industry.**

But five years later, no such thing has happened. The mediocre engineers still continue to work at the same company, having faced absolutely no repurcussions for their mediocre code. And I too work for the same company, having received zero recognition for the extensive love and care I put into the design of my codebases.

It's as if the company doesn't care!

> I repeat - 80% of the companies don't give a shit about good software design!

## But why?

The *bureaucratic masters* who run the company have no idea about code, much less good code. All they care about is having a working software product that they can sell to the clients. As long as the product is in working state, and thereby sellable, they couldn't care less the underlying code. 

A mediocre code writes poorly designed codebase that is not modular, not extendable, and not scalable. **But it works**, especially in the starting phase when the total number of users is less than a hundred. It is sellabe. That last part is all the bureaucratic masters care about. They are happy. No repurcussions for the mediocre engineer.

*"Abhimanyu, I get that the consequences of bad software design are not immediately visible. But won't they manifest themselves in the long run? Take scalability, for example. When the number of users rise to a hundred thousand, wouldn't the product eventually break down, exposing the mediocrity of the engineer behind it?"*

#### No.

1. If a company gets to a hundred thousand users, it would have made a shit ton of money by then. And when scalability issues start to come up, the bureaucratic masters will just initiate a new project solely targeted at scaling the product, treating it as **new development**. They would just write new project off as another business expense!
2. This expenditure is acceptabe to the bureaucratic masters because they don't have enough technical accumen to understand or realize that the product could have been written in a scalable manner in the first place thereby making this new project completely unnecessary, if only they had hired better software engineers who know and care about good software design. This thought wouldn't even occur to them. In their minds, spending money on a new project that improving scalability of a the old, or if you prefer the fancy term, 'legacy' product, is completely acceptable. In fact, in their minds, this is the norm.

And so they would put the same mediocre engineer to work on this new project. Behind the scenes, these mediocre engineers would be creating ugly patchworks to get minor improvements in scale. But these minor improvements would be hailed as great leaps, and the bureaucratic masters would pat the engineers, as well as themselves on the back.

The codebase is pretty much spaghetti and a nightmare to maintain? No problem. Just throw more engineers into the team and divide the extra work between them. 

A new client comes demanding a small customization in one of the features. No problem. Just ask the engineers to branch the entire repository just for that one client.

The possibility that the codebase could have been written in such a manner that it would have completely avoided all these problems in the first place, doesn't even occur to the bureaucratic masters.

## A 20 GB sql table

One of my friends worked for a company where five years ago, a mediocre architect took the decision to store all their static image and video files in the sql db (as base64 encoded blobs)

The size of that single table currently is over 20 GB

Three years later, when scalability issues started to pop up, another mediocre architect came up with a brilliant solution. Let's introduct redis into the mix and just cache the **ENTIRE** database on redis. 

The best part is, the bureaucratic masters don't care. In their minds, it fixed the problem.

Two years later, which is now, their sins have finally caught up to them. The cache size has run into hundreds of GB's. The product is excruciatingly slow.

Are you wondering what would the bureaucratic masters do now?

Don't worry, they have already made a decision. They have decided to rewrite the entire original product (which they are now calling 'legacy' product) with moden tech stack. They have given this new project a hip sounding name and allocated a handsome budget for it. 

There's a flip side to this as well.

## What if you are a great engineer in such a company - one who knows and cares about good software design?

I'm sure you can extrapolate.

You will spend a few extra days coming up with the best possible architecture for the product you've been asked to build. And since you've written your codebase to be fully modular, extendable, and scalable, it would run smoothly even in the long run. But the bureaucratic masters don't have enough technical accumen to understand or realize that the product is running smoothly because you built it that way in the first place. To them, running smootly is the default behavior. No credit to you. 

In fact, the more effort you put into good design, the more smoothly it runs. And the more smoothly it runs, the less attention anyone pays to your product, or you.

On the other hand, mediocre engineers build bad products. Bad products break. When they break, the same mediocre engineer swoops in to fix the "critical" issue that was affecting "client delivery." The fixer is hailed as a hero for saving the day, while realizes that the so called "critical" issue arose in the first place due to the poor design written by the same mediocre engineer!

Remember the few extra days you spent polishing the architecture of your codebase? Beware! That might earn you the tag of a "slow-coder"!

## Are all companies like this?

What I have written above is true for 80% of the comapnies. Who are the other 20%?

1. Companies where there is a very strong tech culture. In such an environment, your peers will realize and acknowledge your capability as a good architect. Not only that, your tech leads would communicate the same to the bureaucratic masters as well.
2. Companies where the consequences of bad software design and architecture manifest **immediately**. Consider Netflix for example. If you write poor architecture for some service, the service will go down immediately! Such companies are forced to hire the best architecture almost as a necessity.

But for all the other companies, good software design doesn't matter.

And that is the saddest reality of working in this industry.
