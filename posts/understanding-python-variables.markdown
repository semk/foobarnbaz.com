---
title: Understanding Python variables and Memory Management
date: '08-07-2012'
time: '23:45'
tags: ['Python', 'C', 'Variables', 'Memory Management']
---
{% extends "post.html" %}
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

![int b = a;](http://python.net/~goodger/projects/pycon/2007/idiomatic/a2box.png)

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
	>>> a_list = [1] * 10
	>>> a_list
	[1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
	>>> [id(value) for value in a_list]
	[26089400, 26089400, 26089400, 26089400, 26089400, 26089400, 26089400, 26089400, 26089400, 26089400]

`id()` will return an objects memory address (object's identity). As you have noticed, when you create a list with same values, the values are not copied in memory. In fact, you are getting a list with references to the value `1`; not a list with 10 copies of `1`.

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

As you can see, the two instances have different identities. That means, there are two different copies of the same object in memory. This behaviour is different from what you have seen before. When you are creating objects they will have unique identities unless you are using [Singleton Pattern](http://foobarnbaz.com/2010/10/06/borg-pattern/). All [immutable](http://en.wikipedia.org/wiki/Immutable_object) objects like `str`, `int`, `float` will have same identities when objects are created simultaneously.
{% block postcontent %}
{% markdown %}

{% endmarkdown %}
{% endblock %}
