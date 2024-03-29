---
layout: post
title:  "Important Lessons From Heroine Dusk"
date:   2023-09-18 12:39:00 -0800
categories: pixel art, game design
published: true
---

Heroine Dusk is a game demo that contains of a set of scenery tiles 
drawn by Clint Bellanger. They are designed to depict a first-person 
perspective aligned to a square grid of snappy movement. While designing 
new maps using these tiles, I've learned several important techniques 
for working with this particular first-person perspective.

Use Tight Spacing in Every Map
------------------------------

Bellanger's tileset works best in close-quarters. Most terrain should be 
spaced no more than three tiles apart. This will make your maps seem 
tiny with little space to place objects and walk around, but in-game the 
player will keep their sense of direction when they can always see what 
walls, trees, and buildings are around them. Wide-open spaces as little 
as 4 tiles squared disrupt the player's ability to maintain a mental map 
of the area they are in.

This is an advantage rather than a drawback. The tileset causes maps to 
be minimal and easy to work with. This design aspect can also be used in 
reverse: if the goal of a part of the game is to make the player feel 
lost or deep in the wilderness, spacing objects and terrain too far 
apart can evoke this feeling rather quickly.

Mind the Four-way Nature of Tiles
---------------------------------

A quirk of this tileset is none of the tiles can have a directional 
aspect. This is mostly fine for walls and pillars, but becomes very 
noticeable when placing doors. The door tile shows a door from every 
direction, which usually goes against any sense of inner space you are 
designing into you map. In each map of the demo, Bellanger is careful to 
always surround a door tile on every side except one, so that only one 
door is ever shown on any one tile leading into (or out of) a structure.

This has the disadvantage of making it difficult to put a door near the 
corner of a house or wall, but it is a trade-off in the design of such a 
minimalistic tile system.

Doors (and walls) in this tileset are essentially and optical illusion. 
If you were able to view a door from all angles, you would see a square 
structure with doors on all four sides opening toward each other, with 
no actual living space inside the block. Surrounding the door on all 
sides generates the illusion of something with walls much thinner than a 
single tile, and plenty of space inside. This illusion works with the 
outer walls of a structure viewed from inside as will. Through clever 
use of map transitions, a doorway into and out of a town or building can 
seem as if it is between two tiles, making the wall feel reasonable 
thick instead of the width of an entire tile.

Choose the Right Background
---------------------------

Bellanger provided two different backgrounds to use during exploration, 
and there is a good reason for each of them. These backgrounds are meant 
to display behind the rendered tiles. `nightsky` depicts a period of 
twilight, just after the sun has set and there is still some glow left 
in the sky; `tempest` depicts a moonlit scene of deep night.

`nightsky` is designed for use in outdoor maps. Using it indoors, i.e. 
on maps with ceiling tiles, can result in the player catching a glimpse 
of the night sky in the distance even though there are walls and 
ceilings in the way. This is due to render distance; walls that are more 
than 3 tiles away from the player are invisible, revealing the 
background.

`tempest` is designed for both indoor and outdoor maps. The middle third 
of this background is dark, with no hint of the moon visible. Because of 
this, walls that are invisible due to render distance do not reveal any 
sort of outdoor sky. Unless there is no ceiling, in which case seeing 
the night sky is expected.

Avoid Symmetry
--------------

Its easy to fall into the trap of designing maps with symmetical 
structutes. It can be especially disorienting with this tile system, 
since the player's view can look identical from certain vantage points. 
The demo provides a handy compass so the play will know when they change 
direction, but it doesn't look great visually. Its best to avoid 
horizontal and vertical symmetry to make the world look good while it is 
being explored. Consider breaking up a square-shaped area with a single 
terrain feature, and other techniques.
