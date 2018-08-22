---
layout: post
title: An off-beat approach to understanding factory design patterns!
published: true
---

I had no idea what factory patterns were. So I did what any sane coder would have done. I decided to google the shit out of it. After spending countless hours in front of my laptop screen, reading article after article, tutorial after tutorial - bang! I still had no idea what the fuck factory patterns were. (Notice the extra fuck, it’s not there for no reason)

The technical definitions made no sense to me. How the hell did it matter if factory method was using inheritance while abstract factory was using composition? They looked fucking same to me! And how does understanding any of this help me in figuring out how and where to use them? It didn’t! Far from taking me closer to an intimate understanding of these beautiful and useful design patterns, my research was sucking me deeper and deeper into a vortex of confusion and chaos.

So, I thought fuck it! 

Let’s do this my way. By starting with directly applying these design patterns to real life problems -  building stuff -  and letting that process lead to a deeper insight into what these patterns meant, and how and why they mattered at all..

And it worked!

What follows is a distilled version of my understanding of the three factory design patterns. This article lacks technical rigor, I know. But this was how I learnt. And if you’re anything like me, this is the only way you are going to learn.

So, without further ado, let’s begin our journey, straight with a practical example.

Consider the following code -

```php
class car
{
	getCar()
	{
		return “car”;
	}
}
```


