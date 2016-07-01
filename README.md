# html-whitespace-parser

[![npm](http://img.shields.io/npm/v/html-whitespace-parser.svg?style=flat)](https://badge.fury.io/js/html-whitespace-parser) [![tests](http://img.shields.io/travis/static-dev/html-whitespace-parser/master.svg?style=flat)](https://travis-ci.org/static-dev/html-whitespace-parser) [![dependencies](http://img.shields.io/david/static-dev/html-whitespace-parser.svg?style=flat)](https://david-dm.org/static-dev/html-whitespace-parser) [![coverage](http://img.shields.io/coveralls/static-dev/html-whitespace-parser.svg?style=flat)](https://coveralls.io/github/static-dev/html-whitespace-parser)

basic parser for whitespace-significant html

> **Note:** This project is in early development, and versioning is a little different. [Read this](http://markup.im/#q4_cRZ1Q) for more details.

### Why should you care?

I previously used jade, and jade is a fantastic library. But the time has come to move forward. It's not well-maintained anymore, and it's monolithic and extremely difficult to work with and understand (which presumably is part of the reason it's not well maintained).

This parser is intended to fit into PostHTML, and only do the most basic parsing tasks. Conditionals, variable interpolation and insertion, partials, blocks, mixins, etc. are all outside the scope of this library, and can be added through other posthtml plugins. It is simply a parser for a simple, jade-like whitespace-significant language that compiles to html.

This is my first time writing a lex/parser, and the code is heavily commented. Please feel free to read through and contribute! This is not intended to be the type of project that only one person can understand or write code for.

### Installation

`npm install html-whitespace-parser -S`

> **Note:** This project is compatible with node v6+ only

### Usage

This parser is very loose with its rules and standards. It is not responsible for enforcing good style or conventions, it's simply responsible for compiling your code. This means that you can use all sorts of invalid characters in attribute and tag names, and indentation rules are extremely loose.

As far as indentation rules, as long as one line is indented with more characters than the last, it will be nested. It doesn't matter if the number of characters that you use is consistent, or if they are spaces or tabs, or anything else. It's just the number of space characters used to indent, that's it. So you can get away with very messy code, if you want, but that's what linters are for.

Input:

```html
doctype html
html
  head
    title Testing
  body#index
    h1 Hello world!
    p.intro Wow what a great little language! Some features:
    ul(data-list='yep' @sortable)
      li: a(href='#') whitespace significant!
      li: a(href='#') simple classes and ids!
```

Pipeline:

```js
const posthtml = require('posthtml')
const whitespace = require('html-whitespace-parser')
const fs = require('fs')

const html = fs.readFileSync('./index.sml', 'utf8')

posthtml()
  .process(html, { parser: whitespace })
  .then((res) => console.log(res.html))
```

Output:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Testing</title>
  </head>
  <body id='index'>
    <h1>Hello world!</h1>
    <p class='intro'>Wow what a great little language! Some features:</p>
    <ul data-list='yep' @sortable>
      <li><a href='#'>whitespace significant!</a></li>
      <li><a href='#'>simple classes and ids!</a></li>
    </ul>
  </body>
</html>
```

---

Using this library directly will output a html syntax tree that mirrors posthtml's standard format. If you'd like to use it outside of posthtml, you are welcome to, but it's probably much simpler to use it with posthtml.

### License & Contributing

- Details on the license [can be found here](LICENSE.md)
- Details on running tests and contributing [can be found here](contributing.md)
