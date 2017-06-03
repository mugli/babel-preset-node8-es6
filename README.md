# Babel 6.x presets for Node 8.x


## Key features:

* Object spread (via [babel-plugin-syntax-object-rest-spread](https://www.npmjs.com/package/babel-plugin-syntax-object-rest-spread) and [babel-plugin-transform-object-rest-spread](https://www.npmjs.com/package/babel-plugin-transform-object-rest-spread))

## Installation

Install via NPM the usual way:

`npm i babel-preset-node8-es6`

## Usage

### Via `.babelrc` (recommended)

Create a `.babelrc` file in your project root, and include 'node8-es6' in your preset path:

```js
{
  "presets": [
    "node8-es6"
  ]
}
```
