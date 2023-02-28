---
layout: post
title:  "The Hidden Balls of Doom II"
date:   2022-12-07 09:50:00 -0700
categories: game development, game modification
published: true
---

While working with the original *Doom II* (1994) game data, I noticed 
among the various projectiles in the sprite list two of them named 
`SPR_BAL1` and `SPR_BAL2`, and another one named `SPR_BAL7`. These 
sprites are referenced in the engine's state table as `S_TBALL`, 
`S_RBALL`, and `S_BRBALL` respectively. They are obvious references to 
sequentially-numbered "balls" or projectiles, but none of the other 
projectiles are numbered in this way. Consequently I began to wonder 
where the "missing" 3rd through 6th balls are and whether there is an 
8th ball and so on.

The answer to Doom's missing "balls" appears to lie in the state table 
itself. The states corresponding to `SPR_BAL1` and `SPR_BAL2` appear 
early in the table, shortly after the player weapon states and before 
the player weapon projectiles, whereas those for `SPR_BAL7` occur later 
in the table next to `S_BOSS`, with several projectiles in between. So, 
lets count them up!

Immediately after `S_RBALL` (`SPR_BAL2`) is `S_PLASBALL`, the Player's 
plasma gun projectile, using sprites `SPR_PLSS` and `SPR_PLSE`. Skipping 
the `S_ROCKET` (not a ball), next is `S_BFGSHOT`, corresponding to 
sprites `SPR_BFS1`, `SPR_BFE1`, and `SPR_BFE2`. These are the last of 
the Player projectiles and the 3rd and 4th balls in the state table.

The next part of the state table lists all of Doom's enemies, which 
usually have their projectile states listed nearby if they have any. The 
first of these is `S_TRACER`, using sprites `SPR_FATB` and `SPR_FBXP`. 
Then comes `S_FATSHOT` which uses `SPR_MANF` and `SPR_MISL`. No more 
projectiles are listed for a while until, sure enough, `S_BRBALL` 
appears corresponding to sprite `SPR_BAL7`! These three match up 
perfectly to a 5th, 6th, and 7th ball. The only remaining projectile in 
the game comes next, after a few more monsters, `S_ARACH_PLAZ` using the 
`SPR_APLS` and `SPR_APBX` sprites; an 8th ball.

How Did Did This Happen?
------------------------

Why did *Doom II* lose or hide most of its balls? Its hard to say for 
sure. At early stages of development, the Plasma Gun and the BFG both 
shot the same two green and red projectiles, and the Imp wielded the 
same red projectile as both of those weapons. I think its possible 
`SPR_BAL1` and `SPR_BAL2` were the first projectiles created by the 
developers, added to the state table early on to be used by most of the 
weapons and enemeies, but subsequently they created custom projectiles 
for the weapons leaving `SPR_BAL1` and `SPR_BAL2` to be used only by 
those enemies.

However `SPR_BAL7` isn't as easy to explain. The Bruiser enemy that uses 
it was created for the original Doom, while those I identified as 5th 
and 6th balls were derived from `SPR_BAL7` and `SPR_SKUL` for *Doom II*, 
making it unlikely that they were intended to be the 5th and 6th balls 
originally. The latest available releases of *Doom* and *Doom II* use 
the same engine, state table, and sprite names (unified for release 
version 1.9), so in order to find out what happened chronologically 
requires examining some older versions of the game and data. In any 
case, the order in which projectiles appear in the state table *now* 
offers a convincing picture of how they are named.

Other Notes
-----------

Naming conventions and oddities are some of my favorite things to 
scrutinize. Here are some other things I discovered while examining 
*Doom II*'s projectiles.

The Imp and Cacodemon projectiles are named in the state table `S_TBALL` 
and `S_RBALL`. The Imp is known as "Troop" in the source code and 
sprites (`MT_TROOP`, `SPR_TROO`, `A_TroopAttack`, `MT_TROOPSHOT`) which 
explains "TBALL", but the Cacodemon is known as "Head" (`MT_HEAD`, 
`SPR_HEAD`, `A_HeadAttack`, `MT_HEADSHOT`) which doesn't explain 
"RBALL". Perhaps it stands for "Red Ball" or some other abbreviation 
that is lost to time.

The Baron of Hell projectile is named `S_BRBALL` in the sprite table, 
which is an obvious reference to the Barons name in the engine, 
"Bruiser" (`MT_BRUISER`, `A_BruisAttack`, `MT_BRUISERSHOT`).

To fire a projectile an enemy enters its `missilestate`, suggesting the 
canonical name for projectiles is "missile". However, various names 
suggest each projectile can also be considered a "shot" or a "ball" with 
a subsequent "explosion".

The Revenant projectile sprites are called `SPR_FATB` and `SPR_FBXP` 
("FAT Ball" and "Fat Ball eXPlosion"), suggesting it was at one point 
intended to be a projectile for the Mancubus which is known in the 
engine as `MT_FATSO` and `SPR_FATT`. The Revenant projectile is known in 
the engine as `MT_TRACER`.

The Mancubus' actual projectile sprite is named `SPR_MANF` ("MANcubus 
Fireball"), and is one of the only sprite names that doesn't refer to 
its engine name `MT_FATSHOT` and `S_FATSHOT`. Instead of using its own 
custom explosion sprite, this projectile switches to `SPR_MISL` as it 
explodes.

The Arachnotron's projectile seems to be called "plazma" with a "z", as 
opposed to the Player's "plasma" with an "s" (`MT_ARACHPLAZ`).
