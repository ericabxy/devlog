---
layout: post
title:  "Programs as Objects"
date:   2022-04-08 09:15:00 -0800
categories: short, programming
---

I am occasionally fascinated by the idea of a computer program 
encapsulated in a form that allows it to exist much like a physical 
object, e.g. a basketball. A ball seems to have an extremely simple and 
fundamental interface with which to physically interact with other 
objects. The interface seems to handle all "bugs" and "errors" 
gracefully and seems to "run" continuously as long as the ball is shaped 
like a ball. Obviously, the nature of computers (at present) makes 
things a lot more complicated, but there are many programming and 
software concepts that curiously approach the idea of an object.

**Object-Oriented Programming** has some obvious connections with the 
idea of programs as objects. Python has the curious ability to evaluate 
new Python code at runtime. Python objects also have introspection: a 
way of describing what attributes (variables and methods) it contains. 
In theory a running Python process could "pick up" arbitrary/unknown 
Python objects and discover how to interact with them through 
introspection. Objects with short, simple, and standardized methods can 
begin to resemble real-life physical objects; providing a generalized 
way for one program to interact with another.

**Functional Programming** has some rigorous conventions that approach 
the robust interface of a ball described above. A pure function accepts 
a well-defined value and returns another well-defined value. It will 
never fail or raise an exception: if the received value is invalid or 
unexpected it does nothing with it. It also does not depend on anything 
except the input; it is a self-contained *thing* with a well-defined and 
generic interface which can interact with the inputs and outputs of 
other functions and thus other programs.

**Powershell** applies some interesting concepts to the idea of a 
command-line interface. Instead of outputting text to the console, a 
program returns small standardized objects with well-defined variables 
and methods. An object can be fed into another program as an argument, 
and the new program can access and use its attributes via a generalized 
interface.

What do object-like programs look like? I don't have a well-defined 
vision, but I suspect such programs would simply interact with each 
other blindly through simple interfaces. An object-like music player 
with simple "back", "stop", "play", and "next" methods could be used by 
a generic GUI that has no idea what the music player is, but can see the 
four methods and simply attach a button to each method call. The player 
would have song information available as a generic string which the 
wrapper object would simply display. A couple more programs could 
interact similarly to allow the user to pick music files and feed them 
to the music player. Parts of this interaction could be replaced; for 
instance instead of a GUI program you might have a voice recognizer.
