---
layout: post
title:  "Working with WAD"
date:   2022-02-10 11:30:00 -0700
categories: Python
published: false
---

A few weeks ago I started a project to work with the WAD game data file 
format. WAD is primarily a binary file format that stores large blocks 
of uncompressed data called "lumps" inside a single file. Originally 
created for Doom in 1993, the WAD format is agnostic to the data format 
of the blocks stored within it and can be used to store any file.

On first impression WAD stores data in a way that is easy and fast for a 
program to parse. The format specifies the location and size for each 
block of data. Loading data from a WAD is as simple as pointing to the 
specified location within the file and reading the exact amount of 
bytes. This differs from interpreting a text file where a program must 
search for an opening symbol, read data until encountering an end 
symbol, handle missing symbols, and so on.

## The WAD

Parsing a WAD is remarkably simple. A program must open the file in 
binary mode and read the first 12 bytes into appropriate variables. With 
that information it can then "seek" to the location of the WAD directory 
indicated by "infotableofs" and read each 16-byte entry until it has 
read the number of lumps indicated by "numlumps". This gives the program 
a list of every lump contained in the file.

In addition to a name, each entry in the WAD directory has a "filepos" 
and "size". The program can easily find an entry by searching for its 
name, or if possible simply its position in the directory (entry 10 or 
entries 51 through 60, for example). To read the lump data the program 
must simply seek to the "filepos" location and read "size" number of 
bytes.

## The Data

How to handle the actual data present in the WAD is specific to the 
program reading it. Doom engines read some lumps in the WAD by name and 
order, and ignore the rest. Missing data can lead to errors or crashing, 
as the engine expects a large amount of specific data to be present in 
specific positions. A WAD parser works in a different way. It doesn't 
expect certain lumps with certain names to be present in certain 
locations. Instead it relies entirely on the directory and works with 
all of the files no matter how many there are or how they're named.

Each of these applications and everything in-between make for an 
interesting mind exercise. Since WAD is normally a format for storing 
game data, I'll list some of the practical applications below.

- a program might use a WAD that contains image data lumps as sprites.
