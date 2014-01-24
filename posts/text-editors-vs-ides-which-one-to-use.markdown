---
title: 'IDEs vs. Text Editors: What should developers use?'
date: '24-01-2014'
time: '19:45'
tags: ['IDE', 'Python']
layout: 'post.html'
---

The answer to this question may vary from developers to developers. I truly believe that it also depends on the kind of programming language they use for developing applications. For example, I have heard many times from my fellow Java programmers that they couldn't live without the [Eclipse](http://www.eclipse.org/) IDE, that they will not be able to write a working Java program with the help of a simple text editor. Predominantly being a Python developer, I use [Emacs](http://www.gnu.org/software/emacs/) for most of my coding. I do not feel code completion or any other fancy IDE features are required for coding in Python, atleast for what I'm doing. The only difficult thing I encountered using just Emacs for coding is *debugging* the code I've written. For most of the time `print` statements will do the job fine. But at times I have to manually initialize `pdb` to debug my code. This could be painful sometimes. Although there are some tricks in Emacs to do the debugging, it didn't seem neat enough to me.

> Then when I started writing C and using GDB inside of Emacs I kind of tried to
> keep up the same way of doing things. That was the model we built Energize
> around. And that just never seemed like it worked well. And as time went by I
> gradually stopped even trying to use those tools like that and just stick in
> print statements and run the thing again. Repeat until done. Especially when
> you get to more and more primitive environments like JavaScript and Perl and
> stuff like that, it's the only choice you have -- There aren't any debuggers.
> <cite>Jamie Zawinski, "Coders At Work: Reflections on the Craft of Programming"
> </cite>

That's when I found [PyCharm](http://www.jetbrains.com/pycharm/) IDE for Python. They had this program where they provide free licenses for the commercial version of PyCharm for doing opensource projects. I applied for one mentioning my [voldemort](https://github.com/semk/voldemort) project and I got one free license.

PyCharm comes with a brilliant visual Python debugger and I found it really easier to debug the code using breakpoints and variable watchers. I spent a month or two coding on PyCharm and it really helped me code faster. One of the main reasons I love Emacs is that I could do all the tasks just by using the keyboard. Be it cutting & pasting text, switching between buffers it was all easy. There was also this good feeling of using just the keyboard. After using PyCharm for quite some time, I felt like using the mouse a lot for most of these taks. Another advantage of using a text editor is that you will get to know the programming language a lot better since there are no autocompletion or any other IDE features. You have to write the code on your own. Then there are things like version control integration for IDEs which I believe eases the effort for most of the people. But I belive they conceal a lot of activities from the user. I prefer doing the version control activities by myself on a terminal rather than handing it to an IDE. In this pypix [article](http://pypix.com/roundups/tools-python-super-stars/) some of the world's top Python developers talk about the tools preferred by them.

So what developer's should ultimately use? Hmm, I believe, there is no concrete answer. Nowadays, I use Emacs for relatively smaller projects and PyCharm for the behemoths. What do you think? What are you using for development?. Give your opinions/suggestions in the comment section.
