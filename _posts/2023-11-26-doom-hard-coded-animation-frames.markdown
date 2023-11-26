---
layout: post
title:  "Doom's Hard-coded Animation Frames"
date:   2023-11-26 10:01:00 -0800
categories: doom, programming
published: true
---

Doom has some well-known hard-coded animation states that interfere with 
third-party game modifications like DeHackEd. Many of them are in the 
PSprites (p_pspr.c) source file due to a sublte work-around that give 
the player special animation frames.

The player's sprite animation is different from all other map objects 
(mobjs). Every mobj has a `meleestate` and a `missilestate` which are 
used in various ways to control and telegraph their behavior. However, 
the player mobj essentially has a third attack state that is not coded 
into the object. Here is how the three attack states work.

__meleestate:__ set to `null`. Melee attacks are initiated by the player 
so if the mobj is not player-controlled it will never try to do a close 
attack.

__missilestate:__ set to S_PLAY_ATK1 and used by most weapon functions 
to make the mobj enter its attack state. This state lasts for 12 tics, 
then swithches the mobj directly back to `spawnstate`. It is a single 
frame showing the player sprite raising its gun to fire.

__not coded:__ there is no entry point to this state in the mobj info 
and it is not connected to other state. It is usually entered by 
referencing the S_PLAY_ATK2 macro value directly from a function. This 
state lasts for 6 tics, is displayed at full brightness, then switches 
to S_PLAY_ATK1. The single frame shows the player sprite firing its gun 
with a bright muzzle flash.

This is essentially a hack to allow the engine to arbitrarily alternate 
between showing the player with a raised weapon and a firing weapon, 
which is used to telegraph each player's actions during multiplayer and 
match the sounds that are being played. However, when it comes to 
third-party mods this hack presents special problems that must be worked 
around. The player states must be carefully left in-place, because 
essential functions in the engine refer to these states in a way that 
can't be changed if the states are re-arranged, unless the engine is 
re-compiled.

I believe more source ports ought to be updated to eliminate this hack, 
but many of the best ports aim for backward compatibility which could be 
broken by fixing it, or which simply puts a bugfix out-of-scope. 
Nonetheless, I can think of some simple solutions that are not very 
intrusive.

1. Add new states to map objects to allow for special animation frames. This is the most obvious solution, as the hack was explicitly created to work around the lack of a special animation frame. It could be named "flashstate" and be called from weapon functions by referencing actor->info->flashstate. It would be fully configurable by third-party DeHackEd mods with the Flashing frame.

2. A close look at player mobj states reveals that its attack frames 
could be fully connected in reverse-order. If `missilestate` referred to 
S_PLAY_ATK2, the next frames would be S_PLAY_ATK1 and the S_PLAY, 
completing the entire sequence. This change would necessitate swapping 
references to each of the two frames in the code, but would avoid the 
need to add a new state to every mobj info table. However, it could be 
difficult to track down and correct every usage that relies on the 
original order.

I'm currently using this knowledge and theory to implement a fix in my 
fork of LibRetro PrBoom, calling the fork All Hell as it is intended as 
a collection of bugfixes and generalizations for Doom modding.
