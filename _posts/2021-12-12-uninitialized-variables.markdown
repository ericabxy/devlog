---
layout: post
title:  "Uninitialized Variables"
date:   2021-12-12 13:25:00 -0700
categories: programming, lua
---

As a Lua programmer I like to take advantage of the default value of 
**nil** assigned to variables and fields. **nil** evaluates in a similar 
way as **false** in an **if** statement, and this behavior can be used 
in various ways.

## using **nil** instead of initializing to **false**

First of all, its conveniant to not define a variable until it is 
needed. This results in fewer lines of code in the initialization stages 
of the program and smaller, more relevent initial fields in a table. 
Take the following examples.

### Example 1:

*A game needs to redraw the background whenever there are changes to the 
level, and I've decided to facilitate this by using a boolean flag 
variable `CLEAN` to indicate when the level layout changes. The game 
also needs to draw the background for the first time when the game 
begins, and it's most efficient to do this with the same code. So, the 
code for drawing the background executes only if the global `CLEAN` 
variable evaluates to **false** with* `if not CLEAN then`. *Since the 
uninitialized `CLEAN` variable defaults to a value of **nil** which 
evaluates to **false**, the program draws the screen for the first time 
and then assigns a value of **true** to `CLEAN` so it will not draw 
again until another part of the program assigns the value of **false** 
to `CLEAN` again.*

### Example 2:

*I have 10 different objects in a game and some of them can jump, which 
is a state indicated by the boolean* `jumping` *field in their table. 
Because the jump state is irrelevant to objects that cannot jump, I do 
not initialize the* `jumping` *field in the initialization of their 
table. In the physics code of the game, certain operations occur only 
after an* `if jumping then` *statement. Since objects with an 
uninitialized* `jumping` *field return a value of **nil** which 
evaluates to **false**, these objects are treated similarly to objects 
that do have a* `jumping` *field that is currently set to **false**.*

## the camparator pitfall

A problem can arise when using this technique because using certain 
comparators on a **nil** value results in an execution error. Let's say 
some of the objects in a game can have an acceleration speed and others 
don't accelerate. Normally, the game loop would have a section that adds 
the value of an object's `accel_x` field to that object's `delta_x` 
field with the `+` operator, translating its frame-by-frame acceleration 
into momentum. However, if the object never has acceleration and thus is 
not initialized with an `accel_x` field, Lua raises an error.

> Error: main.lua:4: attempt to perform arithmetic on field 'accel_x' (a 
> nil value)

There are two solutions to this problem. I could decide that 
acceleration is such an intrinsic property of objects that even an 
object that never accelerates should have an `accel_x` field initialized 
to a permanent value of 0. On the other hand, I could still take 
advantage of the default **nil** value by surrounding the code that 
accelerates objects with an **if** statement like `if object.accel_x 
then`. In that latter case, the `+` operator that raises an error if 
used on a **nil** value would never execute because the **if** statement 
evaluates **nil** as **false**.

A similar problem occurs if objects have a table such as a level that 
has a table of treasure available to collect. In the case of a level 
that does not contain any treasure, an uninitialized `treasure` table 
will have a default value of **nil** and will Lua will raise an error if 
the program tries to access an element in `treasure` if it is **nil**. 
In this case every level has to have a table of treasure initialized to 
be empty i.e. `treasure = {}`, or every attempt to access the table of 
treasure must be preceded by a `if treasure then` statement so the code 
doesn't attempt to access **nil** as if it is a table.

## assigning a value of **nil**

As a general rule, I don't manually assign a value of **nil** to any 
variables in my code because Lua uses the value of **nil** under certain 
circumstances and assigning a value of **false** is a more stylistically 
consistent option that yields similar results. Assigning a value of 
**nil** to an element in a table can result in the table terminating at 
that element, and assigning a value of **nil** to a global variable 
deletes it. Assigning a value of **false** in those circumstances 
doesn't have those extra effects.

For example, if I'm using a global variable `HELD_ITEM` to store a value 
representing what object is currently in the player's hands, it may be 
tempting to assign a value of **nil** to `HELD_ITEM` if the player drops 
the item or gives it to someone else, but I'll assign a value of 
**false** instead for code consistency and because I'm not intending to 
delete the `HELD_ITEM` variable but rather empty it. Any later code 
depending on boolean evaluations like `if HELD_ITEM then` will evaluate 
to **false** which is the desired result for the player's hands being 
empty.
