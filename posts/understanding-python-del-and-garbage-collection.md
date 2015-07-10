---
title: Demystifying Python's del, __del__ and garbage collection
date: 09-07-2015
layout: post.html
tags: ['Python', 'Destructor', 'Garbage Collection', 'Memory Management']
---

People are often confused about Python's `del` keyword and `__del__()` method.
Most people think that applying `del` on an object causes the object's
`__del__()` method to be called. But this is not true. `del` keyword merely
decrements the objects reference count and de-scopes the variable on its
application. But it doesn't necessarily invoke the object's `__del__()` method.
`__del__` method will be called only when the garbage collection kicks in.
And this would happens automatically when the reference count of an object
becomes zero.

`del`: Decrements reference count  
`__del__()`: Object destructor. Called when an object is garbage collected

    :::python
    class Foo():

        def __del__(self):
            print '__del__() called'


Now let's create some instances of this `Foo` class and call `del` upon these
instances to see what happens.

    :::ipython
    In [1]: %paste
        class Foo():

            def __del__(self):
                print '__del__() called'
    ## -- End pasted text --

    In [2]: from sys import getrefcount

    In [3]: getrefcount(Foo)
    Out[3]: 2

    In [4]: a = Foo()

    In [5]: getrefcount(a)
    Out[5]: 2

At this point you might be wondering why `getrefcount(Foo)` and `getrefcount(a)`
shows 2 instead of 1. Well, the reason is when you call the method `getrefcount`
the argument is passed by value, effectively bumping the reference count by 1.
This extra reference will be freed up once the `getrefcount` method exits. So
effectively we have a reference count of 1 for the instance `a` here. Now let's
make more references to the same instance.

    :::ipython
    In [6]: b = a

    In [7]: getrefcount(a)
    Out[7]: 3

    In [8]: getrefcount(b)
    Out[8]: 3

As expected, the reference count is incremented. Now let's call `del` on a to
see if the object's `__del__()` is invoked.

    :::ipython
    In [9]: del a

    In [10]: del b
    __del__() called

As you can see, `__del__()` is not invoked on applying `del` the first reference.
This is because the reference count is still 1. On calling `del` on `b` causes
the reference count become zero, garbage collection kicks in and finally
our long awaited `__del__()` is called.

But there are situation where the destructor goes astray. These situations and
the alternative approaches are explained in detail in the following brilliant
blog post.

[Safely using destructors in Python](http://eli.thegreenplace.net/2009/06/12/safely-using-destructors-in-python)
