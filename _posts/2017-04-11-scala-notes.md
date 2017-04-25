---
layout: post
title:  "Scala Notes"
date:   2017-04-11
categories: scala
---

## Other notes

- http://docs.scala-lang.org/cheatsheets/
- `t._2              // Part of a method name, such as tuple getters` http://stackoverflow.com/a/8001132

## Scala for the Impatient

#### Chapter 1 - Basics

- `val` immutable, constants. should use `val` in most cases
- `var` mutable
- types of values and variables are inferred from the expression that they are initialized with, but you **can** specify type
  - e.g. `val greeting: String = null` `or val greeting: String = "Hi"`
- everything is an object including numbers (e.g. `3.toString()`
- Scala compile converts primitives (`1`, `"hi"`) to wrappers
- Scala has its own classes like `RichInt`, `RichChar`, `StringOps`, etc that wrap Int, Char, String, etc to provide greater functionality
- `{}.isInstanceOf[Unit]` is `True`
- operators `+ - * / %` are methods
  - e.g. `a + b` is equivalent to `a.+(b)`
- method calls can be written in two ways: `a method b` or `a.method(b)`
- a tuple is similar to an Array however it can contain items of differing types
- tuple indexes start at 1, not 0 as in an Array
- `toMap` takes a collection and turns it into a Map
- "static" function calls don't look like Java's static function calls that are called from classes
  - e.g. (scala) `pow(2,4)`
- there are no static function, instead **singleton obkects**
- methods without params are called without parentheses

#### Chapter 2

#### Chapter 5 - classes

- provides getter and setter methods for every field by default (so you don't have to go crazy like one may do in Java, creating getters and setters for everything)
  - getters are named after the field, and setters are named after the field with a `_` appended to the end. e.g. `age` and `age_`
  - Scala will create a class for the JVM from the one below with a private age field, and **public** getter and setter methods (you can make them private if you declare `age` private)
      ```
      class Person {
        var age = 0
      }
      ```
- you can also create a field using `val` but it'll only have a getter (because it's immutable!!), not setter
