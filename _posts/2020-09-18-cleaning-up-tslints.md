---
layout: post
title:  "Cleaning Up TSLint Issues"
categories: vue frontend tslint
date: 2020-09-18 17:36:00
---

In my previous post, I realized that I didn't look at the linter once and there were plenty of linting issues. Here is a summary of things I fixed:

* var -> let + const
* " -> '
* Marked my class methods as either 'private', 'public', or 'protected' 
* Reordered class methods so that private methods come after public methods
* Added/Removed/Changed whitespace
* Parentheses are required around the parameters of an arrow function definition
* Removed extra console.log statements
* Added missing semicolons
* == -> ===
* Changed some nulls -> undefined 
* Updated some functions to accept both numbers and undefined for certain parameters

Some git screenshots:

![](/../assets/2020-09-18-17-42-19.png)

![](/../assets/2020-09-18-17-42-38.png)

![](/../assets/2020-09-18-17-43-00.png)