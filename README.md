# cpmlife

An implementation of Conway's "Game of Life" for CP/M 2.2.

Version 0.1a, May 2021

## What is this?

This is yet another implementation of Conway's famous cellular
simulation for CP/M. It's designed to be used on a CP/M machine
with a fast connection to a VT100/ANSI terminal, because the
display is generated by sending terminal control codes.
In particular, I wrote this for the "Z80 Playground", a single-board
computer with a CP/M-compatible operating system. It also runs
on a Linux-based CP/M emulator, but it's much, much too fast to be
workable on a modern emulator.

I wrote `cpmlife` as an exercise in cross-compiling for Z80 on a 
Linux system. 

## Installation

Just copy `life.com` to a CP/M machine.

## Running

At present, nothing is configurable at run-time. The simulation
is on a 32x16 grid, which is about as much as the Z80 PG can 
cope with, without the display getting clunky. When it's running,
hit 'R' to restart the simulation with a different, random starting
position, or 'Q' to quit.  

## Building

`cpmfile` was intended to be cross-compiled by the `zcc` compiler from 
the Z88DK project:

https://github.com/z88dk/z88dk

With `zcc` and all its dependencies installed, building `cpmfile`
should amount to this:

    zcc +cpm -Wall -o life.com life.c -lndos

There's only one file, `life.com`, to be installed on the CPM machine.
The `ndos` switch selects a dummy file-handling library. Since this
program does not operate on files at all, this settings makes the
executable a kilobyte or so smaller.

`cpmlife` uses a C syntax that is too modern, I think, to be handled by
any native CP/M C compiler. In addition, it uses a tiny bit of 
Z80 assembly language, which relies on parameter-passing being done
the way `zcc` does it.

## Further work

It might be interesting to provide command-line switches, or some sort
of menu, to tweak the simulation settings. 

## Notes

`cpmlife` can be made to output VT52 codes rather than VT100, but only
by changing a compile-time setting. I'd expect it to be faster
with a VT52, because the cursor-movement codes are much shorter,
and do not require any numeric conversion to generate. I've tried
to speed up the number conversion by providing a lookup table
for numbers 1-80 -- no numbers outside this range will appear
in terminal control codes.

The simulation grid is stored as a two-dimensional C array but, in fact,
some parts of the code manipulate the grid it as if it were one-dimensional.
Since the grid is essentially read and written sequentially, row-by-row,
it's a lot faster to work this way, since the offsets into the grid
can be pre-computed, which reduces the amount of arithemetic that
has to be performed. However, this strategy makes the code pretty
unreadable in places.

## Legal stuff

There's nothing proprietary in here. Do what you like with it, 
with my compliments.  


