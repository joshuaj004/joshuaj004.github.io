---
layout: post
title:  "One More TSLint fix"
categories: vue frontend tslint
date: 2020-09-19 07:38:00
---

There actually was one more tslint issue that I didn't want to deal with yesterday, but I woke up relatively full of energy so I fixed it.

The issue was `Expected a 'for-of' loop instead of a 'for' loop with this simple iteration` from this snippet:

```javascript
49:5 Expected a 'for-of' loop instead of a 'for' loop with this simple iteration
    47 | 
    48 |   public processSudokuBoard(sudokuBoard) {
  > 49 |     for (let i = 0; i < sudokuBoard.board.length; i++) {
       |     ^
    50 |       for (let j = 0; j < sudokuBoard.board[i].length; j++) {
    51 |         if (sudokuBoard.board[i][j] === '0') {
    52 |           sudokuBoard.board[i][j] = '';
```

My understanding is that the linter was seeing that nothing was really being done with the outside loop, which isn't entirely true. The `i` was being used for the index and rewriting with a `for-of` loop would actually be messier if I wanted to have access to an index. All I'm doing is replacing any 0's in the nested arrays with an empty space, so I googled how to do that. The recommendation was to use `map` and that was the correct recommendation:

```javascript
    const cleanedBoard = sudokuBoard.board.map(
      (row) => row.map(
        (cell) =>
          cell === 0
          ? cell = ''
          : cell,
      ),
    );
    this.sudokuBoard.board = cleanedBoard;
```

It's a little bit cleaner and tslint doesn't complain about it.

I also noticed a lot of `307 Temporary Redirect`s in my logs because I forgot to put a trailing `/` on my url path _duh_.

`"GET /api/v1/sudoku/sudoku?difficulty=0.5 HTTP/1.1" 307 Temporary Redirect`

vs

`"GET /api/v1/sudoku/sudoku/?difficulty=0.5 HTTP/1.1" 200 OK`

Voila, just cut the size of my logs by 2.