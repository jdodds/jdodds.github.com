---
layout: post
title: "tecs - boolean logic 1.1: background"
date: 2012-09-10 17:43
comments: true
categories: TECS book-runthroughs
---

This is part of a series of posts going through
[The Elements of Computing Systems](http://www1.idc.ac.il/tecs/). This post
covers the background section of the first chapter, which is about Boolean
Logic.

<!-- more -->

## Boolean Logic
All digital devices are made of logic gates. In this book, we'll start with a
Nand gate and build the other gates from it.
## Background
Boolean gates are implementations of Boolean functions.
### Boolean Algebra
A Boolean function operates on binary inputs and returns binary outputs.
#### Truth Table Representation
```
| x | y | z | f(x, y, z) |
|---+---+---+------------|
| 0 | 0 | 0 |          0 |
| 0 | 0 | 1 |          0 |
| 0 | 1 | 0 |          1 |
| 0 | 1 | 1 |          0 |
| 1 | 0 | 0 |          1 |
| 1 | 0 | 1 |          0 |
| 1 | 1 | 0 |          1 |
| 1 | 1 | 1 |          0 |
```
#### Boolean Expressions
The basic operators are "And", "Or", and "Not"

+ `x * y` is "x And y"
+ `x + y` is "x Or y"
+ `!x` is "Not x"

The table above could be represented as `f(x,y,z) = (x + y) * !z`.
#### Canonical Representation
Every Boolean function can be expressed as at least one Boolean Expression, the
"canonical representation"

For each row in the function's truth table that has a value of 1, create a term
by `And`ing literals that "fix" the values of the row's inputs.

```
| x | y | z | f(x, y, z) |
|---+---+---+------------|
| 0 | 0 | 0 |          0 |
| 0 | 0 | 1 |          0 |
| 0 | 1 | 0 |          1 |
| 0 | 1 | 1 |          0 |
| 1 | 0 | 0 |          1 |
| 1 | 0 | 1 |          0 |
| 1 | 1 | 0 |          1 |
| 1 | 1 | 1 |          0 |
```

For the third row, the term would be `!xy!z`. For the fifth, `x!y!z` and for the
seventh, `xy!z`. If we `Or` those three terms together, we'll have a boolean
expression that is equivalent to the given truth table, `!xy!z + x!y!z + xy!z`

#### Two-Input Boolean Functions

The number of Boolean functions that can be defined over `n` binary variables is
`2^2^n`

There are, therefore, sixteen functions possible over two boolean variables:

```
| Function    | x               | 0 0 1 1 |
|             | y               | 0 1 0 1 |
|-------------+-----------------+---------|
| Constant 0  | 0               | 0 0 0 0 |
| And         | x * y           | 0 0 0 1 |
| x And Not y | x * !y          | 0 0 1 0 |
| x           | x               | 0 0 1 1 |
| Not x And y | !x * y          | 0 1 0 0 |
| y           | y               | 0 1 0 1 |
| Xor         | x * !y + !x * y | 0 1 1 0 |
| Or          | x + y           | 0 1 1 1 |
| Nor         | !(x + y)        | 1 0 0 0 |
| Equivalence | x * y + !x * y  | 1 0 0 1 |
| Not y       | !y              | 1 0 1 0 |
| If y then x | x + !y          | 1 0 1 1 |
| Not x       | !x              | 1 1 0 0 |
| If x then y | !x + y          | 1 1 0 1 |
| Nand        | !(x * y)        | 1 1 1 0 |
| Constant 1  | 1               | 1 1 1 1 |
```

Both `Nand` and `Nor` are capable of constructing `And`, `Or`, and `Not` without
using any other functions. 

### Gate Logic
A `gate` is a device that implements a Boolean function. A Boolean function that
operates on `n` variables and returns `m` binary results will have `n` input
pins and `m` output pins.

Complex gates can be made from more elementary gates, the same way that complex
functions can be made from simpler functions.

Any technology that allows switching and conducting capabilities can be used to
transmit data between gates, they are not intrinsically tied to
electricity. This allows computer scientists to work on a more abstract level.

#### Primitive and Composite Gates
Logic gates can be chained together to create "composite gates". If we wanted to
make a gate that implemented `And(a,b,c)`, we could do it as `And(And(a,b),c)`.

If we wanted to implement `Xor(a,b)`, we could do so as
`Or(And(a,Not(b)),And(Not(a),b))`.

The function being implemented -- `Xor(a,b)` or `And(a,b,c)` -- can only be
represented in one way when referring to it's high-level interface, but the
implementation of that interface -- `Or(And(a,Not(b)),And(Not(a),b))` or
`And(And(a,b),c)` -- might be done in many ways. For instance, it's possible to
implement `Xor` using only four gates rather than five.

### Actual Hardware Construction
We could, of course, actually acquire hardware and wire it up to test potential
implementations of interfaces needed. This would, however, be rather tedious.

### Hardware Description Language (HDL)
Hardware designers generally write an HDL (or VHDL) program which can be run in
a simulator and subjected to a bunch of tests and performance measurements.

An HDL definition of a chip consists of a "header" section, and a "parts"
section. The header specifies the chip's interface -- it's name and the names of
it's input and output pins. The parts section describes the names and
connections between the lower-level parts that make up the chip. This requires
that the designer have complete docs of the interfaces of the lower-level parts.

Here's an HDL program describing `Xor`:

{% include_code tecs/boolean-logic/background/xor.vhdl %}

Most hardware simulators are designed to be able to run test scripts, here's an
example of one written for Xor:

```
load Xor.vhdl,
output-list a, b, out;
set a 0, set b 0,
eval, output;
set a 0, set b 1,
eval, output;
set a 1, set b 0,
eval, output;
set a 1, set b 1,
eval, output;
```

And it's possible output:

```
| a | b | out |
|---+---+-----|
| 0 | 0 | 0   |
| 0 | 1 | 1   |
| 1 | 0 | 1   |
| 1 | 1 | 0   |
```
