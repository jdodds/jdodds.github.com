---
layout: post
title: "TECS: Introduction"
date: 2012-09-03 11:29
comments: true
categories: TECS book-runthroughs
---

This starts a series of posts that will follow my progress while reading
[The Elements of Computing Systems](http://www1.idc.ac.il/tecs/), which so far
seems like a very excellent book. I'll be taking notes as I go along and doing
the exercises.

I'm posting these partially as a motivator for myself to finish the book in
entirety, and just in case someone finds them useful.

Anyhow, on to the start of the runthrough. This post just covers the
introductory material.

<!-- more -->

## The World Above

{% include_code tecs/introduction/hello.jack lang:java %}

To run something like this, it must be parsed, "understood", and then expressed
in a form that the machine it's being run on can execute -- a process called
"compilation".

The end result of compilation, machine-level code, is an abstraction on top of
some "hardware architecture", which sits on top of a certain "chipset", made up
of logic gates made out of "switching devices" like transistors. That's as far
down the abstraction ladder we'll go.

### Abstractions
A good modular design implies that you can work on individual modules while
ignoring the rest of the system.

In computer science, abstraction is defined as a statement of what a module
does, ignoring the details of how it does it.

### The World Below

We're going to take a "bottom-up" approach to describing the systems of
abstraction in a modern computing system, starting with basic logic gates.

Below, we'll survey the book plan in the opposite direction.

#### High-Level Language Land
Where people dream up applications and write software that implements them.

#### The Road Down to Hardware Land
Taking a program and compiling it into the machine language of a target
platform.

Source text s analyzed and grouped into a "parse tree., which is then processed
to create a program written in an intermediate language executable by a
stack-based virtual machine, which outputs a large assembly program which can be
executed by an assembler.

#### Hardware Land
Described with HDL.
