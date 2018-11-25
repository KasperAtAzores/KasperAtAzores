---
layout: post
title:  "Blender, python and command pattern"
subtitle: "On the darker side of undo"
date:   2018-11-25
background: '/img/posts/procedural_citybuilding.png'
---


I have now been programming simple and less simple things in blender for a while. The architecture of the python API is to a large degree a mirror of the interface, even to such a degree that you can obtain the python command behind most interface actions.

It seems like the python api is primarily geared towards extending the blender api, and to be able to allow you to build algoritmic mesh - everything from spiral staircases, gemstones, or an isometric torus.

In order to make it easy to extend the user interface many of the more complicated mesh transformations, uv mappings, and what not, many such operations are build as commands that operate on currently selected elements.

In order to use such operations, you first have to programmatically select the object, select the operation, set up its parameters, and finally apply it. You feel like you are doing remote control of the blender interface. It even is necessary to select whihc tool-set/tab you are going to use. When you then finally apply the command it works really well even enabling undo and not the least firing off all dependecies using the internal observer patterns. 

I have been teaching command patterns and observer patterns, undo using command patterns, and even undo over observer lists as in blender. It is fantastic. Except it is ohh so slow!

Very often when you write algoritmic mesh and other such aspects, what happens is that you make alot of objects. And the performance of the command/undo and observer pattern become <i>n<sup> 2</sup></i>. When there is 10 elements, and you add the 11th, the remaining 10 need to be visited, shades computed, etc. When you add the 1000th element, there is 999 other that need to be examined.

Sometime - but only sometimes - are you able to do all the manipulations you want, without having the UI update continously.

### And what did I learn from this
1. It is great to be able to extend a complex system like blender with new functionality, to be able programmatically access (nearly) all that is acessible from the GUI. More systems should have such a powerful scripting language build in at its core.
2. When designing such a system, it is important to recognize that you should not be forced to program in "remote control" style alone, but have access to some underlying datastructures and only in the end explicitly tell the UI: "hey, stuff happened".

##### About the image
I stole the image from: https://josauder.github.io/procedural_city_generation. In part because it is cool, and partly because they too have run into performance issues with large blender projects.
