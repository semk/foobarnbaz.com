---
title: Understanding Python variables
date: '08-07-2012'
time: '23:45'
tags: ['Python', 'C', Variables']
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

{% block postcontent %}
{% markdown %}

{% endmarkdown %}
{% endblock %}
