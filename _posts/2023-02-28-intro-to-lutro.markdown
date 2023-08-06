---
layout: post
title:  "Introducing Lutro"
date:   2023-02-28 02:48:28 -0800
categories: programming, lutro, libretro, retroarch, lua
published: false
---

Developing video games for a game console or dedicated gaming PC is an
aspirational goal for a game programmer. Getting started, however, is
not the most straighforward task. Often it involves having access to
developer hardware, or a proprietary interactive development
environment, or an expensive software development kit.

I'm using my devlog to describe and document my experience using one
inexpensive and relatively easy platform for game development intended
for game consoles.

Introducing LibRetro and Lutro
------------------------------

LibRetro is a simple API that powers the RetroArch gamimg and
emulator frontend. RetroArch runs on many personal computers, game
consoles, and mobile devices. Lutro is a LibRetro core that allows you
to write games in the Lua programming language and play them anywhere
e.g. RetroArch runs.

Lutro Versus LÖVE
-----------------

Lutro uses a subset of the LÖVE API. In fact with a little care
anything written to run with Lutro can run with LÖVE and vice versa.
There are a few things to keep in mind when programming Lutro.






Here is how to write a simple Lutro application that will run on
either.

First of all, the Lutro core will crash if there is no `lutro.update`
function. Secondly, the Lutro core will recognize either `lutro` or
`love` as a prefix to the API functions, so you may choose to use
either. Either way, the main file must be saved as `main.lua` to be
recognized. Thus, a lutro `main.lua` can look like either of the
following.

> `function love.update(dt)`
> `end`

> `if not lutro then lutro = love end`
> 
> `function lutro.update(dt)`
> `end`

Conclusion
----------

This post is meant as a brief introduction to developing Lutro games,
assuming you have some experience with the tools involved already.



Libretro - A crossplatform application API

[1]: https://www.libretro.com/

Lutro - A portable game engine for retro games

[2]: https://lutro.libretro.com/

Lutro LÖVE API Comparison

[3]: https://github.com/libretro/lutro-status

Programming in Lua

[4]: https://www.lua.org/pil/contents.html
