---
layout: post
title: "TECS - boolean logic 1.2: specification"
date: 2012-10-05 12:18
comments: true
categories: TECS book-runthroughs
---

This is part of a series of posts going through
[The Elements of Computing Systems](http://www1.idc.ac.il/tecs/). This post
covers section 1.2, the specification of a typical set of gates, which will be
built on top of later.

We'll be starting with a `Nand` gate, and deriving all other gates from it.

<!-- more -->

## The `Nand` Gate

The `Nand` gate computes the following:

```
| a | b | Nand(a,b) |
|---+---+-----------|
| 0 | 0 |         1 |
| 0 | 1 |         1 |
| 1 | 0 |         1 |
| 1 | 1 |         0 |
```

And can be specified as:

```
Chip Name : Nand
Inputs    : a, b
Outputs   : out
Function  : If a=b=1 then out=0 else out=1
Comment   : This gate is considered primitive, there is no need to implement it.
```

## Basic Logic Gates

These gates are referred to as "elementary" or "basic", but we do not consider
them "primitive" because they can be created from `Nand` gates.

### Not

Also known as a "converter", sets its output to whatever its input isn't.


```
Chip Name : Not
Inputs    : in
Outputs   : out
Function  : If in=0 then out=1 else out=0
```

### And

Returns 1 if and only if both of its inputs are 1.

```
Chip Name : And
Inputs    : a, b
Outputs   : out
Function  : If a=b=1 then out=1 else out=0.
```

### Or

Returns 1 if at least one of its inputs is 1.

```
Chip Name : Or
Inputs    : a, b
Outputs   : out
Function  : If a=b=0 then out=0 else out=1.
```

### Xor

"Exclusive or", returns 1 if its inputs have opposite values.

```
Chip Name : Xor
Inputs    : a, b
Outputs   : out
Function  : If a<>b then out=1 else out=0.
```

### Multiplexor

A three-input gate, it uses one input to select as output the value of one of
the other two inputs. The bit used to select is called the "selection bit", the
other two inputs are called "data bits".

```
Chip Name : Mux
Inputs    : a, b, sel
Outputs   : out
Function  : If sel=0 then out=a else out=b
```

`Mux` calculates the following function:


```
| a | b | sel | out |
|---+---+-----+-----|
| 0 | 0 | 0   | 0   |
| 0 | 1 | 0   | 0   |
| 1 | 0 | 0   | 1   |
| 1 | 1 | 0   | 1   |
| 0 | 0 | 1   | 0   |
| 0 | 1 | 1   | 1   |
| 1 | 0 | 1   | 0   |
| 1 | 1 | 1   | 1   |
```

Which could be abbreviated as:

```
|sel|out|
|---+---|
| 0 | a |
| 1 | b |
```

### Demultiplexor

Takes a single input and sends it to one of two outputs depending on the value
of a selector bit.


```
| sel | a  | b  |
|-----+----+----|
|   0 | in | 0  |
|   1 | 0  | in |
```

```
Chip Name : DMux
Inputs    : in, sel
Outputs   : a, b
Function  : If sel=0 then {a=in, b=0} else {a=0, b=in}
```

## Multi-Bit Versions of Basic Gates

Most computer hardware is meant to work on multi-bit arrays called "buses". A
32-bit computer needs to be able to compute `And` on two 32-bit buses. We can
build an array of 32 separately-operating binary `And` gates, and stick it in a
single chip with two 32-bit input buses and one 32-bit output bus.

Here, we're going to look at a set of multi-bit logic gates needed for a 16-bit
computer. It's worth noting that the architecture of `n`-bit logic gates is not
very dependent on the value of `n`.

### Multi-Bit Not

```
Chip Name : Not16
Inputs    : in[16]
Outputs   : out[16]
Function  : for i=0..15 out[i]=Not(in[i]).
```

### Multi-Bit And

```
Chip Name : And16
Inputs    : a[16], b[16]
Outputs   : out[16]
Function  : For i=0..15 out[i]=And(a[i],b[i]).
```

### Multi-Bit Or

```
Chip Name : Or16
Inputs    : a[16], b[16]
Outputs   : out[16]
Function  : For i=0..15 out[i]=Or(a[i],b[i]).
```

### Multi-Bit Multiplexor

```
Chip Name : Mux16
Inputs    : a[16], b[16], sel
Outputs   : out[16]
Function  : If sel=0 then for i=0..15 out[i]=a[i] else for i=0..15 out[i]=b[i].
```

## Multi-Way Versions of Basic Gates

A lot of logic gates that accept two inputs generalize easily to gates that
accept an arbitrary number of inputs.

### Multi-Way Or

```
Chip Name : Or8Way
Inputs    : in[8]
Outputs   : out
Function  : out=Or(in[0],in[1],...,in[7]).
```

### Multi-Way/Multi-Bit Multiplexor

An `m`-way `n`-bit multiplexor chooses one of `m` `n`-bit input buses and
outputs it to one `n`-bit output bus. Selection is controlled by `k` control
bits, where `k = log2m`. 

The architecture we're making in the book requires a 4-way 16-bit multiplexor,
and an 8-way 16-bit multiplexor.

```
Chip Name : Mux4Way16
Inputs    : a[16], b[16], c[16], d[16], sel[2]
Outputs   : out[16]
Function  : If sel=00 then out=a else if sel=01 then out=b else if self=10 then out=c
            else if sel=11 then out=d
Comment   : The assignment operations mentioned above are all 16-bit. For
            example, "out=a" means "for i=0..15 out[i]=a[i]".
```

```
Chip Name : Mux8Way16
Inputs    : a[16], b[16], c[16], d[16], e[16], f[16], g[16], h[16], sel[3]
Outputs   : out[16]
Function  : If sel=000 then out=a else if sel=001 then out=b else if sel=010 then
            out=c ... else if sel=111 then out=h
Comment   : The assignment operations mentioned above are all 16-bit. For
            example, "out=a" means "for i=0..15 out[i]=a[i]".
```

### Multi-Way/Multi-bit Demultiplexor

An `m`-way demultiplexor takes a single `n`-bit input to one of `m` `n`-bit
outputs. Selection is controlled by a set of `k` control bits, where `k=log2m`.

We'll need a 4-way 1-bit demultiplexor, and an 8-way 1-bit multiplexor.

```
Chip Name : DMux4Way
Inputs    : in, sel[2]
Outputs   : a, b, c, d
Function  : If sel=00 then      (a=in, b=c=d=0)
            else if sel=01 then (b=in, a=c=d=0) 
            else if sel=10 then (b=in, a=b=d=0) 
            else if sel=11 then (d=in, a=b=c=0)
```

```
Chip Name : DMux8Way
Inputs    : in, sel[3]
Outputs   : a, b, c, d, e, f, g, h
Function  : If sel=000 then      (a=in, b=c=d=e=f=g=h=0)
            else if sel=001 then (b=in, a=c=d=e=f=g=h=0) 
            else if sel=010 ...
            ...
            else if sel=111 then (h=in, a=b=c=d=e=f=g=0)
```
