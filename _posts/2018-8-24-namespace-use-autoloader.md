---
layout: post
title: 'namespace, use, and autoloader - The Holy Trinity'
published: true
---

I made this repo -  

root
	Foos
    	demo.php
    Bars
    	demo.php
    index.php
        

Foos/demo.php
```php
class demo
{
	function doStuff()
    {
    	echo "foo";
    }
}
```

Bars/demo.php
```php
class demo
{
	function doStuff()
    {
    	echo "bar";
    }
}
```

index.php
```php
require_once 'Foos/demo.php';
require_once 'Bars/demo.php';
```

This is the error I get - 
```php
Cannot redeclare class demo in root\Bars\demo.php
```

>This is the worst possible approach to understanding namespaces

Here's your mistake - you're trying to understand namespace as a **'conflict resolution mechanism'** - a system that you have to artificially introduce to avoid conflict in case two classes, metods etc. have a common name.

It isn't that.

Namespace is an **'organizing mechanism'**.

It's a way of organizing the various classes into various drawers by giving them something akin to a 'full name'.

Here's what we do - 

Foos/demo.php
```php
namespace Foos;
class demo
{
	function doStuff()
    {
    	echo "foo";
    }
}
```

Bars/demo.php
```php
namespace Bars;
class demo
{
	function doStuff()
    {
    	echo "bar";
    }
}
```

index.php
```php
require_once 'Foos/demo.php';
require_once 'Bars/demo.php';
```

No error this time!

You see. That's all we did. We sort of orgainzed our classes into respective drawers. 

And the conflict resolution came as a natural outcome of that process. 

This is the essence of namespaces.

>I'm not saying that just for the sake of saying. Once you deploy namespaces, each class gets a full name, and you HAVE to refer to a class by its full name!

I do this - 

index.php
```php
require_once 'Foos/demo.php';
require_once 'Bars/demo.php';

$fooDemo = new demo();
$fooDemo->doStuff();
```

and I get the error - 
```php
class 'demo' not found in root\index.php
```

Notice that the error doesn't say 'conflict'. It says, class 'not found'.

Class was genuinely not found. Becuase we had to refer it with it's full name. And we didn't.

But if we do - 

index.php
```php
require_once 'Foos/demo.php';
require_once 'Bars/demo.php';

$fooDemo = new Foo\demo();
$fooDemo->doStuff();
```

Bingo!

```php
OUTPUT
foo
```
