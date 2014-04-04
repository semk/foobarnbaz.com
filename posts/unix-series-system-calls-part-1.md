---
title: "UNIX Series: System Calls - Part I"
date: '27-01-2014'
time: '20:45'
tags: ['Unix', 'Linux', 'System Calls']
layout: 'post.html'
---

System programmers must know about *system calls* and for for those who are not system programmers,
it is still good getting to know about how things work under the hood. My idea here is to do a series
of blog posts on UNIX/Linux system internals for the upcoming weeks. I'm doing this as a part of my
learnings from [CodeSchool](https://www.codeschool.com) and reading
*[Linux System Programming](http://shop.oreilly.com/product/9780596009588.do)*. Let's get started
with UNIX *System Calls* in the first place.

##### What are System Calls?

From *Linux System Programming* book,

> System programming starts and ends with system calls. System calls (often shortened to syscalls)
> are function invocations made from user space—your text editor, favorite game, and so on—into the
> kernel (the core internals of the system) in order to request some service or resource from the
> operating system. System calls range from the familiar, such as read() and write(), to the exotic,
> such as get_thread_area() and set_tid_address().

So system calls acts as an intermediary between userspace applications and the kernel space. It
allows programs to request the operating system to do something on its behalf. The system calls
are functions used in the kernel itself. If there was no clear distinction between the kernel
space and user space, then all the user applications will be able to do absolutely anything
they want to do in the operating system. Now that would be insecure and chaotic. System calls
exposes only those functionalities of the kernel through an API which will get only that task
done for the user application. In short, its a secure way of getting things done in the operating
system.

##### Some of the popular System Calls

All of us are atleast familiar with the `read()` and `write()` system calls. But applications need
even more of these. the following table shows the most commonly used system calls.

<table>
<colgroup>
<col style="text-align:left;"/>
<col style="text-align:center;"/>
<col style="text-align:right;"/>
</colgroup>

<thead>
<tr>
	<th style="text-align:left;">General Class</th>
	<th style="text-align:center;">Specific Class</th>
	<th style="text-align:right;">System Call</th>
</tr>
</thead>

<tbody>
<tr>
	<td style="text-align:left;">File related</td>
	<td style="text-align:center;">Creating a Channel</td>
	<td style="text-align:right;">creat()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;"></td>
	<td style="text-align:right;">open()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;"></td>
	<td style="text-align:right;">open()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;">Input/Output</td>
	<td style="text-align:right;">read()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;"></td>
	<td style="text-align:right;">write()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;">Random Access</td>
	<td style="text-align:right;">lseek()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;">Channel duplication</td>
	<td style="text-align:right;">dup()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;">Aliassing Files</td>
	<td style="text-align:right;">link()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;">Removing Files</td>
	<td style="text-align:right;">unlink()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;">File Status</td>
	<td style="text-align:right;">stat()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;"></td>
	<td style="text-align:right;">fstat()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;">Access Control</td>
	<td style="text-align:right;">access()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;"></td>
	<td style="text-align:right;">chmod()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;"></td>
	<td style="text-align:right;">chown()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;"></td>
	<td style="text-align:right;">umask()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;">Device Control</td>
	<td style="text-align:right;">ioctl()</td>
</tr>
<tr>
	<td style="text-align:left;">Process related</td>
	<td style="text-align:center;">Process creation</td>
	<td style="text-align:right;">exec()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;">Process creation</td>
	<td style="text-align:right;">fork()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;">Process wait</td>
	<td style="text-align:right;">wait()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;">Process termination</td>
	<td style="text-align:right;">exit()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;">Process Owner &amp; Group</td>
	<td style="text-align:right;">getuid()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;"></td>
	<td style="text-align:right;">geteuid()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;"></td>
	<td style="text-align:right;">getgid()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;"></td>
	<td style="text-align:right;">getegid()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;">Process identity</td>
	<td style="text-align:right;">getpid()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;"></td>
	<td style="text-align:right;">getppid()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;">Process control</td>
	<td style="text-align:right;">signal()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;"></td>
	<td style="text-align:right;">kill()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;"></td>
	<td style="text-align:right;">alarm()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;">Change working directory</td>
	<td style="text-align:right;">chdir()</td>
</tr>
<tr>
	<td style="text-align:left;">IPC</td>
	<td style="text-align:center;">Pipelines</td>
	<td style="text-align:right;">pipe()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;">Messages</td>
	<td style="text-align:right;">msgget()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;"></td>
	<td style="text-align:right;">msgsnd()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;"></td>
	<td style="text-align:right;">msgrcv()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;"></td>
	<td style="text-align:right;">msgctl()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;">Semaphores</td>
	<td style="text-align:right;">semget()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;"></td>
	<td style="text-align:right;">semop()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;">Shared memory</td>
	<td style="text-align:right;">shmget()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;"></td>
	<td style="text-align:right;">shmat()</td>
</tr>
<tr>
	<td style="text-align:left;"></td>
	<td style="text-align:center;"></td>
	<td style="text-align:right;">shmdt()</td>
</tr>
</tbody>
</table>

##### How System Calls are being called?

##### UNIX Process Address Space

##### References

[syscalls](http://www.di.uevora.pt/~lmr/syscalls.html)