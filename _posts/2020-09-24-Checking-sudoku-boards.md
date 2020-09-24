---
layout: post
title:  "Checking Sudoku Boards"
categories: sudoku
date: 2020-09-24 12:55:00
---

The Age of Devops is over! Long live the Age of Development! Okay devops is never really over, but it's in a good position for now and I wanted to actually build something out. Before, you could fill out a board and not receive feedback on it so I decided this was a good thing to add. 

I added this button.

![](/../assets/2020-09-24-12-45-22.png)

It tells you how many are wrong.

![](/../assets/2020-09-24-12-45-45.png)

Accounting for a singular number being wrong.

![](/../assets/2020-09-24-12-46-15.png)

And for when it's all correct.

![](/../assets/2020-09-24-12-46-34.png)

Very cool. What did it take?

## Backend Additions

`sudoku.py` additions
```python
@router.post("/check", response_model=schemas.SudokuBoardCheck)
def check_board(sudoku_board: schemas.SudokuBoard):
    puzzle = Sudoku(3, seed=sudoku_board.seed).solve()

    diff_count = 0
    for rowIdx, row in enumerate(puzzle.board):
        for colIdx, col in enumerate(row):
            if puzzle.board[rowIdx][colIdx] != sudoku_board.board[rowIdx][colIdx]:
                diff_count += 1

    returnDict = {
        'correct': diff_count == 0,
        'difference': diff_count
    }
    return returnDict
```

`sudoku_board_check` model
```python
from pydantic import BaseModel, Field


class SudokuBoardCheck(BaseModel):
    correct: bool = Field(..., example=False)
    difference: int = Field(..., example=40)
```

`__init__.py` additions
```python
from .sudoku_board_check import SudokuBoardCheck
```

## Frontend Additions
`api.ts` additions
```javascript
async checkSudokuBoard(board: number[], seed: number, difficulty: number) {
    return axios.post(`${apiUrl}/api/v1/sudoku/check`, {
      board,
      seed,
      difficulty,
    });
  },
```

`actions.ts` additions
```javascript
async checkSudokuBoard(context: MainContext, payload:
            { board: number[], seed: number, difficulty: number}) {
        const loadingNotification = { content: 'Checking Sudoku Board', showProgress: true };
        try {
            commitAddNotification(context, loadingNotification);
            const response = (await Promise.all([
                api.checkSudokuBoard(payload.board, payload.seed, payload.difficulty),
                await new Promise((resolve, reject) => setTimeout(() => resolve(), 500)),
            ]))[0];
            commitRemoveNotification(context, loadingNotification);
            commitAddNotification(context, { content: 'Sudoku Board Successfully Verified', color: 'success' });
            return response.data;
        } catch (error) {
            commitRemoveNotification(context, loadingNotification);
            commitAddNotification(context, { color: 'error', content: 'Error Verifying Sudoku Board' });
        }
    },

[some skipped lines]
export const dispatchCheckSudokuBoard = dispatch(actions.checkSudokuBoard);
```

`Sudoku.vue` Additions
* Added a results div that is hidden before checking.
* Changed the vue model for the sudoku cells to be numbers instead of strings
* Added a listener to the verify button. 