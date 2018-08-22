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

Open [any tutorial](https://code.tutsplus.com/tutorials/design-patterns-the-factory-method-pattern--cms-24530) for this pattern, and you’ll pretty much find the same definition everywhere, that comes straight from the _Gang of Four_ handbook-
 
"Define an interface for creating an object, but let subclasses decide which class to instantiate. The Factory method lets a class defer instantiation it uses to subclasses." 

Here’s the funny thing. When I read this definition, I immediately knew what each of those words meant individually. I knew what these words meant when put together. I perfectly understood both of the sentences.

Yet, I had no fucking clue what Factory Method pattern was, or what was it supposed to do, or why was it supposed to be useful!

So, I closed all the tutorials, opened a notebook, and started doodling on it. One thing was clear to me - I had to start with what I already knew. And that was Simple Factory. 

So, I started drawing various diagrams, trying to visualize my understanding of Simple Factory. 

At one point, I ended up with this figure - 

![Untitled Diagram.jpg]({{site.baseurl}}/images/factory-design-patterns/SimpleFactory.jpg)

This is Simple Factory. It beautifully solves the initial problem that we started with - instantiating different cars on the basis of $type without introducing a mishmash of if-else statement.

But wait, there is another way to solve the exact same problem. What if we slightly change this diagram, and do this instead - 

![Untitled Diagram.png]({{site.baseurl}}/images/factory-design-patterns/FactoryMethod.png)

We have simply introduced a bifurcation at the level of factory itself. Now, instead of one, there are two factories producing the two cars. We’ve pretty much solved the same problem, just a bit differently.

I can’t stress this enough. Factory Method is just and additional bifurcation ahead of Simple Factory. In its very essence, we just solved the same problem that we did with Simple Factory, just a bit differently.

But in the process, we discovered something so powerful, it will have huge implications. We’ll come to these implications in a moment, but first let’s see how my understanding of Factory Method as a variant of Simple Factory, is compatible with its conventional definitions.

[Here’s](https://stackoverflow.com/a/5740080) the best UML of Factory Method that I could find- 

![Untitled Diagram.png]({{site.baseurl}}/images/factory-design-patterns/FactoryMethodUML.png)

And here’s the best definition -> 

_A concrete creator implements an abstract creator and creates a concrete product which implements an abstract product._

Here’s the wikipedia PHP code for factory method pattern, that satisfies both the diagram and the definition - 

```php
/* Factory and car interfaces */

interface CarFactory 
{
    public function makeCar();
}

interface Car 
{
    public function getType();
}

/* Concrete implementations of the factory and car */

class SedanFactory implements CarFactory 
{
    public function makeCar() 
    {
        return new Sedan();
    }
}

class Sedan implements Car 
{
    public function getType() 
    {
        return 'Sedan';
    }
}

/* Client */

$factory = new SedanFactory();
$car = $factory->makeCar();
print $car->getType();

```

Now that we have listed all three conventional things about Factory Method, let’s see how they're compatible with my understanding - 

Consider my diagram again- 

![Untitled Diagram.png]({{site.baseurl}}/images/factory-design-patterns/FactoryMethod.png)

Watch me draw one teeny tiny red line - 

![Untitled Diagram.png]({{site.baseurl}}/images/factory-design-patterns/FactoryMethodCircled.png)

You see! 

Concentrate on the part inside the red snake.

It’s the same as the conventional UML for Factory Method!

So for now, for interviews and stuff, you can remember the three conventional wisdoms about Factory Method (uml, definition and php example). But understanding it my way has an enormous advantage that it immediately unlocks the full unexpected power of Factory Method, which we will now see.

>A side note - Why is it called ‘Factory Method’?
>
>Because the abstract creator (abstract factory) will have a makeCar() method. And each of the concrete creators (concrete factories) will override this method in its own way. 
>
>SedanFactory will say - 
>
>```php
>makeCar()
>{
>	$car = new car_sedan();
>}
>```
>
>SuvFactory will say - 
>
>```php
make Car()
{
	$car = new car_suv();
}
>```
>
>This method makeCar() is called the Factory Method. And the entire design pattern gets its name from it. So, it is very crucial. Just remember - 
>
>**In Factory Method, the abstract creator has a factoryMethod, which each of the concrete creator would override in its own way.**

## ABSTRACT FACTORY - the magic sword you already hold in your hands and don't even realize!

Here’s the magical mantra -

>“You can join two Simple Factories using Factory Method Pattern. The resulting structure is called Abstract Factory and it is immensely powerful.”

Here’s how.

Let’s say in our nice little car example, suddenly, another parameter is introduced - location. Now, car manufacturing is taking place in two locations USA and UK. Both locations produce the same models as before, but slightly differently.

If you do not realize the exact implication of this harmless looking new requirement, let me elaborate just how cornered we are.

Consider our car manufacturing code as a black box. When we began this article, that black box had no input. It just produced an output - a “car”.

Then we introduced an input, $type. And walked straight into our first problem - Now, our black box had to be internally split into two black boxes - each of which handles one $type. For doing this, most people would have used if-else conditions, as we witnessed in our first problem. If there were more than 2 $types, as many if-else conditions would've had to be used. The entire code base in essence would've had to be branched.

That was one level of bifurcation.

What happens when we introduce another input $location? Now not only the car can differ by $type, but by $location as well. A US sedan will be different from a UK sedan which will be different from a UK suv. The if-else conditions don’t double, they get squared! The codebase is now a messy soup of spaghettified if-else's. 

This was the second level of bifurcation.

Using Simple Factory, we handled the first one and avoided the use of if-elses.

Now, to handle the second one, we do this - 

![Untitled Diagram.png]({{site.baseurl}}/images/factory-design-patterns/AbstractFactoryUncircled.jpg)


Imagine a black colored box around this diagram, if you will. The only two inputs that go into this black box are $location and $type.

At the top of this diagram, there should be a driver (which is not shown here) that receives these two inputs. 

It takes $type and instantiates the corresponding concrete factory -
```php
$concreteFactory = new ucFirst($type)."Factory";
```

Next, it calls the factory method makeCar() of the instantiated concrete class (which, you might remember, is the overridden version of AbstractFactory’s factory method)

```php
$concreteFactory->makeCar($type,$location)
```

makeCar() does this - 

```php
makeCar($type,$location)
{
	$car = new “car_”.$location.”_”.$type;
	return $car->getCar();
}
```

Do you see the beauty of it? No if-else conditions necessary. Black box remains a black box.


Wait, but what the fuck does this diagram have to do with the magic matra introduced in the beginning of this section?

Let me redraw the same diagram, but this time, a bit differently - 

![Untitled Diagram.png]({{site.baseurl}}/images/factory-design-patterns/AbstractFactoryCircled.jpg)

Look closely. The things inside the two yellow circles are two Simple Factories.

And then, these two yellow circles are joined together in Factory Method pattern (red circle)

That, ladies and gentlemen, is how you get an Abstract Factory.

And that is what the mantra was talking about!


>I can't stress this enough - Abstract Factory is a superpower. Because, it allows you to write systems in which the entire code/blackbox has a two orders bifurcation based on two different inputs. When you encounter such a system, don't think. Just go straight to Abstract Factory Design Pattern.

## SOME MORE EXAMPLES OF ABSTRACT FACTORY

Let’s say you are implementing a GUI app that has two elements - a toolbar, and a dialog box. That, right there, is your first level of bifurcation.

Now, let’s assume that you want to develop this app in two different themes - Light Theme and Dark Theme.

That is your second level of bifurcation.

If you were doing this with if else, the code would look something like this - 

```
if lighttheme:
	if toolbar:
	else if dialog:
else if darktheme:
	if toolbar:
	else if dialog:
```

[Here’s](https://medium.com/software-engineering-101/design-patterns-abstract-factory-39a22985bdbf) how you would do it using Abstract Factory -

![Untitled Diagram.png]({{site.baseurl}}/images/factory-design-patterns/GUIthemeUML.png)

Here’s the best UML for Abstract Factory I could find - 

![Untitled Diagram.png]({{site.baseurl}}/images/factory-design-patterns/AbstractFactoryUML.png)

And you can see, it follows the exact same pattern.

From an understanding point of view, this diagram can almost be directly be translated to my home made diagram that I introduced in the beginning of this section. But those of you with a keener eye will notice one differece - 

In my diagram, ProductA1, ProdutA2, ProductB1, and ProductB2 would have implemented one common interface - AbstractProduct

But here, ProductA1 and Product A2 implement one interface - AbstractProductA
while,
Product B1 and Product B2 implement another interface - AbstractProductB

This is as if in our example, car_usa_sedan and car_uk_sedan would have implemented interface - car_sedan, while, car_usa_suv and car_uk_suv would have implemented another interface - car_suv, instead of all four of them implementing a common interface - car.

I must clarify that technically speaking, the UML’s understanding is more correct than mine. But either way, it doesn’t matter. When we come down to the Product level, the exact interface we choose to implement depends on the nature of our problem Interfaces, in essence, are just constraining mechanisms, that force the code to follow a pattern. 

## EPILOGUE

I must end this by telling you how it all started. How the fuck I found myself in a position where I could no longer do without a basic knowledge of these design patterns.


You see, I was designing a scoring system.

Here's how it works - 

A candidate gives a test.

We have the candidate’s answer
We have the correct answer
And we have n number of criteria (competencies) on which to score the candidate (speed, accuracy, grammar etc.)

But that’s not all!

The test might be conducted in multiple languages!


If you’ve understood the essence of what I've taught you in this article, then you will immediately realize that we're facing black-box system with two-level bifurcation here. 

The first level - competencies 
The second level - languages.

Lo and Behold! Abstract Factory design pattern to the rescue.

I would end this article by giving you a homework. Normally, I don’t do this, but this time, I think it’s necessary.

So, here goes - 

1. Look around you and try to identify real life problems that exhibit _"a black-box with two-level bifurcation"_ structure.
2. I want you to take this understanding from this article, and use it to solve any one of these problems. Implement the codebase using Abstract Factory design pattern
3. Once you’re comfortable with recognizing and using Abstract Factory, I want you to go through all the technical definitions and expositions of all the three factory design patterns we discussed in this article, and see how they translate into our way of understanding them. Are the two compatible, or is there a discrepancy. If so, what modifications in our understanding (both yours and mine) are required. Let me know, and I will update this article accordingly.

That’s it.

I hope this article would fucking change your life.

Later, bitches!



