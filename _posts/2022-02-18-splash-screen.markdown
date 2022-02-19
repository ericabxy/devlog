---
layout: post
title:  "The Splash Screen"
date:   2022-02-18 21:38:00 -0800
categories: gamedev
---

Similar to their motion picture counterparts, video games began to 
present "splash" screens in the early 90s as a matter of routine before 
giving the player control of the main menu or playing a short intro 
video. I'm talking about the silent black screens with a single logo in 
the center that would fade in, remain for two seconds, then fade back 
out. Or the logo would be accompanied by some sound or music with a flat 
color or gradient background. There was usually one splash for the 
developer and one for the publisher of the video game. Seeing a splash 
screen before the game starts evokes a sense of professional polish.

Since I have been focusing on minimalism in game design, I haven't had 
the opportunity to present splash screens in a long time. However, as an 
amatuer developer I have used some ideas before about how to use splash 
screens to tap into that sense of polish *and* give some proper credit 
even though I have no publisher and the developer is simply my self and 
not a company with a flashy name.

When developing a game I rely heavily on a multimedia engine to take 
care of the complicated bits. Most game developers will. When 
programming in Python I've used a module called 
[Pygame](https://www.pygame.org/). In Lua, I use an excellent framework 
called [LÖVE](https://love2d.org/). If I wrote in C I'd probably use 
[Simple DirectMedia Layer](http://www.libsdl.org/). For this reason I'll 
put this engine's logo on my first splash screen, informing the player 
who's behind the audio-visual programming.

Unless otherwise motivated, I release all of my games using a software 
license that neatly defines the player's rights in posession of my 
software. I usually choose the [GNU General Public License 
v3.0](https://www.gnu.org/licenses/gpl-3.0.en.html). To let the player 
know that they've been granted some rights under copyright law, I put 
the official logo for this license on my second splash screen.

Each of these logos has a direct correlation with the concept of a 
developing company and a publishing company respectively. The 
organization and contributors behind Pygame did a massive amount of 
development to give people the ability to write games in Python, and the 
organization at [GNU.org](https://www.gnu.org/) has been working for 
decades to help people like me publish software under copyright terms 
that I agree with.

On a similar note, when my game includes a credit sequence at the end 
that's an appropriate place list myself as Lead Programmer and credit 
anyone else who contributed art or code to the final product. This can 
bring attention to artist's work and puts another coat of professional 
polish in a traditional place.
