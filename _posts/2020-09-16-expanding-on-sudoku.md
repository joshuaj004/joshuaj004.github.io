---
layout: post
title:  "Expanding on Sudoku"
categories: FastAPI Cookiecutter
date: 2020-09-16 11:13:00
---

Continuing from my [previous post]({% post_url 2020-09-16-first-steps-with-the-fastapi-template %}), let's see about expanding this endpoint. 

Below is the full file, but I'll explain each part.

Py-sudoku lets you pass in a seed and difficulty so likewise we should make that configurable. We'll accept two optional parameters, puzzle_id (our seed) and difficulty (a sliding value that really just decreases the number of populated cells). Puzzle_id is given a default value of `None` and difficulty is given a default value of `0.5` which corresponds to about medium. Additionally we're giving the difficulty a range between, but not including, 0.0 and 1.0. FastAPI validates this parameter to make sure we aren't given values outside of that range.

![](/../assets/2020-09-16-11-23-03.png)

Looking at the other files, they have schemas, so let's give the Sudoku Boards a schema too. It's quite basic at the moment, consisting of 3 fields, but that should be all that's need for now. 

However, do not forget to import this schema in the `schemas/__init__.py` file

`from .sudoku_board import SudokuBoard`

Sudoku Board Schema file

`schemas/sudoku_board.py`
```python
from pydantic import BaseModel, Field


class SudokuBoard(BaseModel):
    seed: int = Field(..., example=1)
    difficulty: float = Field(..., example=0.5)
    board: list = Field(..., example=[
        [0,0,7,0,4,0,0,0,0],
        [0,0,0,0,0,8,0,0,6],
        [0,4,1,0,0,0,9,0,0],
        [0,0,0,0,0,0,1,7,0],
        [0,0,0,0,0,6,0,0,0],
        [0,0,8,7,0,0,2,0,0],
        [3,0,0,0,0,0,0,0,0],
        [0,0,0,1,2,0,0,0,0],
        [8,6,0,0,7,0,0,0,5]
    ]
)

```

We use this to let FastAPI know what exactly we're returning. That's why we assemble the returnDict because otherwise it throws an error.

```python
from fastapi import APIRouter, Query
from random import randint
from sudoku import Sudoku
from typing import Optional

from app import schemas

router = APIRouter()

@router.get("/sudoku/", response_model=schemas.SudokuBoard)
def generate_puzzle(
    puzzle_id: Optional[int] = Query(
        None,
        title="Puzzle ID",
        description="Selects a specific puzzle."
    ),
    difficulty: Optional[float] = Query(
        0.5,
        title="Puzzle Difficulty",
        description="Sets the difficulty for the generated Sudoku puzzle. Accepts floats between 0.0 (easier) and 1.0 (harder).",
        gt=0.0, 
        lt=1.0
    )
):
    puzzle_seed = puzzle_id if puzzle_id != None else randint(0, 1000)
    puzzle = Sudoku(3, seed=puzzle_seed).difficulty(difficulty)
    returnPuzzle = puzzle.board
    for rowIdx, row in enumerate(returnPuzzle):
        for colIdx, col in enumerate(row):
            if returnPuzzle[rowIdx][colIdx] == None:
                returnPuzzle[rowIdx][colIdx] = 0

    returnDict = {
        'board': returnPuzzle,
        'seed': puzzle_seed,
        'difficulty': difficulty
    }
    return returnDict
```

To recap what was added:
- A seed can be specified
- Difficulty can be specified
- The difficulty range is checked automatically 
- The parameters are documented
- A SudokuBoard schema is created, registered, and used
- A SudokuBoard object is returned with the board, the seed, and the difficulty rather than just a raw board

This thing is really starting to shape up! I'm gonna go take a walk or something now...
