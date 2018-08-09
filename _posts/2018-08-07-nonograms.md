---
layout: post
title: Solving colored Japanese crosswords with the speed of light
language: en
---

*This is a translation of my post from the finest technical news and programming site for Russian-speaking people - habr.com. The post got rating +88 and has been the post of a day (the post which got the biggest rating that day).*

Japanese crosswords (aka nonograms) are picture logic puzzles. The solution of puzzles based on numbers which are placed to the left of the rows and on the top of the columns.

The size of the puzzle may be as big as 150x150. A player calculate the color of each cell with some special logical methods. The solution process can take a couple of minutes on puzzles for newcomers and dozens of hours on hard ones.

A good algorithm is capable to solve the task too much faster. In the post I described how to write a solution which works almost instantly, using most convenient algorithms (which lead to a solution in general), and their optimizations and features of C++ (which decrease the running time in dozens of time).

<img align="center" src="https://habrastorage.org/webt/w1/tw/-c/w1tw-caq1d9vswcrqfapdufcbfk.gif" />
<!--break-->

## Game rules
At the start the canvas of the image is filled with white. For vanilla black-white crosswords it is needed to restore the positions of black cells.

