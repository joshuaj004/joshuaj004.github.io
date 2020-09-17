---
layout: post
title:  "Performance Testing with K6"
categories: performance-testing k6
date: 2020-09-17 08:08:00
---

You would think that a framework called FastAPI would be fast, but I want to see it to believe it. At least from when I hit the API with curl or [Insomnia](https://insomnia.rest/) (A _REST_ client haha get it?), it feels pretty fast. However I want to see how fast it is when it's not just a request every so often. 

Enter [K6](https://k6.io/), a nifty little performance testing tool that I've used in the past. The reason I picked it up in the first place was that it had a tool to convert Postman requests to K6 requests so that I wouldn't have to rewrite them and I've had a good time with it ever since. It basically lets you write tests in NodeJs, but then runs them in Go for performance reasons. It even allows you to write setup and teardown code, which is useful if you need to grab authentication to hit a url. Anyways, let me show you how I'll be testing this basic sudoku endpoint. 

After installing K6, I created a `script.js` file that I dropped in the backend folder. It really is dead simple:

```javascript
import http from 'k6/http';

const url = 'http://localhost/api/v1/sudoku/sudoku/';

export default function() {
  http.get(url);
}
```

Now let's say I wanted to run this with 1 'virtual user' for 30 seconds, the command is:

```
k6 run --vus 1 --duration 30s script.js
```

And the results are:

![](/../assets/2020-09-17-08-25-12.png)

What I want to highlight are the following lines:
- http_req_duration
    - Total time for the request. It's equal to http_req_sending + http_req_waiting + http_req_receiving (i.e. how long did the remote server take to process the request and respond, without the initial DNS lookup/connection times).  
- iteration_duration
    - The time it took to complete one full iteration of the default/main function.
- iterations
    - The aggregate number of times the VUs in the test have executed the JS script (the default function).

`http_req_duration` and `iteration_duration` are basically the same thing when it's an http request.
`iterations` / time tells you how many requests per second, and the output even does the math for you.

As you can see, this easily handles over 120 requests per second with a sub 10 ms response time which is very good. Of course it is just testing localhost -> localhost so I'd expect low numbers.

I'll try it again with 10 and 100 virtual users.

![](/../assets/2020-09-17-08-31-18.png)

![](/../assets/2020-09-17-08-32-14.png)

It takes about 10x as long with 10 virtual users and 100x as long with 100 virtual users. However, I have a hunch that the sudoku library I'm using wasn't written for this use case and I've created a dummy endpoint that doesn't generate a sudoku board, but instead returns an already made sudoku board.

```python
@router.get("/sudoku/dummyboard", response_model=schemas.SudokuBoard)
def dummy_board():
    returnDict = {
        'board': [
            [0,0,7,0,4,0,0,0,0],
            [0,0,0,0,0,8,0,0,6],
            [0,4,1,0,0,0,9,0,0],
            [0,0,0,0,0,0,1,7,0],
            [0,0,0,0,0,6,0,0,0],
            [0,0,8,7,0,0,2,0,0],
            [3,0,0,0,0,0,0,0,0],
            [0,0,0,1,2,0,0,0,0],
            [8,6,0,0,7,0,0,0,5]
        ],
        'seed': 1,
        'difficulty': 0.5
    }
    return returnDict    
```

Let's update the url and hit it with 1, 10, and 100 virtual users:

![](/../assets/2020-09-17-08-36-31.png)

![](/../assets/2020-09-17-08-37-24.png)

![](/../assets/2020-09-17-08-38-08.png)

Exactly as I'd suspected, using an already generated sudoku board allows over 1000 requests to be responded to in a second with a sub 10ms response time. Even looking at the 100 virtual users, it's serving up 1200+ responses with a sub 100ms response time which is low enough that it gives the feeling of instantaneous response [[source]](https://www.nngroup.com/articles/website-response-times/).

Is this really a problem, will I have to rewrite this sudoku library? The answer is No, not really. This unoptimized library can still handle 100 requests per second, which is 100 * 60 (seconds) * 60 (minutes) * 24 (hours) or 8,640,000 daily users (assuming evenly spaced out requests). If this API receives that many users, I will either rewrite the library myself (lol) or hunt down a more optimized library myself (way more likely).