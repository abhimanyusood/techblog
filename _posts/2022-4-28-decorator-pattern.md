---
layout: post
title: Decorator Pattern
published: true
---

>Decorators are used to add new behavior to an already instantiated object **at runtime**, as opposed to extending (adding new behavior to) that object's class at compile time by subclassing it.

It is easy to add functionality to an entire class of objects by subclassing an object's class, but it is impossible to extend a single object this way. With the Decorator Pattern, you can add functionality to a single object and leave others like it unmodified.

Say you want to write a logger. It takes a message and outputs it to one of the following targets - cmd, file, email, db. 

The first solution that would come to anybody's mind is to create a Logger interface and write four classes that implement it - 

```java
class Logger
   function log()
```

* Logger
    * CmdLogger
    * FileLogger
    * EmailLogger
    * DbLogger

Suddenly a wild new expectation appears. Before writing the message, your system should convert it to one of the following formats - json, xml or yaml. And this doesn't have to be an expectation that arises in the future, it can be present from the get-go itself. In either case, the most obvious solution that comes to mind is subclassing each of the above classes-

* Logger
    * CmdLogger
        * CmdJsonLogger
        * CmdXmlLogger
        * CmdYamlLogger
    * FileLogger
        * FileJsonLogger
        * FileXmlLogger
        * CmdYamlLogger
    * EmailLogger
        * EmailJsonLogger
        * EmailXmlLogger
        * EmailYamlLogger
    * DbLogger
        * DbJsonLogger
        * DbXmlLogger
        * DbYamlLogger


Why is this not a good solution?

Class Explosion - What if you had ten targets and five output formats? Would you have written 10x5=50 classes?

But that's a logictical problem. The core issue lies deeper. 

Core issue - Your class structure does not fundamentally mirror the structure of the problem. If you think about it, CmdJsonLogger, FileJsonLogger, EmailJsonLogger, and DbJsonLogger aren't supposed to be independent entities. Rather JsonLogger is a separate behavior that acts on - or is sort of chained to - the output of CmdLogger, FileLogger, and others. As a matter of fact, if you create a new implementation of Logger in the future with an entirely new target, JsonLogger can be chained to that as well! XmlLogger and YamlLogger can do the same.

"Chaining" is the key word here.

Your system should ideally have just 4+3=7 classes (as opposed to 4\*3=12 classes) - 

* Logger
    * CmdLogger
    * FileLogger
    * EmailLogger
    * DbLogger
  
 * Decorator
   * JsonDecorator
   * XmlDecorator
   * YamlDecorator


1. We have the Logger interface, which we'll interchangeably call Component interface
2. We'll call the original classes that implement the Logger interface ConcreteLogger(s) or ConcreteComponents
3. We'll call the independent classes that can be chained to the output of Loggers to format them Decorator(s).


I'll repeat - "Chaining" is the key word here. Almost like function compsition fog = f(g(x)). Or in our case, Decorator(ConcreteLogger(x)).

For this to work, two preconditions are necessary - 
1. ConcreteLogger was earlier plugged into our main system via Logger interface. For us to be able to hot-swap ConcreteLogger with the entire chain -  Decorator(ConcreteLogger(x)), the Decorator must be implemeting the Logger interface as well!
2. In fog, output of first function becomes the input of second function. In our case, we'll make the entire ConcreteLogger itself to be the input of Decorator. We do this by injecting ConcreteLogger into the constructor of Decorator - i.e. class composition. 

Since this is my blog and I can do whatever I want, I'll repeat the entire second-last paragraph in generic terms - 

For this to work, two preconditions are necessary - 
1. ConcreteComponent was earlier plugged into our main system via Component interface. For us to be able to hot-swap ConcreteComponent with the entire chain -  Decorator(ConcreteComponent(x)), the Decorator must be implemeting the Component interface as well!
2. In fog, output of first function becomes the input of second function. In our case, we'll make the entire ConcreteComponent itself to be the input of Decorator. We do this by injecting ConcreteComponent into the constructor of Decorator - i.e. class composition. 


When the chain is eventually plugged into the main system, the main system calls the Decorator's log() function with logMessage as input. Please note that the Decorator has inherited this log() function from Logger interface and then implemented it. Decorator's log() function first jsonifies the incoming logMessage, and then calls the log() function of the ConcreteLogger (that's incoming via its own constructor) with jsonifiedLogMessage as input, which will basically write jsonifiedLogMessage to that particular log target.


```java
class CmdLogger implements Logger
   function log()
```
```java
class Decorator implements Logger
   Logger concretelogger  //this will be injected in constructor of JsonDecorator
   abstract function log() //this will be implemented by JsonDecorator
```
```java
class JsonDecorator extends Decorator
   constructor JsonDecorator(concretelogger)
      this.concretelogger = concretelogger
   function log(logMessage)
      jsonifiedLogMessage = jsonify(logMessage)
      this.concretelogger.log(jsonifiedLogMessage)
```

Since this is my blog, I'll write the above code's generic version as well - 



```java
class ConcreteComponent implements Component
   function process()
```
```java
class Decorator implements Component
   Component concretecomponent  //this will be injected in constructor of ConcreteDecorator
   abstract function process() //this will be implemented by ConcreteDecorator
```
```java
class ConcreteDecorator extends Decorator
   constructor ConcreteDecorator(concretedecorator)
      this.concretecomponent = concretecomponent
   function process(input)
      //Decorate the input
      //Invoke this.concretecomponent.process()
```




Comment section of this answer - https://stackoverflow.com/a/2707425
D section of this answer - https://stackoverflow.com/a/47949162
This answer - https://stackoverflow.com/a/1549771
https://stackoverflow.com/a/9915893
https://www.geeksforgeeks.org/timing-functions-with-decorators-python/
