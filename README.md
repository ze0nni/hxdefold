![image](https://cloud.githubusercontent.com/assets/576184/15849567/80f4ada8-2c93-11e6-8430-5b5dbe5e58a3.png)

# Haxe support library for the [Defold](https://www.defold.com/) game engine

[![Build Status](https://travis-ci.org/hxdefold/hxdefold.svg?branch=master)](https://travis-ci.org/hxdefold/hxdefold) ![Defold API version: 1.2.148](https://img.shields.io/badge/api%20version-1.2.148-orange.svg)

This library allows writing beautiful [Haxe](https://haxe.org/) code for KING's [Defold](https://www.defold.com/) game engine \o/

## Features
 - Fully typed Defold API with proper compile-time errors and IDE services.
 - Type-safe game object messages and properties with zero overhead.
 - Strengths of Haxe without compromises: powerful type system, meta-programming, static optimizations and cross-target code sharing.

## Quick start

(assuming you already [installed Haxe](https://haxe.org/download/)😊)

 - Install this library (from this repo): `haxelib git hxdefold https://github.com/hxdefold/hxdefold`
 - Run `haxelib run hxdefold init` inside your Defold project. It will create a sample `Hello.hx` script component class and a `build.hxml` for building it.
 - Read the comments in these files to quickly get some idea.
 - Build with `haxe build.hxml` to get the lua output.
 - Add `Hello.script` to your game object in the editor and observe the greeting in the debug console.
 - Proceed with writing well-structured, expressive and type safe code for your Defold game.

## How does it look like

```haxe
// sample script component code

// definition of the component data, passed as `self` to the callback methods
typedef HelloData = {
	// fields with @property annotation will show up in the editor
	@property(9000) var power:Int;
}

// component class that defines the callback methods
// after compiling Haxe, the `Hello.script` will appear in the Defold project that can be attached to game objects
class Hello extends defold.support.Script<HelloData> {
	// the `init` callback method
	override function init(self:HelloData) {
		trace('Haxe is over ${self.power}!'); // will be printed to the debug console
	}
}
```

## Documentation

Here is the [API reference](http://hxdefold.github.io/hxdefold/).

And here are some example Defold projects, ported from Lua:
 * https://github.com/hxdefold/hxdefold-example-sidescroller
 * https://github.com/hxdefold/hxdefold-example-platformer
 * https://github.com/hxdefold/hxdefold-example-frogrunner
 * https://github.com/hxdefold/hxdefold-example-magiclink
 * https://github.com/hxdefold/hxdefold-example-throwacrow
 * https://github.com/hxdefold/hxdefold-example-warbattles

## How does it work?

Since version 3.4, Haxe supports compiling to Lua, making it possible to use Haxe with Lua-based engines, such as Defold.

However, this requires a bit of autogenerated glue code, because Defold expects scripts to be in separate files, while Haxe compiles everything in a single lua module. So what we do, is generate a simple glue `.script` file for each class extending the magic `defold.support.Script` base class (there are also `GuiScript` and `RenderScript`).

For example, for the `Hello` script from this README, this glue code is generated in the `Hello.script` file:

```lua
-- Generated by Haxe, DO NOT EDIT (original source: src/Sample.hx:11: lines 11-16)

local m = require "main"

go.property("power", 9000)

local script = m.Hello.new()

function init(self)
	script:init(self)
end
```

You can then add this script to the game objects in the Defold Editor.

## Logo

Made by the awesome [**@markknol**](https://github.com/markknol). Check out [his website](https://blog.stroep.nl/) for more art&code!
