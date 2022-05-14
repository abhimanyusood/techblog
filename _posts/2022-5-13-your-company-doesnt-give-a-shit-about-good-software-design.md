---
layout: post
title: Do tech companies “really” care about good software design?
published: true
---

I mean, they have to, don’t they?

After all, good software design results in great products, which translates into more profit for the company.

That’s why the best engineers spend years studying this stuff and perfecting their craft. Design patterns, good architecture, writing modular, extendable, and scalable code — these form the core part of our arsenal.

Isn’t this what makes us great at our job? And being acknowledged as a great software engineer in our company would lead to both recognition and money…

> Newsflash — Most tech companies don’t give a fuck about good software design!

## A Fresher’s Delusion
When I joined the tech world as a starry-eyed fresher, I was immediately bewildered to see that most of the engineers around me were extremely mediocre.

Sure they could get a new project up and running, but their codebases resembled a bowl of freshly baked spaghetti with zero thought put into the scalability, extendibility, or good design.

> Surely their charade wouldn’t continue for long, right? Surely one day these mediocre engineers would get caught — their mediocrity laid bare for the entire company to see…

And by extension, since I put extensive care into the design and architecture, wouldn’t my greatness one day be discovered and acknowledged by the whole company?

**This was my biggest fantasy during my first few years in the tech industry.**

## The Bitter Reality

But five years later, no such thing has happened.

The mediocre engineers still continue to work at the same company, having faced absolutely no repercussions for their mediocre code.

And, I too work at the same company, having received zero recognition for the extensive love and care I put into the design and architecture of my codebases.

## Companies Are Run By Bureaucrats
The **bureaucratic masters** who run the company have no idea about code, much less good code. All they care about is having a working software product that they can sell to the clients. As long as the product is in working state, and thereby sellable, they couldn’t care less about the underlying code.

A mediocre engineer writes poorly designed codebase that is not modular, extendable, or scalable.

**But it works!**

Especially in the starting phase when the total number of users is less than thousand.

**It is sellable.**

> And that last part is all the bureaucratic masters care about.

## What About Scalability Issues?
*“Abhimanyu, I get that the consequences of bad software design are not immediately visible. But won’t they manifest themselves in the long run? Take scalability, for example. When the number of users rise to a hundred thousand, wouldn’t the product eventually break, exposing the mediocrity of the engineer behind it?”*

If a company gets to hundred thousand users, it would have made a shit ton of money by then.

And when scalability issues start to come up, the bureaucratic masters will just initiate a “new development exercise”, or perhaps, a “brand new project” altogether, targeted at scaling the product.

> They would write off this new project as just another business expense.

> This expenditure is acceptable to the bureaucratic masters because they don’t have enough technical acumen to understand or realize that the product could have been written in a scalable manner in the first place, thereby making this new development exercise completely unnecessary.

This thought wouldn’t even occur to them.

In their minds, spending money on a new development exercise that improves scalability of the old — or if you prefer the fancy term — ‘legacy’ product is a perfectly acceptable, and perhaps, the only possible way to deal with the situation.

## Other Examples?
***The codebase is pretty much spaghetti and a nightmare to maintain?***

No problem. Just throw more engineers into the team and divide the extra work between them.

***A new client comes demanding a small customization in one of the features but the architecture is not extendable?***

No problem. Just ask the engineers to branch the entire repository just for that one client.

The possibility that the codebase could have been written in such a manner that it would have completely avoided all these problems in the first place, doesn’t even occur to the bureaucratic masters.

## Story Of A 20-GB SQL Table
One of my friends worked for a company where five years ago, a mediocre architect took the decision to store all their static image and video files in a new SQL table as base64 encoded blobs.

Three years later, when size of that table reached 20 GB and scalability issues started to core up, another mediocre architect came up with a ***brilliant*** solution — let’s just cache the **ENTIRE** database on redis!

The best part is, the bureaucratic masters didn’t care. In their minds, caching fixed the problem.

Two years later, i.e. today, their sins have finally caught up to them. The cache size has grown into hundreds of GB’s. Their app has become excruciatingly slow.

***Are you wondering how are the bureaucratic masters going to deal with it?***

Don’t worry, they have already made a decision.

> They have decided to rewrite the entire original product (which they are now calling ‘legacy’ product) with modern tech stack.

They have given this new project a hip sounding name and allocated a handsome budget for it.

## What Happens To Good Engineers?
What if you are a great engineer in such a company — one who knows and cares about good software design?

I’m sure you can extrapolate.

You will spend a few extra days coming up with the best possible architecture for the product you’ve been asked to build.

And since you’ll write your codebase to be fully modular, extendable, and scalable, it would run smoothly, even in the long run.

But the bureaucratic masters don’t have enough technical acumen to understand or realize that the product is running smoothly because you built it that way in the first place. To them, running smoothly is the default behavior. No credit to you.

> In fact, the more effort you put into good design, the more smoothly it runs. And the more smoothly it runs, the less attention anyone pays to your product, or you.

On the other hand, mediocre engineers build bad products.

Bad products eventually break.

When they break, the same mediocre engineer swoops in to ***“fix”*** the ***“critical”*** issue that was affecting ***“client delivery”***.

Suddenly, the mediocre engineer is hailed as a hero for saving the day, while nobody realizes that the so called critical issue arose in the first place due to the poor design written by the same mediocre engineer!

And remember the few extra days you spent polishing the architecture of your codebase?

> Congratulations. You’ve now earned the tag of a “slow-coder”!

## Are All Companies Like This?
Absolutely not.

There are two kinds of companies that do give a shit about good software design —

Companies where there is a very strong tech culture. In such an environment, your peers will realize and acknowledge your capability as a good architect. Not only that, your tech leads would communicate the same to the bureaucratic masters too.
Companies where the consequences of bad software design and architecture manifest immediately. Consider Netflix, for example. A Netflix engineer has to think about good design constantly, else the users would start facing streaming issues ***immediately***! Such companies are forced to hire the best architects as a necessity.
But I suspect that companies like this constitute only a tiny minority of the tech world.

The rest of them absolutely don’t give a shit about good software design!

And that is the saddest reality of working in tech industry.
