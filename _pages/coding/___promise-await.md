---
title: Promises, async, await
---

## Promises & async / await

Some of the things we ask our programs to do take time. And not just a little bit of time: downloading a file from the internet can take several seconds. If our program waits for an image, sound or data file, everything halts: animations stop, buttons don't respond, our sketches freeze up. To get around this, we use JavaScript's built-in tools for handling things that take time: **Promises** and **async / await**.

Take a look at this code:

```js
let data = loadJSON("https://data.com/rent.json");
```

Our intuition says that the variable `data` should now contain the data from the JSON file. But the file lives on a server somewhere on the internet. Downloading it takes time — many milliseconds at best, seconds at worst. JavaScript can't just stop everything to wait for it.

`loadJSON()` gives us a **Promise** and later, the data.

## Promise

A **Promise** is a JavaScript object that represents a value that doesn't exist yet, but will exist in the future. It's an IOU for something that's on its way.

Think about ordering food at a restaurant. You don't get the food right away. Instead, you get a number — a promise — that says "your dish will be ready soon". You can do other things while you wait: check your phone, talk to a friend, read a book, etc. When the food is ready, you hear your number called and you go pick it up.

A Promise works the same way. It can be in one of these states:

- **pending** — the data is on its way (the file is being downloaded).
- **fulfilled** — the data is ready (the file arrived and is stored).
- **rejected** — something went wrong (the file doesn't exist, or the server didn't answer).

Every p5.js function that loads something returns a Promise: `loadJSON()`, `loadImage()`, `loadTable()`, `loadStrings()`, `loadFont()`, and `loadSound()`. To get the actual value out of a Promise, we use a keyword called `await`.

## await

The `await` keyword does what it sounds like: it waits. When we write `await` before a Promise, our code pauses on that line until the Promise is fulfilled.

`await` can only be used inside functions marked with the `async` keyword. An `async` function is a function that is allowed to pause and wait: it tells JavaScript "this function may use `await`". It also means that the function itself returns a Promise — a value that will be ready later, once everything inside the function is done.

In p5.js we can make our `setup()` function `async`. p5.js runs `setup()` and waits for it to finish — including all of its `await`s — before it starts calling `draw()`:

```js
let data;

async function setup() {
  createCanvas(640, 480);
  data = await loadJSON("https://data.com/rent.json");
  // code below this line will not run until the promise of loadJSON is fulfilled
}
```

Let's break it down:

1. The sketch starts and p5.js calls `setup()`.
2. `loadJSON()` begins downloading the JSON file. Because of `await`, it pauses right there.
3. When the file arrives, the Promise is fulfilled, and the downloaded data is assigned to `data`.
4. `setup()` can now finish, running any code that follows `loadJSON()`.

## Sounds great, what happens if a file isn't there?

Files are not guaranteed to load. The file might not exist, the URL might be wrong, the network might fail, or the server might be down. When that happens, the Promise is **rejected** instead of fulfilled, and our `await` line throws an error.

We can handle that with `try` / `catch`, which lets us specify what to do if a block of code fails:

```js
let data;

async function setup() {
  createCanvas(640, 480);
  try {
    data = await loadJSON("https://data.com/rent.json");
  } catch (err) {
    print("Could not load the data: " + err);
    data = []; // use an empty array as a fallback, or better still, store some dummy data 
  }
}
```

With a fallback value in place, the rest of our sketch can keep running even when the load fails.

## I see reference to callbacks, what's that?

Before `async` / `await` existed, JavaScript used **callback functions**: you would pass a function as an argument, and that function would be called once the value was ready.

```js
// callback version
loadJSON("https://data.com/rent.json", (data) => {
  // use data here, inside the callback
});
```

The problem with callbacks is that if we need to load several things in order, functions need to nest inside of functions and the code becomes hard to read. With `await` it reads like synchronous code:

```js
// await version: reads top to bottom, like synchronous code
data = await loadJSON("https://data.com/rent.json");
// use data here, in the normal flow of our code
```

p5.js load functions still accept callbacks, But `await` is the pattern we'll use when we write our own code. We may encounter some libraries that rely on callbacks, but we're going to avoid them when possible.

## Loading all kinds of things

This pattern works for all of p5's load functions. 

Images:

```js
let mImage;

async function setup() {
  createCanvas(640, 480);
  mImage = await loadImage("photo.jpg");
}

function draw() {
  image(mImage, 0, 0);
}
```

Fonts:

```js
let myFont;

async function setup() {
  createCanvas(640, 480);
  myFont = await loadFont("font.otf");
  textFont(myFont);
}
```

Tables and text files:

```js
let mTable;
let mLines;

async function setup() {
  createCanvas(640, 480);
  mTable = await loadTable("data.csv", "csv", "header");
  mLines = await loadStrings("story.txt");
}
```

Sound files:

```js
let mSound;

async function setup() {
  createCanvas(640, 480);
  mSound = await loadSound("song.mp3");
}

function mousePressed() {
  mSound.play();
}
```
## Loading several things at once

If we `await` one load function one after another, they download in sequence: first an image, then a font, then some CSV data (or however you arrange that code). Each file would have to finish before the next one starts.

If your files don't depend on each other, so they can all be downloaded at the same time. JavaScript gives us a helper for this: `Promise.all()`. It takes an array of Promises and returns a single Promise that is fulfilled when all of them are ready:

```js
let images;

async function setup() {
  createCanvas(640, 480);
  let results = await Promise.all([
    loadImage("one.png"),
    loadImage("two.png"),
    loadImage("three.png")
  ]);
  // results is an array of the three images, in order
  images = results;
}
```

Using `Promise.all()` is usually faster than loading the files one at a time, because the downloads overlap.


## Recap

- A **Promise** represents a value that will be available in the future.
- `loadJSON()`, `loadImage()`, `loadTable()`, `loadStrings()`, `loadFont()` and `loadSound()` all return Promises.
- `await` pauses our code until a Promise is fulfilled, and `await` only works inside `async` functions.
- An `async` function is allowed to use `await`, and returns a Promise itself.
- In p5.js, we load everything we need at the top of an `async function setup()`, before p5.js starts calling `draw()`.
- Use `Promise.all()` to load several files at the same time.
- Use `try` / `catch` to handle files that fail to load.
- `await` works with any Promise, not just p5.js functions.

Now that we know how to wait for our files, we can put this into practice. Next we'll load real data into our sketches — JSON and CSV files — and visualize it.

