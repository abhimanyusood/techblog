---
layout: post
title: Interfaces, Polymorphism, and Factory Design Patterns
published: false
---

The endgoal of factories is object creation. Factories are just ways to create an object. To understand factories, you should completely ignore factories and think in terms of how you’re creating the final object.

## Thought-process when creating objects 
```java
Nokia a = new Nokia()
```
Anytime you see a “new” statement like this, your mind should immediately think – 

_This is too concrete! Can I bind Nokia to an interface (Mobile) and then do this -_
```java
Mobile a = new Nokia()
```

Principle – 
>Always code to an interface

## What is an interface?
An interface is a contract that you impose upon your object. Let’s say interface Mobile has a method “ring”. Now, any class that implements this interface is contractually obligated to implement this method. If not, an error will be thrown at compile time.

## Why should you always code to an interface?
Problem Statement: Create two mobiles – Nokia and Oneplus and call their ring methods

```java
Nokia a = new Nokia()
Oneplus b = new Oneplus()
a.ring()
b.ring()
```

If you make make Nokia and Oneplus sign a contract by binding them to Mobile Interface, then you can create something called polymorphic arrays – 

```java
Mobile a = new Nokia()
Mobile b = new Oneplus()
Mobile[] m = {a,b}
for (i=0;i<2;i++){
    m[i].ring()
}
```

On the surface, it might just seem like a nifty trick. But fundamentally, something much deeper is going on. The compiler allows you to put two different types of objects (Nokia and Oneplus) into the same array (m) because Nokia and Oneplus are following the same contract which ensures they’ll both have ring() method. Without this contractual obligation imposed by the interface, compiler couldn’t have allowed this.

Polymorphic arrays is just one manifestation of this fundamental principle –  

> Coding to an interface decouples your code from any concrete implementation of that interface.

You can create arrays of type Mobile, pass arguments of type Mobile between functions, and basically write your entire code to type Mobile, without caring about which exact implementation of Mobile (Nokia or Oneplus) will be flowing through your code when it runs. The exact implementation can and will be determined at runtime.

## Usefulness - This allows you to future-proof your code!

Think of it this way. When a customer brings you a new project, the goal is always to create one thing – Nokia. Nothing else is in the picture. If you’re too focused on Nokia, you’ll write your entire code to the type Nokia. You’ll create arrays of type Nokia. You’ll pass arguments of type Nokia between functions. 

One year later, when your initial product is in production and stable, the customer will come back and tell you – “Now we also want you to support Oneplus along with Nokia.”

But all your code is written to Nokia! 

You’re screwed. You have to change entire code which is - 
1. a big hassle
2. a complete violation of open-close principle

Rewind to one year earlier. The customer just told you that you’ve to create Nokia. Rather than straightaway starting with the code, you take time to think. Nokia is a mobile. There can be other mobiles too all of which will have some common behaviors – like ring(). 

You abstract all the common behaviors out into an interface “Mobile” and make Nokia implement the Mobile interface. Now you write your entire code to type Mobile, instead of Nokia. There’s no mention of Nokia anywhere in your codebase.

One year later, when the requirement for Oneplus comes, you’re already sorted. You just create a new Oneplus class, make it implement Mobile interface and pass it into your code. Your code already knows how to work with Mobile interface.


## Simple Factory Pattern

So, the customer wants you to create a phone of their choice - Nokia or Oneplus, and do some stuff with it, like make it ring.
Your code would look something like this - 

```java
class MainLogic
    function createMobileAndDoStuff(type)
        if (type == "Nokia")
            Mobile m = new Nokia()
        else if (type == "Oneplus")
            Mobile m = new Oneplus()
        
        m.ring()
```  

Let's say, tomorrow the customer comes back and tells you to add a new mobile - Apple. You will have to touch your main logic which is a violation of open-close principle.

> Open-close principle is not just theoretical. Imagine you've got complex production code that has been thoroughly tested. Would you want to touch it even if it's  to add just one line of code to one file, without taking it through the entire QA process again?

Solution - Separate the Object Creation from the Main Logic and abstract it out into a separate class. This class is called a Simple Factory. It does just one thing - creating the object (based on the type flag it gets as input).

```java
class SimpleFactory
    function createMobile(type) //generally a static function
        if (type == "Nokia")
            Mobile m = new Nokia()
        else if (type == "Oneplus")
            Mobile m = new Oneplus()
        return m

class MainLogic
    function createMobileAndDoStuff(type)
        SimpleFactory f = new SimpleFactory()
        m = f.createMobile(type)
        m.ring()
``` 

Your don't have to touch your Main Logic anymore if you want to add a new type of Mobile.

> Ultimate endgoal is object creation. Simple Factory just provides a different way to do create the same object, in a way that is more future-proof.

## Factory Method Pattern

#### Clearing some Misconceptions - 

1. Unlike simple factory, it is NOT the core purpose of factory method pattern to encapsulate out object creation. 
2. In Headfirst book, the factory method createPizza() of NYStylePizzaStore has the parameter 'type' based on which the exact NYStyle pizza is instantiated. This assumption that factory method must be parameterized leads to the confusion that the purpose of the factory is to allow clients to pick and choose amongst multiple product implementations at runtime. This is false. A factory method by default has no parameters. Sending a parameter to it is just an add-on choice that you can make (this is actually written in GangOfFour book!)
3. It is NOT the core purpose of factory method pattern to impose a standard operating procedure for quality control on the further process that you perform on your newly created object, as hinted by the Headfirst book.
4. Don't try to understand factory method pattern by building it UP from simple factory. Simple Factory is a degerate case of Factory Method only in a very tangential sense.

```java
class MobileFactory
    function orderMobile() //generally a static function
        Nokia n = new Nokia()
        n.test()
        n.box()
        n.ship()
        
//Application Logic
MobileFactory mf = new MobileFactory()
mf.orderMobile()
```

First, let's fix the most glaring error.

```java
class MobileFactory
    function orderMobile() //generally a static function
        Mobile m = new Nokia()
        m.test()
        m.box()
        m.ship()
        
//Application Logic
MobileFactory mf = new MobileFactory()
mf.orderMobile()
```

This is always the first step in making your code generic. Now, how can we go one step further and make it even more generic?

**Observation** - Fundamentally, I'm creating a mobile and then performing some business logic steps (test(), box(), ship()) on it. The important thing to note is that no matter which specific instance of Mobile interface that I choose to create, my business logic steps will remain the same! Therefore, in order to make my MobileFactory truly generic, I have to remove Mobile m = new Nokia() instantiation from it. 

One way to do it is to give the responsibility of instantiating Mobile m = Nokia() to someone else. Then you just inject the newly created m into orderMobile function and perform the rest of the business logic steps on m

```java
class MobileFactory
    function orderMobile(m) //generally a static function
        m.test()
        m.box()
        m.ship()
        
//Application Logic
Mobile m = new Nokia()
MobileFactory mf = new MobileFactory()
mf.orderMobile(m)
```

Now, your MobileFactory is completely generic. It can take any type of Mobile as input and perform your business logic steps on it. But the problem is that it is no longer a factory! A factory, by definition, must do object creation and there's no object creation happening here. There's nothing wrong with this. It's just a different way of accomplishing the same goal. It's just that it is not a factory, and factories is what we're studying.

>So, to keep it a factory, we have to somehow keep the object creation tied to the the MobileFactory class, and yet, not have any object instantiation happening in this class!

```java
class MobileFactory
    function orderMobile() //generally a static function
        Mobile m = new Nokia()
        m.test()
        m.box()
        m.ship()
        
//Application Logic
MobileFactory mf = new MobileFactory()
mf.orderMobile()
```

#### Step 1 - move the object instantiation to a separate createMobile Function within the MobileFactory
```java
class MobileFactory

    function createMobile()
        Mobile m - new Nokia()
    
    function orderMobile() //generally a static function
        m = createMobile()
        m.test()
        m.box()
        m.ship()
        
//Application Logic
MobileFactory mf = new MobileFactory()
mf.orderMobile()
```

#### Step 2 - Make the createMobile function abstract. 

```java
class MobileFactory

    abstract function createMobile()
    
    function orderMobile() //generally a static function
        m = createMobile()
        m.test()
        m.box()
        m.ship()
```

If a client like Nokia wants to use MobileFactory, they will first have to subclass it. Since createMobile function is abstract, this subclass will first have to implement it. The subclass can implement it in its own way and instantiate the type of Mobile that it wants.

> We have *deferred* object instantiation to the subclass
> The core purpose of factory method is to defer object instantiation to the subclass

```java
class MobileFactory

    abstract function createMobile()
    
    function orderMobile() //generally a static function
        m = createMobile()
        m.test()
        m.box()
        m.ship()

class NokiaFactory extends MobileFactory
    function createMobile()
        Mobile m = new Nokia()
        return m
        
//Application Logic
MobileFactory mf = new NokiaFactory()
mf.orderMobile()
```

#### Terminology - 
1. MobileFactory is called the Abstract Creator
2. NokiaFactory is called the Concrete Creator
3. Mobile is called the Abstract Product
4. Nokia is called the Concrete Product
5. createMobile() is called the Factory Method


In real life, you often come across situations (especially while building frameworks and third-party libraries) where you know beforehand a sequence of operations you want to perform on an object as a part of your business logic, but at the time of coding those operations, you don't know what concrete object those operations will eventually be performed on. You don't care either. These concrete objects follow an interface and can be hot-swapped with each other anyway. For now, you write your business logic operations (in Abstract Creator) as if they are expected to be performed on the interface (Abstract Product) itself.

Eventually, a client will come along and subclass your Abstract Creator into a Concrete Creator. They will implement the factory method and instantiate the Concrete Product. With this, their job would be over. The Concrete Product created by them will sort of flow back into the framework and the rest of the business logic operations will be performed on it.

Thinking in terms of framework is helpful. Let's take this example of a Mobile Ordering Framework. I can ship this, just like Laravel or Yii2 - 

```java
class MobileFactory

    abstract function createMobile()
    
    function orderMobile() //generally a static function
        m = createMobile()
        m.test()
        m.box()
        m.ship()

//APPLICATION INSTANTIATION LOGIC FROM CONFIG
```

Now, a new client, Oneplus wants to use this framework. They will download the framework code and create a new project on top of it.

Let's see how things look from the client's perspective. 

First thing - unless they specifically open the framework files and study them, the above framework code will be completely hidden from them. All they will get is an empty folder and some Documentation.

The document will tell them to create three files in the empty folder - 

```java
class OneplusFactory extends MobileFactory
    function createMobile()
        Mobile m = new Oneplus()
        return m
```


```java
class Oneplus extends Mobile
    function test()
        print("Test Oneplus")
    function box()
        print("Box Oneplus")
    function ship()
        print("Ship Oneplus")
```
```json
{'ConcreteCreatorClass':'OneplusFactory'}
```

That's it. By just creating these three files, their project will be up and running!

#### Notes - 

1. Here, Oneplus (Concrete Product) didn't even exist at the time the framework was written!
2. When the Client plugs itself into the framework (via inheritance) it defines a ConcreteProduct for the framework to use
3. Thus, Factory Method allows you to implement a framework in terms of an unknown product. Inheritance is the vehicle it uses to do that.
4. The most important point to grasp here is that the ConcreteCreator is the client. In other words, the client is a subclass whose parent defines the factoryMethod(). This is why we say that Factory Method is implemented by Inheritance.
5. Abstract Creator invokes its own factory method. If we remove orderMobile() from the MobileFactory class, leaving only a single method createMobile() behind, it is no longer the Factory Method pattern. In other words, Factory Method cannot be implemented with less than two methods in the parent class; and one must invoke the other.

Illustration of last point - 

Original Code - 

```java
class MobileFactory

    abstract function createMobile()
    
    function orderMobile() //generally a static function
        m = createMobile()
        m.test()
        m.box()
        m.ship()

class NokiaFactory extends MobileFactory
    function createMobile()
        Mobile m = new Nokia()
        return m
        
//Application Logic
MobileFactory mf = new NokiaFactory()
mf.orderMobile()
```

Let's remove orderMobile() function from MobileFactory. orderMobile() used to invoke createMobile(). Since a factory must create object, we must invoke createMobile() from somewhere else now. The only logical place to do so is Application Logic. Other business logic operations will also have to go to Application Logic. Your final code looks like this - 

```java
class MobileFactory

    abstract function createMobile()

class NokiaFactory extends MobileFactory
    function createMobile()
        Mobile m = new Nokia()
        return m
        
//Application Logic
MobileFactory mf = new NokiaFactory()
Mobile m = mf.createMobile()
m.test()
m.box()
m.ship()
```

Your Factory Method pattern has devolved into Simple Factory pattern! (Core principle behind Simple Factory is encapsulating object creation. That creation doesn't always have to go through if else statements. However, if you insist, you can simply create subtypes of Nokia like Nokia2g and Nokia3g both of which will implement Mobile interface, and send a type parameter in createMobile() function which will decide which of these two types to actually create. Your code will become COMPLETELY indistinguishable from Simple Factory)

#### References - 
1. https://stackoverflow.com/a/67047425
2. https://stackoverflow.com/a/50786084
3. https://stackoverflow.com/a/63831679
4. https://stackoverflow.com/a/38668246
5. https://stackoverflow.com/a/2982330
6. Gang Of Four Book
