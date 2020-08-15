---
layout: post
title: n2bd - processes
---

{{ page.title }}
================

<p class="meta">15 August 2020 - Istanbul</p>

if you read any of my previous blogs, [the challenge](https://www.cagataytanyildiz.com/2020/07/03/the-challenge.html), [the dilemma of the corporate](https://www.cagataytanyildiz.com/2020/06/16/the-dilemma-of-the-corporate.html) or [feeling tired](https://www.cagataytanyildiz.com/2020/03/14/feeling-tired.html), you can say that I have problems in my career and you are right. I was searching for alternative ways to handle my complains, I found [this](https://medium.com/@enbilgin/what-i-have-learned-about-landing-a-job-in-tech-2-be-a-marathon-runner-41974beb3f46) marvelous article of [Enes Bilgin](https://www.linkedin.com/in/enesbilgin/). it was the last drop on the glass. I convinced myself that complaining is not gonna help me. I have to admit that, complaining is way easier than doing something for change. so I planned some side gigs for me for sharpening my skills.

today I would like to introduce my new series "**note to backend developer (n2bd)**". with this series I'm going to write about some valuable topics from my perpective. sometimes we will talk about some sections of the state of art course books. sometimes we will dig some article the internet. if you have anything to suggest feel free to reach me over [Telegram](https://t.me/cagtaa).

as a starting point, I would like to refresh my memory on modern operating systems. it was one of my favorite courses back in college. I think I'm the second most heard person on that course's audio recording because of my questions.

when it comes to operating systems [Tanenbaum's Modern Operating Systems](https://www.amazon.com/Modern-Operating-Systems-2nd-GOAL/dp/0130313580) book is one of the best resource that I can refer to. what I'm planning to do is I will study one subject at a time and I will share my notes along this journey as a blog post for each topic. after all this long introduction let's start with processes.

# processes

> **a process** is just an executing program, including the current values of the program counter, registers, and variables.

> **a program** an algorithm expressed in some suitable notation.

> **a daemon** process that stays in background to handle some activities such as email, web pages, etc.

## 1 - process creation

four principal events that cause processes to be created:

* system initialization
* execution of a process creation system call by a running process
* a user request to create a new proccess.
    - opening browser, email, etc.
* initiation of a batch job.
    - mainframe runs following job from the queue

### unix: fork

**[fork](https://www.tutorialspoint.com/unix_system_calls/fork.htm)** creates exact clone of calling process. the parent and the child have same memory image, environment strings and some open files.

another system call ([execve](https://www.tutorialspoint.com/unix_system_calls/execve.htm) or similar call) change its memory image and run a new program.

**the reason** for this two-step process is to allow the child manipulate it's file descriptors after the fork but before the execve to accomplish redirection of standard input, standard output, and standard error.

### windows: CreateProcess
**CreateProcess** handles both process creation and loading the correct program into new process.

## 2 - process termination

sooner or later the new process will terminate, usually due to one of the following conditions:

* normal exit (voluntary)
    - unix: exit
    - windows: ExitProcess
* error exit (voluntary)
* fatal error (involuntary)
* killed by another process. (involuntary)
    - unix: kill
    - windows: TerminateProcess

## 3 - process hierachy

**unix**, a process and all of its children and further descendants together form a process group. when user sends a signal from the keyboard, the signal is delivered to all members of the process group currently associated with the keyboard. individually, each process

* can catch the signal
* can ignore the signal
* or take default action, which is to be killed by signal.

**windows**, does not have any concept of a process hierarchy.

## 4 - process states

- running
- ready
- blocked

possible transaction between these states:

- process blocks for input (running to blocked)
- scheduler picks another process (running to ready)
- scheduler picks this process (ready to running)
- input becomes available (blocked to ready)

## 5 - implementing process model

to implement the process model, the operating system maintains a table, which called a **process table**, with one entry per process. each entry is known as a  **process control block**.

some of the fields of a typical process table entry:

- **process management**
    - registers
    - program counter
    - program status word
    - ...
- **memory management**
    - text segment pointer
    - data segment pointer
    - stack segment pointer
- **file management**
    - root directory
    - working directory
    - file descriptors
    - ...


