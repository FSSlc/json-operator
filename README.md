# JSON Operator

Perform efficient path based operations on JSON Objects (or most Javascript data object)

## Install

`npm i json-operator --save`

## API

- constructor(target, path, jp)

- setters/getters 
  - .path : default operator path
  - .target : target object
  - .jp : jsonpath engine      

Note that `indent`, `path` and `opts` are optional. 

- targetAsStr(indent = 2) : get prettified string of target obj
- display(indent, logger) : display prettified string of target obj using logger (default console.log) 
- query(path) : get all match results in list
- value(path) : get first match result
- parent(path) : parent of first match
- delete(path) : delete all matches
- set(obj, path) : set matches to new object
- merge(obj, opts) : merge matches with new object
- reverseMerge(obj, opts) : merge matches with new object


## Usage

Given the following book store:

```js
{
  "store": {
    "book": [ 
      {
        "category": "reference",
        "author": "Nigel Rees",
        "title": "Sayings of the Century",
        "price": 8.95
      }, {
        "category": "fiction",
        "author": "Evelyn Waugh",
        "title": "Sword of Honour",
        "price": 12.99
      }, {
        "category": "fiction",
        "author": "Herman Melville",
        "title": "Moby Dick",
        "isbn": "0-553-21311-3",
        "price": 8.99
      }, {
         "category": "fiction",
        "author": "J. R. R. Tolkien",
        "title": "The Lord of the Rings",
        "isbn": "0-395-19395-8",
        "price": 22.99
      }
    ],
    "bicycle": {
      "color": "red",
      "price": 19.95
    }
  }
}
```

Note: Books are indexed from 0 as normal, so book [2] is the 3rd book in the list.

We perform the following operations to:
- get the value of book [2]
- get the value of book [3]
- set the default opertor path to book [3]
- overwrite book 3 with book [2]
- merge `{price:100}` onto book [3]
- reverse order merge `{rating: 4}` onto book [3]
- delete book [3]
- display full target object after operations

Full API example

```js
const operator = new JsonOperator(store)

let paths = {
  book2: '$..book[2]',
  book3: '$..book[3]'
}

// query for all matching path
let book2results = operator.query(paths.book2);
console.log('original book 2', book2results)

// get the first match for book 2
let book2val = operator.value(paths.book2);
console.log('original book 2 value', book2val)

// let book2par = operator.parent(paths.book2);
// console.log('original book 2 parent', book2par)

// get the first match for book 3 
let book3 = operator.value(paths.book3);
console.log('original book 3', book3)

// set default path to use for the following ops
operator.path = paths.book3;

// overwrite book3 with book2
operator.set(book2val)
let book3Set = operator.value(); 

console.log('original book 3', book3)
console.log('book 3 set', book3Set)

// overwrite book2 with value found at book3
operator.merge({price: 100})
let book3Merged = operator.value()

console.log('original book 3', book3)
console.log('book 3 set', book3Set)
console.log('book 3 set', book3Merged)

// operator.merge({price: 100}, {reverse: true})
operator.merge({rating: 4}, {reverse: true, path: mergePath})
operator.reverseMerge({rating: 4})
operator.reverseMerge({rating: 4}, path)

operator.delete()
let book3deleted = operator.value()
console.log('book 3 deleted', book3deleted)
console.log('store after all operations', operator.targetAsStr())
```

The example can be found in `/examples/demo.js` in the repo. 

## Set alternative jsonpath engine

You can now also set an alternative `jsonpath` engine.

```js
const fastpath = require('fastpath');
operator.jp = fastpath;
``` 

## Alternatives

Some alternative `jsonpath` transformers

- [jsonpath-plus](https://www.npmjs.com/package/jsonpath-plus)
- [jsonpath-transform](https://www.npmjs.com/package/jsonpath-transform)
- [jsonpath-object-transform](https://www.npmjs.com/package/jsonpath-object-transform)
- [jsonpath-rep](https://www.npmjs.com/package/jsonpath-rep) with replacement

Would be nice if we could combine the best of all ;)

## JSON path alternative

- [jsonave](https://www.npmjs.com/package/jsonave)
- [fastpath](https://www.npmjs.com/package/fastpath)

## Misc

- [jsonpath-to-dot](https://www.npmjs.com/package/jsonpath-to-dot)

## Licences

MIT 

Kristian Mandrup <kmandrup@gmail.com> 2016