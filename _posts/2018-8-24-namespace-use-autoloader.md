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

Here's your mistake - you're trying to understand namespace as a **'conflict resolution mechanism'** - a system that you have to artificially introduce to avoid conflict in case two classes, metods etc. happen to have the same name.

It ain't that.

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

$fooDemo = new Foos\demo();
$fooDemo->doStuff();
```

Bingo!

```php
OUTPUT
foo
```

That's all well and good, but wouldn't it be tedious to use full class names EVERY TIME!

That's where **use** comes to rescue.

## USE

I do this - 

index.php
```php
require_once 'Foos/demo.php';
require_once 'Bars/demo.php';

use Foos\demo;

$fooDemo = new demo();
$fooDemo->doStuff();
```

What just happened?

Well, the statement - 
```php
use Foos\demo;
```
is interpreted as - 
```php
use Foos\demo as demo;
```

So, in your code, you can just use the class name demo and the full name Foo\demo will automatically be referenced!

That's the beauty of use statement. You retain the organizational power of namespaces without going through the pain of using full class names each time.

## AUTOLOADER

We've almost gotten to the point where our code is perfect. But the two require statements are still bothering me.

Come to think of it, if you have a comlex project where each file requires multiple other files, we would soon be in a mess!

Isn't there a way to automatically include these classes, without having to explicitly write the require statements?

Introducing 

```php
spl_autoload_register
```

This nifty little function does exactly what we want.

Here's how to use it. We add a new file autoloader.php parallel to index.php in repo root

autoloader.php
```php
function my_autoloader($class)
{
	$class = str_replace('\\', '/', $class);
  	include __DIR__.'/'.$class .'.php';
}

// register the autoloader
spl_autoload_register('my_autoloader');

```

That's pretty much it.

We define a function my_autoloader that takes a class (full) name as input (the one defined as per namespaces), invert backslashes to forward slashes, prefix __DIR__ to get full class path and includes it!

in your index.php, remove the two requires, and add a new one - 

index.php
```php
require_once 'autoloader.php';

use Foos\demo;

$fooDemo = new demo();
$fooDemo->doStuff();
```