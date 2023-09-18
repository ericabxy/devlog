---
layout: post
title:  "Old is New: Backgrounds and Objects"
date:   2021-12-18 11:57:00 -0700
categories: programming, computer science
published: true
---


When programming for modern architecture with modern languages, I find 
it useful to use techniques from game development history even when the 
limitations are no longer relevant. This calls into the question the 
nature of historical concepts like nametables, characters, and objects 
when their configurations are nearly limitless compared to an age ago.

### Backgrounds

Background rendering systems previously used nametables, blocks of 
memory that referred to a list of e.g. 8x8 pixel characters (tiles) to 
render every frame in a pattern 64x64 characters large with a certain 
offset position with respect to the screen. Modern libraries can more 
efficiently render an entire bit-by-bit direct-color image, but you can 
still use the traditional concept for ease and clarity.

Background tilesheets save time and space, so you can construct a 
function that draws e.g. 32x32 pixel tiles from a tilesheet onto a 
canvas 1024x1024 pixels large and send it to a draw routine with window 
offset information. With the same system you could also render full-size 
hand-drawn backgrounds just as easily and switch between them at will. 
With transparency you can layer multiple backgrounds with ease.

### Objects

Objects used to be stored in a small space of memory with limited 
information that the rendering system would use to quickly draw moving 
images to the screen every frame. Objects had x and y values, priority 
information, transformation flags, and similar to nametables referred to 
tiles in memory. Modern libraries have various and similar capabilities 
for rendering any amount of individual images, but you can use the 
concept of objects again for ease and clarity of development.

Your objects can have any amount of values and transformation flags. An 
individual object can easily refer to any image to pick tiles from, and 
a spritesheet is a very manageable image. Each object can refer to a 
sheet to render from, any individual tile within that image, nearly 
limitless x and y values, any transformations the library is capable of, 
etc.. Thereafter the draw routine can iterate through the list of 
objects and render them each to the screen with a short for loop.

### Font Layers

Classic games used to render text to the background as tile just like 
the graphics tiles, but with modern fonts and text rendering functions 
its easy to simply draw a text layer or print text onto an existing 
background.

### Use Your Library's Standards

Depending on the language you're using and the game development library 
you're building with, for the sake of clarity its a good idea to use the 
library's naming standards where possible. For example, when programming 
games for LÖVE a tilesheet is an *image*, a tile is a *quad*, a sound 
effect is a *source*, and so on. Concepts that are not covered by the 
library's names--like *background*, *layer*, and *object*--are fair game for 
your own coinage. Python refers to objects as *sprites*.

Even though you're capable of writing complicated functions that 
manipulate graphics in ways the library cannot, its a useful exercise to 
limit your program to the capabilities of the library itself. If the 
library has a "draw" function that can perform offsets and transforms, 
your objects' information can conform to these standards. For example, 
LÖVE is capable of rendering a tile from a larger image at a specific 
floating-point x and y coordinate, rotate that image, size or flip it, 
and can even shear the tile. Lutro is is not capable of transforms, so 
my drawable objects in Lutro have fewer fields related to the draw 
routine.

### Numbers and Powers of Two

For purely aesthetic reasons, it can be convenient to conform to 
graphical conventions that are no longer relevant; for instance, storing 
bullet sprites on tilesheets that are 8x8 per tile, or painting a 
scroll-able background on a canvas 1024x1024 pixels in size. On the 
other hand, the unlimited nature of images in modern programming allows 
for a more custom approach. You can use 15x15 tiles in case your sprites 
have an odd number of pixels. This allows you to center them in the 
frame of each tile. You can use a scrolling background that's simply two 
times the size of the screen, or two times the size of the pre-rendered 
image.
