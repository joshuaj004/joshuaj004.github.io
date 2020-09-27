---
layout: post
title:  "Making Given Sudoku Cells Read Only"
categories: sudoku vue
date: 2020-09-27 10:31:00
---

I finally created a Trello board with a list of improvements that would make the UX nicer. Furthering my learning with Vue and wanting to improve my sudoku website, I decided I would make the given sudoku cells bold and readonly vs the user inputed cells not bold and not readonly. It turns out this wasn't that difficult, but there were a couple things to note. You couldn't check if [[the cell you were trying to make readonly] was in the board] because anytime the user inputs a number, that cell ends up in the board, making every cell write once. That is a terrible way to explain it and I wish I could make it more clear. Maybe explaining the solution will make it clearer. The solution is to make a list of all the given cells when a Sudoku board is retrieved and check if each cell is in that list. If they are, make them bold and readonly, otherwise make them normal and editable. 

The end result looks like this:

![](/../assets/2020-09-27-10-38-35.png)