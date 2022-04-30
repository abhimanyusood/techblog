---
layout: post
title: Decorator Pattern
published: false
---

>Decorators are used to add new behavior to an object, **dynamically**, **at runtime**, without changing the underlying code.

1. Creating a subclass is how you extend the functionality of your code (i.e. add new behavior) at compile time
2. Using decorators is how you extend the functionality of your code at runtime

Say you want to write a logger. It takes a message and outputs it to one of the following targets - cmd, file, email, db. 

The first solution that would come to anybody's mind is to create a Logger interface and write four classes that implement it - 
* Logger
    * CmdLogger
    * FileLogger
    * EmailLogger
    * DbLogger

Suddenly a wild new expectation appears. Before writing the message, your system should convert it to one of the following formats - json, xml or yaml.

The most obvious solution is subclassing each of the above classes- 
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
 
 
