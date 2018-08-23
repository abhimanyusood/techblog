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
    	echo "foo";
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

        
