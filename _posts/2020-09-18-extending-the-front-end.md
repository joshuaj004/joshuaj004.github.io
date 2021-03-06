---
layout: post
title:  "Extending the Frontend"
categories: vue frontend
date: 2020-09-18 15:16:00
---

At this point, this project has a working backend that can deliver many sudoku puzzles a second 🎉🥳🎉! I can get in the business of Sudoku as a Service and rake in the money. Unfortunately, if I tell my non-technical friends I created this, they'll never use it. Luckily I know a little bit of frontend, but no Vue (the default frontend framework that came with this cookiecutter project). I could either replace the whole front end with some basic html+css+js or *grumble grumble* learn something new.

I have a little bit of experience with AngularJs, but that was years ago. The University of Helsinki offers a course that teaches React, Redux, Node, MongoDB, GraphQL, and TypeScript see [here](https://github.com/joshuaj004/Software-Engineering-Resources), but I actually didn't end up completing this course. I don't really know what my point was, I guess just that I'll be picking Vue up basically from scratch. They have a [Guide](https://vuejs.org/v2/guide/) which I'll at least skim.

Once again I'll be using some of the autogenerated code as reference material. Let's see if we can figure out what it's doing so that I can copy it.

```typescript
<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import { api } from '@/api';
import { appName } from '@/env';
import { readLoginError } from '@/store/main/getters';
import { dispatchLogIn } from '@/store/main/actions';

@Component
export default class Login extends Vue {
  public email: string = '';
  public password: string = '';
  public appName = appName;

  public get loginError() {
    return readLoginError(this.$store);
  }

  public submit() {
    dispatchLogIn(this.$store, {username: this.email, password: this.password});
  }
}
</script>
```

Component, Vue are used for some Vue stuff which I think we can use as is for now.

We're import `api.ts` which presumably handles some API calls, but I don't actually see it used in the snippet.

appName is used in the template to display the project name.

These next 2 imports look like where the magic happens with the getters and actions. Getters looks related to the state, so I think we need to modify the `actions.ts` file.

I'm going to add this little snippet here right next to the other functions to allow a call to be made to the api.

```typescript
async getSudokuBoard(context: MainContext, payload: { seed: number, difficulty: number}) {
        const loadingNotification = { content: 'Loading Sudoku Board', showProgress: true };
        try {
            commitAddNotification(context, loadingNotification);
            const response = (await Promise.all([
                api.getSudokuBoard(payload.seed, payload.difficulty),
                await new Promise((resolve, reject) => setTimeout(() => resolve(), 500)),
            ]))[0];
            commitRemoveNotification(context, loadingNotification);
            commitAddNotification(context, { content: 'Sudoku Board Successfully Loaded', color: 'success' });
            return response.data;
        } catch (error) {
            commitRemoveNotification(context, loadingNotification);
            commitAddNotification(context, { color: 'error', content: 'Error Loading Sudoku Board' });
        }
    },

[some other lines that were already there skipped]
export const dispatchGetSudokuBoard = dispatch(actions.getSudokuBoard);
```

I also needed to modify the `api.ts` file and added this:

```typescript
async getSudokuBoard(seed?: number, difficulty?: number) {
    var sudokuURL = `${apiUrl}/api/v1/sudoku/sudoku?`;
    if (seed) {
      sudokuURL += "puzzle_id=" + seed + "&";
    }
    if (difficulty) {
      sudokuURL += "difficulty=" + difficulty;
    }
    return axios.get(sudokuURL);
  },
```

As far as I know, this is what actually makes the call.

I also make changes to `router.ts` to render the `sudoku.vue` file when I navigate to /sudoku.

```typescript
{
    path: '/sudoku',
    component: () => import('./views/Sudoku.vue'),
},
```

And here is the `sudoku.vue` file (the rendering is a little wonky due to the vue code): 

```javascript
<template>
  <v-content>
    <v-container fluid fill-height>
      <v-layout align-center justify-center>
        <v-flex xs12 sm8 md4>
          <div class="sudoku-container">
            <div class="grid-sudoku">
              <div v-for="(row, rowIndex) in sudokuBoard.board" :key="rowIndex" class="grid-row">
                <div v-for="(cell, colIndex) in row" :key="colIndex" class="grid-cell">
                  <!-- <transition-group tag="div" name="list-animation"> -->
                    <input type="text" v-bind:key="cell" v-model="row[colIndex]" class="grid-cell-editor" />
                </div>
              </div>
            </div>
            <br />
            <div class="board-metadata">
              Difficulty: <input type="range" min="0.01" max="0.99" v-model="sudokuBoard.difficulty" class="slider" step="0.01">
              {{ sudokuBoard.difficulty }}
              <br />
              Seed: {{ sudokuBoard.seed }}
            </div>
            <br />
            <v-btn @click.prevent="submit">Get New Sudoku Board</v-btn>
          </div>
        </v-flex>
      </v-layout>
    </v-container>
  </v-content>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import { api } from '@/api';
import { appName } from '@/env';
import { dispatchGetSudokuBoard } from '@/store/main/actions';

@Component
export default class Sudoku extends Vue {
  public appName = appName;
  public sudokuBoard = {seed: null, difficulty: 0.5};

  beforeMount(){
    dispatchGetSudokuBoard(this.$store, {seed: null, difficulty: 0.5}).then(sudokuBoard => {
      this.processSudokuBoard(sudokuBoard);
    });
  }

  public submit() {
    dispatchGetSudokuBoard(this.$store, {seed: null, difficulty: this.sudokuBoard.difficulty}).then(sudokuBoard => {
      this.processSudokuBoard(sudokuBoard);
    });
  }

  public processSudokuBoard(sudokuBoard) {
    console.log(Sudoku)
    for (var i = 0; i < sudokuBoard.board.length; i++) {
      for (var j = 0; j < sudokuBoard.board[i].length; j++) {
        if (sudokuBoard.board[i][j] == '0') {
          sudokuBoard.board[i][j] = "";
        }
      }
    }
    this.sudokuBoard = sudokuBoard;
  }
}
</script>

<style scoped>
@import './../assets/sudokustyle.css';
</style>
```

All ths does is on page load (beforeMount function), grab a sudoku board and replace all the 0s with empty strings. You can also adjust the difficulty and get a new board.

I also used some minor styling that I found from [this post](https://medium.com/better-programming/how-to-build-sudoku-in-vue-js-f97509b523ed). It isn't cleaned up or anything so you could probably remove a lot of these rules.

```css
* {
    margin: 0;
    padding: 0;
}

input:focus,
select:focus,
textarea:focus,
button:focus {
    outline: none;
}

body {
    display: grid;
    grid-template-rows: 150px 1fr;
    background-color: #ff5252;
    font-family: 'Dosis', sans-serif;
}

header {
    display: grid;
    color: white;
}

header h1 {
    place-self: center;
}

#app-sudoku {
    place-self: center;
    display: grid;
    grid-template-rows: auto 1fr;
    justify-items: center;
}

.buttons-container {
    display: grid;
    grid-template-rows: auto auto;
}

.button {
    display: inline-block;
    border-radius: 6px;
    background-color: whitesmoke;
    border: none;
    color: black;
    text-align: center;
    font-size: 16px;
    padding: 10px;
    width: 230px;
    transition: all 0.5s;
    cursor: pointer;
    margin: 0px 0px 25px 0px;
    font-family: 'Dosis', sans-serif;
    font-weight: bold;
}

.button span {
    cursor: pointer;
    display: inline-block;
    position: relative;
    transition: 0.5s;
}

.button span:after {
    content: '\00bb';
    position: absolute;
    opacity: 0;
    top: 0;
    right: -20px;
    transition: 0.5s;
}

.button:hover span {
    padding-right: 25px;
}

.button:hover span:after {
    opacity: 1;
    right: 0;
}

.grid-sudoku {
    display: table;
    background: white;
    border: 3px solid black;
}

.grid-sudoku > div:nth-child(3), .grid-sudoku > div:nth-child(6) {
    border-bottom: 3px solid black;
}

.grid-row > div:nth-child(3), .grid-row > div:nth-child(6) {
    border-right: 3px solid black;
}

.grid-cell {
    display: table-cell;
    padding: 10px;
    border: 1px solid gray;
}

.grid-cell-editor {
    border: none;
    width: 20px;
    height: 20px;
    font-family: 'Dosis', sans-serif;
    font-weight: bold;
    text-align: center;
    font-size: 18px;
    transition: all ease 1.0s;
}

.answer {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    display: flex;
    justify-content: center;
    align-items: center;
}

.answer-image {
    border: 2px solid black;
    width: 300px;
    height: 300px;
}

.list-animation-enter, .list-animation-leave-to {
    opacity: 0;
    transform: translateY(20px) translateX(20px) rotate(200deg) scale(0.5);
}

.list-animation-leave-active {
    position: absolute;
}

.fade-enter-active, .fade-leave-active {
    transition: opacity .5s;
}

.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
    opacity: 0;
}
```

All told, this gives a very basic page that fetches and displays sudoku boards.

![](/../assets/2020-09-18-15-16-21.png)