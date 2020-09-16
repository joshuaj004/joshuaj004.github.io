---
layout: post
title:  "First Steps with the FastAPI Cookiecutter Template"
categories: FastAPI Cookiecutter
date: 2020-09-16 07:23:00
---

It's been a busy day, but I've been able to digest so more of this project in between running around and this should serve as a mini guide to adding your first route to the FastAPI cookiecutter template. I don't really have any idea what I want my project to be, so let's use sudoku as a filler.

I could write my own sudoku library, but I've done it _so_ many times that at this point I'm lazy. I found [py-sudoku](https://pypi.org/project/py-sudoku/) and it looks like it does everything I'd need so let's install it. By looking at the readme, this project uses [Poetry](https://python-poetry.org/) which is new to me, but easy enough to use:

```console
$ poetry add py-sudoku
```


Then you might have to throw an install command for good measure:

```console
$ poetry install
```

Okay so at this point you can play sudoku locally, but we're not here for that. Let's add an endpoint. I peeked at one of the other files to see what I'd need to do, and this seems to be the most basic endpoint:

```python
from fastapi import APIRouter

router = APIRouter()

@router.get("/sudoku")
def generate_puzzle():
    return "generated sudoku puzzle"
```

Let's check the docs and
![](/../assets/2020-09-15-18-33-12.png)

It's not there...

That's because we need to tell the api router about this sudoku router and it's a 1 line addition to the `api.py` file.

```python
api_router.include_router(sudoku.router, prefix="/sudoku", tags=["sudoku"])
```

Let's refresh and voila!
![](/../assets/2020-09-15-18-35-29.png)

If we try it out, we get exactly what we'd expect:
![](/../assets/2020-09-15-18-36-09.png)

Of course it would be a lot more impressive if it actually returned something resembling a sudoku puzzle so let's make that happen.

```python
from fastapi import APIRouter
from random import randint
from sudoku import Sudoku

router = APIRouter()

@router.get("/sudoku")
def generate_puzzle():
    puzzle = Sudoku(3, seed=randint(0, 1000)).difficulty(0.5)
    returnPuzzle = puzzle.board
    for rowIdx, row in enumerate(returnPuzzle):
        for colIdx, col in enumerate(row):
            if returnPuzzle[rowIdx][colIdx] == None:
                returnPuzzle[rowIdx][colIdx] = 0

    return returnPuzzle
```

We import random so that we can adequately seed our puzzle. If we don't specify the seed, it returns the same puzzle over and over again. We also go ahead and replace all the `None`s with 0's because I think it looks nicer (but this really doesn't _need_ to occur). Anyways, it returns what looks like a sudoku puzzle, all ready for a frontend to make use of it!

![](/../assets/2020-09-16-07-09-01.png)

I could also create a schema to validate, but I feel like I've reached a natural stopping point, so that'll have to be another post for another day!