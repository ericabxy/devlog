---
layout: post
title:  "Inheritance and Initialization in Lua"
date:   2024-12-25 10:23:00 -0700
categories: programming, lua, object-oriented-programming
---

Programming in Lua offers a reasonably simple approach to object-oriented
programming concepts. Class derivation can be achieved with "metatables" using
the method described in [Chapter 16.1][1] of _Programming in Lua_. Inheritance
is covered in [Chapter 16.2][2].

The LuaRocks project publishes a [style guide][3] outlining a standard way to
write class modules, based on a [discussion by Hisham][4].

<pre>
--- @module myproject.myclass
local myclass = {}

-- class table
local MyClass = {
  number = 0
}

function MyClass:increment()
   self.number = self.number + 1
   -- code
end

function myclass.new()
   local self = {}
   setmetatable(self, { __index = MyClass })
   return self
end

return myclass
</pre>

The *new* function creates an empty table and sets *MyClass* in that table's
"metatable". The table returned by *new* is always a new table that can be
treated as an object, while *MyClass* is an immutable prototype who's fields
are referenced until the object's fields are set/modified.

## Initialization

_Programming in Lua_ describes a way to initialize an object by supplying *new*
with a pre-constructed table, e.g. <code>object = myclass.new({number = 10})</code>
. Let's instead make a constructor function similar to *new* that initializes a
table before setting its metatable.

<pre>
function myclass.init(value)
  local self = {number = value}
  setmetatable(self, { __index = MyClass })
  return self
end
</pre>

## Inheritance

The technique for subclassing described by _Programming in Lua_ also involves
supplying *new* with a table, albeit a class table from which further objects
are derived. To extend *MyClass*, a submodule would first import myclass with
<code>local myclass = require('myclass')</code>, then declare its class table
and supply it to *myclass.new*. Let's instead write an inheritance function
that works similarly to *new*.

<pre>
function myclass.extend(self)
  setmetatable(self, { __index = MyClass })
  return self
end
</pre>

And lets write a submodule that extends *myclass*. The structure of the module
is the same as the *myclass*, except that it needs the <code>require</code>
statement and it declares its class table in a slightly different way.

<pre>
local MySubclass = myclass.extend{
  othernumber = 100
}
</pre>

Now when a new object derived from *mysubclass.new* references its *number*
field, it looks in MySubclass for the default value. Since it does not exist
there, but *myclass.new* set the metatable for *MySubclass* to point to
*MyClass*, the *number* field will be looked up and found there.

## Superclass Methods

Using a chain of metatables as above, an object derived from subclasses is
essentially an object with all of the classes' fields and methods. You must
be careful to name each subclass method with a unique identifier unless you
intend to override a superclass method completely. The module methods *extend*,
*init*, and *new*, however are unique and can be used to access a chain of
initialization methods. For example, the *mysubclass* module can define an
*init* function.

<pre>
function mysubclass.init(value, ...)
  local self = myclass.init(...)
  self.othernumber = value
  setmetatable(self, { __index = MyClass })
  return self
end
</pre>

The '...' operator contains all the arguments passed to *init* (after *number*)
in a sequence (not a table), so passing it to *myclass.init* allows *myclass*
to initialize the table without *mysubclass* having to be changed. For
instance, if *myclass* defines
<code>function myclass.init(value1, value2, value3)</code>,
*mysubclass.init* can simply pass along every parameter passed to it except
that which it knows how to deal with.

## Putting It All Together

Above I defined three different module functions--*extend*, *init*, and the
unmodified *new*-- to illustrate the differing needs of prototyping,
inheritance, and initialization. Now that we know what is needed, the number of
administrative functions can be collapsed into two. For example, the same can
be accomplished with just *extend* and *new*, with *new* taking over for
*init*.

<pre>
function myclass.extend(self)
  setmetatable(self, { __index = MyClass })
  return self
end

function myclass.new(value)
   local self = {number = value}
   setmetatable(self, { __index = MyClass })
   return self
end
</pre>

[1]: https://www.lua.org/pil/16.1.html
[2]: https://www.lua.org/pil/16.2.html
[3]: https://github.com/luarocks/lua-style-guide
[4]: https://hisham.hm/2014/01/02/how-to-write-lua-modules-in-a-post-module-world/
