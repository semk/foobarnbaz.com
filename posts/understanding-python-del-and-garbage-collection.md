---
title: Understanding Python's del, __del__ and garbage collection
date: 09-07-2015
layout: post.html
tags: ['Python', 'Garbage Collection', 'Memory Management']
---

People are often confused about Python's `del` keyword and `__del__()` method. Most people think that applying `del` on an object causes the object's `__del__()` method to be called. But this is not true. `del` keyword merely decrements the objects reference count and de-scopes the variable on its application. But it doesn't necessarily invoke the object's `__del__()` method. `__del__` method will be called only when the garbage collection kicks in. And this would happens automatically when the reference count of an object becomes zero.

`del`            : Decrements reference count  
`__del__()`      : Object destructor. Called when an object is garbage collected
