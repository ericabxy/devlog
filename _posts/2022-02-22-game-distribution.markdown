---
layout: post
title:  "Game Distribution"
date:   2021-02-22 10:57:25 -0800
categories: video games, game development
published: false
---

Software distribution--the process of consolidating a program or suite 
of programs into a "shippable" form intended for one or more "target" 
operating systems--is a particular interest of mine. From meticulously 
maintained software repositories like Debian to the MacOS-like AppImage 
format, or from dependency hell to standalone binaries, I love to figure 
out how best to get software from developer to end-user in a working 
state.

One of my favorite methods is that of having a "runner" installed on the 
target system associated with a particular file format. Examples of this 
include LÖVE and PrBoom+, both of which--if installed--will simply run a 
".love" file or a ".wad" file respectively if you double-click it, 
dropping you directly into the game. The ".love" or ".wad" file in this 
case behaves a lot like a game cartridge or game ROM, and feels very 
intuitive to use since double-clicking is a common desktop interaction.
