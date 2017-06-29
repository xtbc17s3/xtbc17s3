+++
date = "2017-06-28T09:55:32-04:00"
title = "Day 3: Arrays and Objects"
toc = true
weight = 3
prev = "week1/day2"
next = "week1/day4"

+++

<date>Wednesday, June 28, 2017</date>

## Lecture Videos

Morning:

[Playlist](https://www.youtube.com/playlist?list=PLuT2TqJuwaY882Figl-Tr-VXWweaeS45B) | [Day 3, Part 1](https://www.youtube.com/watch?v=1tMQlcsg0jc&index=19&list=PLuT2TqJuwaY882Figl-Tr-VXWweaeS45B)

Afternoon:

[Playlist](https://www.youtube.com/playlist?list=PLuT2TqJuwaY_yOPNQJLn2Ya_hfes8g2fv) | [Day 3, Part 1](https://www.youtube.com/watch?v=JtEbzz4-n9Q&index=23&list=PLuT2TqJuwaY_yOPNQJLn2Ya_hfes8g2fv)
 
## Topics

### Arrays
* `Array.map` - [Docs on MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map?v=control)
* [Understanding JavaScript's `map()`](https://www.discovermeteor.com/blog/understanding-javascript-map/) blog post

### Objects
* Object literals
* Property Naming
* Retrieving property values
* Setting property values

### Handling exceptions

* `try`
* `catch`
* `finally`

### `this`

* _default_ binding through function invocation
* _implicit_ binding through method calls
* _explicit_ binding with `.bind`

### Foundation

* The grid system
* Responsive design (adjusting style based on window size)
* [Foundation Docs](http://foundation.zurb.com/sites/docs/)

### Markdown

* [Markdown Cheatsheet](http://assemble.io/docs/Cheatsheet-Markdown.html)
* [Mastering Markdown](https://guides.github.com/features/mastering-markdown/)

## Examples

### Arrays
Arrays are extremely useful data structures in JavaScript, as they can be easily iterated and transformed through methods like `map`, `filter`, and `reduce`.  Sometimes, you may have an 'array-like' collection (like a `NodeList` or function arguments) that you would need to convert to an actual Array before you could use these methods.  This can be done using `Array.from`

{{< code js >}}
let paragraphs = document.querySelectorAll('p')
paragraphs.map((p) => {
  p.textContent = "This won't work because paragraphs is a NodeList, not Array!"
})
// => Uncaught TypeError: paragraphs.map is not a function

let actualArrayOfParagraphs = Array.from(paragraphs)
actualArrayOfParagraphs.map((p) => {
  p.textContent = "This totally does work because we created an Array from our NodeList!"
})
{{< /code >}}

{{% aside info "Requirements for 'Array.from'" %}}
What objects can you convert to an Array using 'Array.from'?  

* Any array-like object with a 'length' property and indexed elements
* Iterable objects (like Map or Set)

For more info, check out [this article](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from?v=control) on MDN.
{{% /aside %}}

### Objects
Almost everything in JavaScript is an Object.  The easiest way to create new Objects is with the [object initializer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer), more commonly known as 'object literal' syntax.  Basically, use curly braces to make an object `{}` and fill in the properties that you want.

Objects contain `key`/`value` pairs that allow you to set and retrieve values from them.

{{< code js >}}
// create a new object and assign some properties
const myObject = {
  prop1: 'Hello there',
  prop2: 42
}

// access the values in several ways, usually 'dot' or 'square bracket' notation
myObject.prop1 // => 'Hello there'
myObject['prop1'] //=> 'Hello there'

// new key/value pairs can also be assigned with these notations
myObject.prop3 = 'New Value!'
myObject['prop4'] = 'Newest Value!'

console.log(myObject)
// { 
//   prop1: 'Hello there',
//   prop2: 42,
//   prop3: 'New Value!',
//   prop4: 'Newest Value!'
// }
{{< /code >}}

We know that we can iterate through an Array using `map` or `forEach`.  Can we do something similar with objects?  There are a few ways to do it, but one of the newest and easiest is the `Object.keys` method.  It iterates through the enumerable properties of an object, returning an array of the property keys. Once we have an array of keys, we can `map` over _that_ and access each of the object properties individually.

{{< code js >}}
const myObj = {
  a: 'hi',
  b: 42,
  c: [1, 2, 3]
}

const myObjKeys = Object.keys(myObj)    // ['a', 'b', 'c']

myObjKeys.map(key => myObj[key])        // ['hi', 42, [1, 2, 3]]
{{< /code >}}

### Handling exceptions
In JavaScript (as in many languages), there is a way to 'try' a block of code that may produce an exception, and if it _does_ produce an exception, 'catch' it and execute a different block of code.  The catch block receives the exception as an argument.  There is also an optional 'finally' block, which will _always_ run, regardless of whether there was an exception.

{{< code js >}}
try {
  // try running this code first
  somethingThatMightBlowUp()
} catch (e) {
  // executes if the 'try' block produced an exception
  logMyErrors(e)
} finally {
  // always executes after the previous blocks have run
  console.log("Done!  Maybe there was an exception, maybe there wasn't.")
}
{{< /code >}}

For more info, read [this MDN article](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch).

### `this`

The same function can have different values for `this` depending on how the function is called/invoked.

[Try this example live on CodePen.](https://codepen.io/dstrus/pen/XgmLyv)

{{< code js >}}
const app = {
  sayYeah() {
    console.log(`Yeah, ${this}`)
  },
  
  toString() {
    return 'app object'
  }
}

// When invoked as a method
app.sayYeah() // "Yeah, app object"

// When invoked as an event handler
document
  .querySelector('button')
  .addEventListener('click', app.sayYeah)
  // "Yeah, [object HTMLButtonElement]"

// When manually set with bind
app.sayYeah.bind('w00t')() // "Yeah, w00t"
{{< /code >}}

### Foundation
Foundation is a CSS (and JS) framework that makes it easy to create stylish, responsive web pages.  The foundation (get it?) of it is the [grid system](http://foundation.zurb.com/grid.html).  The grid splits the page into 12 equally-sized columns, making it easy to set the alignment of elements on the page by specifying how many columns they span.

In addition, you can add sizes of 'small', 'medium', 'large', etc, to specify different behavior at different screen sizes.  In the following example, the two child divs will be full screen width at small screen sizes (stacked on top of each other), and half of the screen width at medium and larger screen sizes (appearing next to each other).

{{< code html >}}
&lt;div class=&quot;row&quot;&gt;
  &lt;div class=&quot;small-12 medium-6 columns&quot;&gt;
  &lt;/div&gt;
  &lt;div class=&quot;small-12 medium-6 columns&quot;&gt;
  &lt;/div&gt;
&lt;/div&gt;
{{< /code >}}

## Projects

### Person Stats
[Final Version](https://github.com/xtbc17s3/person-stats/tree/930ba46d35c4cd6c90f0e0eb5b460350999e6db4)

## Homework

* Store the flicks in an array, as well as in the DOM.

### Bonus Credit

* Add a _fav_ button to each list item.
* Make it change the appearance of that item (_e.g._ Add a background color.)

### Mega Bonus Credit

* Add a _remove_ button to each list item.
* Make that button actually work.

### Super Mega Bonus Credit

* Make sure that both of those buttons affect the array as well.
* Make the _fav_ button toggle the _fav_ status.

### Super Mega Bonus Credit Hyper Fighting

* Add buttons that move a flick up and down the list.
