# Babel 6.x presets for Node 6.x (updated regularly!)

Node 6.x brings ~95% [native ES6/ES2015 coverage](http://node.green).

This preset for Babel 6 attempts to bridge the gap for the much of the remaining 5% of the evolving ECMAScript spec using [Babel plug-ins](https://github.com/babel/babel/tree/master/packages), from stage 0 and up.

## Motivation

Babel 6.x is awesome, but simply including the [ES2015 preset](https://www.npmjs.com/package/babel-preset-es2015) means you're transpiling features
that your Node 6.x installation can already do faster and natively, replacing them with inferior / old code.

This preset complements existing V8-native functionality - it doesn't work _around_ it.

The end result is nearly always a faster build and script execution time.

## Key features:

* Removes trailing commas from function calls (via [babel-plugin-syntax-trailing-function-commas](https://www.npmjs.com/package/babel-plugin-syntax-trailing-function-commas))
* CommonJS import/export module syntax ([babel-plugin-transform-es2015-modules-commonjs](https://www.npmjs.com/package/babel-plugin-transform-es2015-modules-commonjs))
* Sticky RegEx (via [babel-plugin-transform-es2015-sticky-regex](https://www.npmjs.com/package/babel-plugin-transform-es2015-sticky-regex))
* Async/await (via [babel-plugin-transform-async-to-generator](https://www.npmjs.com/package/babel-plugin-transform-async-to-generator) and [babel-plugin-syntax-async-functions](https://www.npmjs.com/package/babel-plugin-syntax-async-functions))

**Note: This package originally shipped with the React preset, but to avoid bloat, doesn't any longer. If you want to add that, please install [babel-preset-react](https://www.npmjs.com/package/babel-preset-react) too**

## Usage instructions

## Installation

Install via NPM the usual way:

`npm i babel-preset-node6-es6`

## Usage

### Via `.babelrc` (recommended)

Create a `.babelrc` file in your project root, and include 'node6-es6' in your preset path:

```js
{
  "presets": [
    "node6-es6"
  ]
}
```

Now whenever you run `babel-node`, it will polyfill your app with the ES2015 features that Node 6 is missing.

### Via CLI
`$ babel script.js --presets node6-es6`

### Via Node API

If you don't want to use a project-wide `.babelrc` file (as above):

```js
require("babel-core").transform("code", {
  presets: ["node6-es6"]
});
```

And if you _do_, and you want to use vanilla `node` instead of `babel-node` as your CLI, you can create an entry script that references your pre-transpiled code like so:

```js
require('babel-register');
require('path/to/es6/script');
```

... which will then run everywhere Node can.

Of course, make sure to `npm i -S babel-core` or `npm i -S babel-register` respectively, to grab the NPM packages you'll need to transpile on-the-fly.

### Webpack, Gulp, Browserify, etc

Follow vendor instructions and include `node6-es6` in your babel "preset" list.

## How to add React support

Babel has a ready-made preset for React, and you now need to install it separately.

Just grab it via NPM:

`npm i babel-preset-react`

And then add it to your "presets" list in `.babelrc`:

```js
{
  "presets": [
    "node6-es6",
    "react"
  ]
}
```

## How to use async/await

The async/await proposal allows you to wait on a Promise, and write asynchronous code that looks synchronous.

Here's an example:

```js
async function getUsers(howMany) {
  try {
    const response = await fetch(`http://jsonplaceholder.typicode.com/users/${howMany}`); // <-- a Promise
    return response.json(); // <-- Another promise.
  } catch(e) {
    console.log('some kind of error occurred: ', e)
  }
}

getUsers(10).then(users => {
  // "users" contains the result of `response.json()`. Async functions *always*
  // return a promise, even if that means wrapping a non-Promise in Promise.resolve
})
```

In the above example, `fetch` returns a promise. By prefixing the function with `async` and prefixing every Promise with `await`, we avoid the typical `.then()` chain inside of the function block and can reason about the flow of the application a little more clearly.

We can also wrap promises in `try/catch` blocks, instead of bolting on `.catch()` chains.

The necessary babel plug-ins to use async/await are included in this package, so you can use this syntax right away.
