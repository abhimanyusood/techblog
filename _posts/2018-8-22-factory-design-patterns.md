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
		return "car";
	}
}
```

This is a simple car object that returns a "car" (I’d love to own an actual Lamborghini some day, but for now, I’ll pretend that the string "car" is the car itself)

Now suppose, we get an additional requirement - the system will receive an additional input $type, and that particular model of car has to be returned.

```php
class car
{
	getCar($type)
	{
		if($type=="sedan")
		{
			return "sedan";
		}
		else if($type=="suv")
		{
			return "suv";
		}
	}
}
```

Simple enough, isn’t it?

There’s just one problem - this code is not extendable. Plus it violates **open-close principle** _(software entities like classes, methods etc must be open for extension but closed for modification)_

A better solution - SIMPLE FACTORY

1. Each "car" ("sedan","suv") is defined in a separate class of its own (car_suv.php, car_sedan.php)
2. A separate class called CarFactory instantiates one of these classes depending on the $type it receives as input
3. The CarFactory may do the instantiation via - 
* Dynamic loading of class based on $type (new "car_".$type)
* If-else condition (if($type=="suv")new car_suv())

```php
class car_suv implements car
{
	getCar()
	{
		return "suv";
	}
}

class car_sedan implements car
{
	getCar()
	{
		return "sedan";
	}
}

class CarFactory()
{
	buildCar($type)
	{
		$car =  new "car_".$type;
		return $car->getCar();
	}
}
```

Now, you may wonder, in real world, car_sedan and car_suv classes will have almost all the methods and members common (4 doors, 4 wheels, 1 steering) and only a few differences (stick vs. automatic). There would be a lot of duplicate code. 

This brings us to our fourth point -

  4 All the products (cars in this case) implement a single interface (or abstract class)
  ```php
  class car
  {
      getCar();
  }
  ```

If, at this point, you’re saying to yourself - “I can see the utility of using an abstract class here - to cram all the common methods together - but what the fuck is the use of an interface? It’s not as if interface can house any methods! Why are they even included in our discussion?” then, my friend, you are colossally fucked. You don’t yet understand the sheer beauty and utility of binding similar classes with a common interface, forcing them to follow a common pattern. You don’t understand the very essence of object oriented programming. 

## FACTORY METHOD PATTERN

Open any tutorial for this pattern, and you’ll pretty much find the same definition everywhere, that comes straight from the _Gang of Four_ handbook-
 
"Define an interface for creating an object, but let subclasses decide which class to instantiate. The Factory method lets a class defer instantiation it uses to subclasses." 

Here’s the funny thing. When I read this definition, I immediately knew what each of those words meant individually. I knew what these words meant when put together. I perfectly understood both of the sentences.

Yet, I had no fucking clue what Factory Method pattern was, or what was it supposed to do, or why was it supposed to be useful!

So, I closed all the tutorials, opened a notebook, and started doodling on it. One thing was clear to me - I had to start with what I already knew. And that was Simple Factory. 

So, I started drawing various diagrams, trying to visualize my understanding of Simple Factory. 

At one point, I ended up with this figure - 

![Untitled Diagram.jpg]({{site.baseurl}}/_posts/Untitled Diagram.jpg)

This is Simple Factory. It beautifully solves the initial problem that we started with - bifurcating cars on the basis of $type without introducing a mishmash of if-else statement.

But wait, there is another way to solve the exact same problem. What if we slightly change this diagram, and do this instead - 

![Untitled Diagram.png]({{site.baseurl}}/_posts/Untitled Diagram.png)




