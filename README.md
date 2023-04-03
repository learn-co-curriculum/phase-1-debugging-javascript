# Debugging JavaScript

## Learning Goals

- Debug JavaScript code using `console.log()`
- Debug JavaScript code using Chrome's developer tools
- Review how to access information from objects and how to iterate through
  objects
- Use the debugger with deeply nested objects
- Use the debugger when writing code

## Introduction

Debugging is one of the most useful and important skills you can learn as a
developer. Faced with dozens, or hundreds, or even thousands of lines of code,
finding the bit of code that's causing a bug can be a daunting task! Luckily,
JavaScript and Chrome have some really great tools to help you find where the
problem code is — or even avoid bugs in the first place!

That's right: these tools aren't just good for debugging! They can also be used
as you're _developing_ your code, to check the values of variables as you go to
make sure the code is doing what you intend.

We have two options to debug our JavaScript programs:

1. Use `console.log()` to print out parts of our program.
2. Use a debug tool to pause our program, step through it and inspect values.

We will go over each of these options.

## Debugging Using `console.log()`

Using `console.log()` is an absolutely legitimate debugging technique. It can be
nice because you can see lots of output from your program without it stopping
and you can see how values change over time as you print them out. For example,
after calculating a variable or executing a function, you can log the result to
make sure it's what you expect. If you're executing a loop, you can log values
inside the loop to see how they change with each iteration.

The disadvantage of using `console.log()` is that you have to manually type out
exactly what you want to see, run the code, add or change your logs, run the
code again, and so on. You don't have the flexibility to examine other variables
or try code as you go. Also, you can wind up with your code littered with lots
of `console.log()`s. If you want to follow best practice (always a good idea),
you'll need to go back and delete them all once you're done debugging. However,
for a quick check of a particular value or two, `console.log()` can be a great
way to go.

## Debugging Using a Debug Tool

Unlike debugging with `console.log()`, using a debug tool will allow you to
pause your program and interact in more powerful ways in the middle of a
debugging session. You can set more breakpoints while you're debugging, type new
code into the console to see what happens, and interactively investigate the
value of different parts of your program.

If you completed the "Debugging in Node" lesson in the Prep course, you already
know a little about one of the tools available to you for debugging JavaScript
code. Frankly, however, the Node debugger isn't the best option: it's not very
intuitive and it's tricky to use. In this lesson, we'll learn about another
option: debugging with Chrome developer tools.

### Debugging Using Chrome Developer Tools

Included in the repo for this lesson are an `index.html` file and a `src` folder
containing three JavaScript files. If you take a look at the `index.html` file,
you'll see that those three scripts have been attached (the last one is
currently commented out). When we open the html page in the browser, Chrome will
execute the JavaScript in the included script files and we can open Chrome's dev
tools to debug our program.

Open up the `src` folder in VS Code and take a look at `01-simple-debug.js`.
We'll be debugging this file in the Chrome debugger.

Note that we've added the `debugger` keyword in a couple of places. That will
stop our program in those places and allow us to inspect it with Chrome's dev
tools.

To get started, open the `index.html` file in the browser, then open the dev
tools, either by pressing `F12`, or by using the appropriate keyboard shortcut:
`cmd+opt+J` for Mac or `ctrl+Shift+J` for Windows or Linux.

Click on the Sources tab. You should now see the source code in the main window,
in this case, `01-simple-debug.js`. You should also see the Console window open
at the bottom. If it isn't open, you can press ESC to toggle it. The console
should show the output from the two `console.log()`s in our code.

![browser with dev tools open](https://curriculum-content.s3.amazonaws.com/phase-1/debugging-javascript/dev-tools.png)

Refresh the page with the dev tools panel open using `cmd+R`/`ctrl+R`. This will
reload the code and start the debugger.

**Note**: Refreshing the page is necessary because the debugger will only be
triggered if the dev tools are open.

![code paused in debugger](https://curriculum-content.s3.amazonaws.com/phase-1/debugging-javascript/paused-in-debugger.png)

Take a look at the Sources pane to see your source code and note that the
debugger has paused your program after the first `console.log()`. This is what's
known as a breakpoint — a place in the code where execution is paused for
purposes of debugging. If you look in the console, you will only see output from
the first `console.log()`; the debugger has paused execution before the second
log.

In the Scope section on the right side of the page, you'll see that the values
of `x` and `y` are currently undefined. Here, too, this is because the code
where those variables are initialized has not been executed yet. When the
debugger is running, you can also check the current value of a variable by
typing its name into the console or by hovering over it in the source code, but
only if it has already been initialized at the point in the code where the
debugger is paused.

At the top of the debugger window there's a "Paused in debugger" message along
with two buttons: a "play" button and a "step over" button. Clicking the play
button resumes execution until either another breakpoint is reached or the code
finishes executing. Go ahead and click the play button; it will pause again at
the second debugger. You will also see the values of `x` and `y` shown in the
Scope section. Press the play button once more. The code will finish executing
and exit the debugger. You will now see the second message logged in the
console.

Refresh the page again to restart execution and re-enter the debugger. This
time, try pressing the step over button and see what happens. In each step, be
sure to note any changes in the Scope section or the console. Note that the line
of code that the debugger is paused on executes _after_ you press the step over
button.

## Brief Review: Accessing Values and Iterating Through Objects

There are three ways to access information in JavaScript objects:

- Access values directly using the key
- Iterate through the object using a `for...in` loop
- Use JavaScript's built-in `Object` methods

### Accessing Values Directly

To access values in objects directly, we use dot notation if we can:

```js
const obj = { foo: 42, bar: 83 };
obj.foo;
// => 42
obj.bar;
// => 83
```

If the name of a key doesn't conform to JavaScript's variable naming rules, dot
notation won't work so we use bracket notation instead:

```js
const obj = { foo: 42, bar: 83, "key w/ spaces": true };
obj["key w/ spaces"];
// => true
```

With nested objects, we can chain the commands:

```js
const obj = { foo: 42, bar: 83, "key w/ spaces": {baz: 79} };
obj["key w/ spaces"]["baz"];
// => 79
```

Question: Do you think JavaScript will let us combine dot and bracket notation
in a command chain?

<details><summary><b>Answer</b></summary><p>
    <p>Yes!</p>
    <code>const obj = { foo: 42, bar: 83, "key w/ spaces": {baz: 79} };</code><br/>
    <code>obj["key w/ spaces"].baz;</code><br/>
    <code>// => 79</code>
</p></details>

### Using a Loop to Access Values

To iterate over all the keys in an object and access their associated values, we
use a `for..in` loop. Recall that bracket notation is also required in this
case, because we're using a _variable_ to access the keys' names:

```js
const obj = { foo: 42, bar: 83, "key w/ spaces": {baz: 79, qux: 112} };
for (const key in obj) {
  const value = obj[key];
  console.log(`key: ${key}, value: ${value}`);
}
// =>
// key: foo, value: 42
// key: bar, value: 83
// key: key w/ spaces, value: { baz: 79, qux: 112 }
```

Note what's logged in the third iteration through the array. The "key w/ spaces"
key points to the nested object, and JavaScript will not automatically iterate
through any structures nested within our object.

**Challenge:** Write a `for...in` loop that accesses and logs the values in the
_nested_ object.

<details><summary><b>Hint</b></summary><p>
    <ul>
      <li>Instead of iterating over the object itself, you'll need to iterate over the value associated with the "key w/ spaces" key</li>
    </ul>
</p></details>

<details><summary><b>Answer</b></summary><p>
    <p>Here's one possible solution:</p>
    <code>const innerObj = obj["key w/ spaces"];</code><br/>
    <code>for (const innerKey in innerObj) {</code><br/>
    <code>const innerVal = innerObj[innerKey];</code><br/>
    <code>console.log(`key: ${innerKey}, value: ${innerVal}`)</code><br/>
    <code>}</code>
</p></details>

### Using Built-In Object Methods to Access Information

Finally, there are several built-in methods attached to the `Object` class
that you can use to access keys, values, or entries:

- `Object.keys(obj)` returns an array of all keys.
- `Object.values(obj)` returns an array of all values.
- `Object.entries(obj)` returns an array of arrays; each inner array contains
  both the key and value for one of the object's properties.

```js
const obj = { foo: 42, bar: 83, baz: 79 };
console.log("Object.keys(obj) =>", Object.keys(obj));
console.log("Object.values(obj) =>", Object.values(obj));
console.log("Object.entries(obj) =>", Object.entries(obj));
// =>
// Object.keys(obj) => [ 'foo', 'bar', 'baz' ]
// Object.values(obj) => [ 42, 83, 79 ]
// Object.entries(obj) => [ [ 'foo', 42 ], [ 'bar', 83 ], [ 'baz', 79 ] ]
```

Next, you'll practice your debugging skills and knowledge about accessing
information in objects to get ready for the lab that follows this lesson. To do
this, you'll use the `gameObject()` function you created in the last lesson.

## Using the Debugger to Help Write Code

Copy your `gameObject()` function and paste it into `src/00-object-ball.js`.
Next, go to the `index.html` file, comment out the second `<script>` element,
`01-simple-debug.js`, and uncomment `02-advanced-debug.js`.

Open `index.html` in Chrome, then open Chrome's developer tools and refresh the
page to enter the debugger. You should see that the source code in the main
window is the code in the `02-advanced-debug.js` file:

```js
console.log("Advanced debugging example running.");
debugger;

// first, define the function.
function debugPractice() {

}

// then, call the function so it runs!
debugPractice();
```

Let's start by creating a variable to hold the value of our game object. We'll
want to take a look at it in the debugger, so we'll add a debugger on the next
line:

```js
function debugPractice() {
  const game = gameObject();
  debugger;
}
```

Refresh the browser page and press the play button to advance to the debugger we
just added. (Feel free to delete the first debugger if you like.)

![paused in advanced debug](https://curriculum-content.s3.amazonaws.com/phase-1/debugging-javascript/paused-in-advanced-debug.png)

Note that the debugger shows you the top level of `game` highlighted in the main
window. If you want to drill down into the object more, you can do that in the
"Scope" section on the right, or you may find the easiest way is to type the
variable into the console and explore it there.

The top level of our game object consists of two keys (`home` and `away`), each
of which points to another object. Let's use a `for...in` loop to iterate
through the two keys and move our debugger inside the loop:

```js
function debugPractice() {
  const game = gameObject();
  for (const gameKey in game) {
    // are you ABSOLUTELY SURE what 'gameKey' is?
    // use the debugger to find out!
    debugger;
  }
}
```

Our loop is iterating over the top level of our object, so what do you expect
the value of `gameKey` to be in the first iteration? Use the debugger to check
whether you're right.

Next, click the play button. The code will continue executing until it gets to
the next debugger. In this case, it will enter the second iteration through
`game` and stop on the same line. Now what do you expect the value of `gameKey`
to be?

If you type `game` into the console and expand the object, you'll see that each
of the two keys points to another object:

![expanded view of game object](https://curriculum-content.s3.amazonaws.com/phase-1/debugging-javascript/game-object-expanded-in-console.png)

To drill down more, we'll need to nest another loop inside the current loop. For
this loop, we'll first capture the object associated with the current key
(`home` or `away`) and save it to a variable, then iterate through _that_
object:

```js
function debugPractice() {
  const game = gameObject();
  for (const gameKey in game) {
    const teamObj = game[gameKey];
    for (const teamKey in teamObj) {
      // are you ABSOLUTELY SURE what 'teamKey' is?
      // use debugger to find out!
      debugger;
    }
  }
}
```

Refresh the page and take a look at `teamObj` and `teamKey`. Are they what you
expected?

At this point, you may find (if you haven't already) that you're not seeing the
key you expected to see. Recall that objects in JavaScript, unlike arrays, are
not ordered. What this means is that you may not always see the key you expect
when you inspect variables containing object keys, such as `gameKey` and
`teamKey`. Inspect the `teamObj` variable. You'll see that it's an object that
contains three keys, `teamName`, `colors` and `game`. When you inspect
`teamKey`, as long as it's one of those three values, the iteration is set up
properly.

You can continue to drill down into the `game` object using the same basic
technique:

1. Create a variable to contain the object (or array) you want to access.
2. Create a `for...in` (or `for...of`) loop to iterate through it.
3. Use the debugger to make sure that you're seeing what you expect. If not, you
   can use the console to experiment with your variables until you figure out
   how to access what you need.

**Challenge:** Add the next step to your `debugPractice()` function to iterate
through the players for each team, using the steps above. Use the debugger to
make sure you're accessing the stats for each player in turn for each team.

<details><summary><b>Answer</b></summary>
    <code>const playerData = teamObj.players;</code><br/>
    <code>for (const key in playerData) {</code><br/>
    <code>debugger;</code><br/>
    <code>}</code>
</details>

NOTE: If you simply add code to loop through the players inside our existing
code, the code will probably not function the way you are expecting it to. Be
sure to step through the inner loop, paying attention to the values of the
variables (in particular, the `key` variable) until you figure out what's going
on. Once you've done that, see if you can figure out how to fix it.

<details><summary><b>What's causing the bug</b></summary>
<p>Launch the debugger and take a look at the value of the <code>teamObj</code> variable. Note that it is an object with three properties: <code>teamName</code>, <code>colors</code>, and <code>players</code>. Because we are iterating through that object in the loop that encloses the loop through the player stats, <strong>the inner loop is being executed three times for each team — once for each property in <code>teamObj</code>!<strong></p>
</details>

<details><summary><b>How to fix it</b></summary>
<p>There are two ways you can fix the problem:</p>
  <ol>
    <li>Remove the loop through <code>teamObj</code> and go directly to defining the <code>playerData</code> variable and looping through that.</li>
    <li>Wrap the inner loop in an <code>if</code> statement so it only executes if the current <code>teamKey</code> is "players".</li>
  </ol>
  <p>Which one is the better choice? That will depend on whether you also want to access any of the other information inside <code>teamObj</code>.</p>
</details>

In the lab that follows, you will be building a number of functions to access
different information from the object returned by `gameObject()`. We encourage
you to use the `debugPractice()` function to help you figure out how to write
those functions. Be sure to **use the strategy of placing LOTS of `debugger`
keywords when you iterate over things in order to investigate your program and
solve the lab.**

## Conclusion

In this lesson, we've gone over some of he debugging techniques you can use to
debug problems with your code and to check how your code is working as you write
it. Adding a debugger as you build each step of a complicated program will be
**much** easier than writing the whole program and then trying to find where in
the code a bug is occurring.

The lab that follows is a challenging one! We strongly encourage you to use the
debugging techniques you learned in this lesson to help you through it.

## Resources

- [Chrome Developer Tools](https://developer.chrome.com/docs/devtools/overview/)
