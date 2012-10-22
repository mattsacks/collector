# Collector.js

Client-side MapReduce

## Why
By defining functions assigned to unique keys in an object, we can use the
closure to run any code local to whatever the current scope is. Anonymous
functions make this super awesome. 

## How to use
Including Collector.js on your page will expose two variables, `Collector` and
`collector` on the `window` object. `Collector` is a class for creating
instances of itself, `collector` is a vanilla instantiation.

`.collect(data, mappings[, reductions])` - the main function, it takes in a
dataset and runs a unique mapping across each item of data. if reductions, is
provided - it will run those over the mapped dataset.

* `data` (Array): an array of data to iterate across
* `mappings` (Object): an object of keys to functions that will be called on
  each datum in data. mapping function signature looks like `function(datum, i)`
  and should return a value.
* `reductions` (_optional_ Object): an object of identical keys to that of
  `mappings` that gets called after each mapping is called on data. iff the
  mapping function **returns** a result, the signature will be
  `function(current, result, datum, i)`. otherwise it will be `function(current,
  datum, i)`. this function **must** return a value in order for it to be of any
  real use

## Example
A simple map and reduction using numbers.

```javascript
var data = [0, 1, 2];

var mappings = {
  test: function(x, i) {
    return x + 1;
  }
};

var reductions = {
  test: function(current, result) {
    // if current hasn't been set yet (this is the first call of reduce), then
    // just return the result of the first mapping.  otherwise, add them.
    return current == null ? result : current + result; 
  }
}

var collection = collector.collect(data, mappings, reductions);
collection.test === 6; // 1 + 2 + 3
```

## License
(The MIT License)

Copyright (c) Matt Sacks

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the 'Software'), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to do so,
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.  IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
