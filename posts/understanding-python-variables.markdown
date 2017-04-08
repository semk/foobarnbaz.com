---
title: Understanding Python variables and Memory Management
date: '08-07-2012'
time: '23:45'
tags: ['Python', 'C', 'Variables', 'Memory Management']
layout: 'post.html'
---

Have you ever noticed any difference between variables in Python and C? For example, when you do an assignment like the following in C, it actually creates a block of memory space so that it can hold the value for that variable.

	:::c
	int a = 1;

You can think of it as putting the value assigned in a box with the variable name as shown below.

![int a =1;](http://python.net/~goodger/projects/pycon/2007/idiomatic/a1box.png)

And for all the variables you create a new box is created with the variable name to hold the value. If you change the value of the variable the box will be updated with the new value. That means doing

	:::c
	a = 2;

will result in

![a = 2;](http://python.net/~goodger/projects/pycon/2007/idiomatic/a2box.png)

Assigning one variable to another makes a copy of the value and put that value in the new box.

	:::c
	int b = a;

![b=2](http://python.net/~goodger/projects/pycon/2007/idiomatic/b2box.png) ![a = 2](http://python.net/~goodger/projects/pycon/2007/idiomatic/a2box.png)

But in Python variables work more like tags unlike the boxes you have seen before. When you do an assignment in Python, it tags the value with the variable name.

	:::python
	a = 1

![a = 1](http://python.net/~goodger/projects/pycon/2007/idiomatic/a1tag.png)

and if you change the value of the varaible, it just changes the tag to the new value in memory. You dont need to do the housekeeping job of freeing the memory here. Python's Automatic Garbage Collection does it for you. When a value is without names/tags it is automatically removed from memory.

	:::python
	a = 2

![a = 2](http://python.net/~goodger/projects/pycon/2007/idiomatic/a2tag.png) ![1](http://python.net/~goodger/projects/pycon/2007/idiomatic/1.png)

Assigning one variable to another makes a new tag bound to the same value as show below.

	:::python
	b = a

![b = a](http://python.net/~goodger/projects/pycon/2007/idiomatic/ab2tag.png)

Other languages have 'variables'. Python has 'names'.

### A bit about Python's memory management

As you have seen before, a value will have only one copy in memory and all the variables having this value will refer to this memory location. For example when you have variables `a`, `b`, `c` having a value 10, it doesn't mean that there will be 3 copy of `10`s in memory. There will be only one `10` and all the variables `a`, `b`, `c` will point to this value. Once a variable is updated, say you are doing `a += 1` a new value `11` will be allocated in memory and `a` will be pointing to this.

Let's check this behaviour with Python Interpreter. Start the Python Shell and try the following for yourselves.

	:::python
	>>> a = 10
	>>> b = 10
	>>> c = 10
	>>> id(a), id(b), id(c)
	(140621897573616, 140621897573616, 140621897573616)
	>>> a += 1
	>>> id(a)
	140621897573592

`id()` will return an objects memory address (object's identity). As you have noticed, when you assign the same integer value to the variables, we see the same ids. But this assumption does not hold true all the time. See the following for example

	:::python
	>>> x = 500
	>>> y = 500
	>>> id(x)
	4338740848
	>>> id(y)
	4338741040

What happened here? Even after assigning the same integer values to different variable names, we are getting two different ids here. These are actually the effects of CPython optimization we are observing here. CPython implementation keeps an array of integer objects for all integers between -5 and 256. So when we create an integer in that range, they simply back reference to the existing object. You may refer the following links for more information.

[Stack Overflow: "is" operator behaves unexpectedly with integers](http://stackoverflow.com/questions/306313/is-operator-behaves-unexpectedly-with-integers)

Let's take a look at strings now.

	:::python
	>>> s1 = 'hello'
	>>> s2 = 'hello'
	>>> id(s1), id(s2)
	(4454725888, 4454725888)
	>>> s1 == s2
	True
	>>> s1 is s2
	True
	>>> s3 = 'hello, world!'
	>>> s4 = 'hello, world!'
	>>> id(s3), id(s4)
	(4454721608, 4454721664)
	>>> s3 == s4
	True
	>>> s3 is s4
	False

Looks interesting, isn't it? When the string was a simple and shorter one the variable names where referring to the same object in memory. But when they became bigger, this was not the case. This is called interning, and Python does interning (to some extent) of shorter string literals (as in `s1` and `s2`) which are created at compile time. But in general, Python string literals creates a new string object each time (as in `s3` and `s4`). Interning is runtime dependant and is always a trade-off between memory use and the cost of checking if you are creating the same string. There's a built-in `intern()` function to forcefully apply interning. Read more about interning from the following links.

[Stack Overflow: Does Python intern Strings?](http://stackoverflow.com/questions/17679861/does-python-intern-strings)<br/>
[Stack Overflow: Python String Interning](http://stackoverflow.com/questions/15541404/python-string-interning)<br/>
[Internals of Python String Interning](http://guilload.com/python-string-interning/)<br/>

Now we will try to create custom objects and try to find their identities.

	:::python
	>>> class Foo:
	...     pass
	...
	>>> bar = Foo()
	>>> baz = Foo()
	>>> id(bar)
	140730612513248
	>>> id(baz)
	140730612513320

As you can see, the two instances have different identities. That means, there are two different copies of the same object in memory. When you are creating custom objects, they will have unique identities unless you are using [Singleton Pattern](http://foobarnbaz.com/2010/10/06/borg-pattern/) which overrides this behaviour (in `__new__()`) by giving out the same instance upon instance creation.

Thanks to Jay Pablo (see comments) for correcting the mistakes and making this post a better one.
