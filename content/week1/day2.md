+++
date = "2017-06-27T09:24:18-04:00"
title = "Day 2: Functions and the DOM"
toc = true
weight = 2
prev = "week1/day1"

+++

<date>Tuesday, June 27, 2017</date>

## Lecture Videos

Morning:

* [Playlist](https://www.youtube.com/playlist?list=PLuT2TqJuwaY882Figl-Tr-VXWweaeS45B) | [Day 2, part 1](https://www.youtube.com/watch?v=XXy6WXyResk&index=6&list=PLuT2TqJuwaY882Figl-Tr-VXWweaeS45B)

Afternoon:

* [Playlist](https://www.youtube.com/playlist?list=PLuT2TqJuwaY_yOPNQJLn2Ya_hfes8g2fv) | [Day 2, part 1](https://www.youtube.com/watch?v=YA4_zuVsu5s&index=10&list=PLuT2TqJuwaY_yOPNQJLn2Ya_hfes8g2fv)

## Topics

### Functions
* Function Expressions
* Function Declarations
* Functions as Object properties (methods)
* Variable Scope (`var`, `const`, `let`)

### DOM
* Adding HTML content to an existing element with `someElement.innerHTML`
* Creating elements with `document.createElement`
* Setting style properties with `someElement.style.stylePropertyName`
* Appending child elements with `someElement.appendChild`
* [REM vs EM - The Great Debate](https://zellwk.com/blog/rem-vs-em/)

## Examples

### Functions
{{< code js >}}
  // function declaration - use 'function' keyword
  function aMostExcellentFunction() {
    console.log('This function is great!')
  }

  aMostExcellentFunction() // => 'This function is great!'

  // function expression - defines a function as part of a larger expression syntax
  // (usually assignment to a variable)
  const anotherExcellentFunction = () => {
    console.log('This function is also great!')
  }

  anotherExcellentFunction() // => 'This function is also great!'

  // functions as object properties (also known as 'methods')
  const myObject = {
    myMethod() {
      console.log('I am a method!')
    }
  }

  myObject.myMethod() // => 'I am a method!'
{{< /code >}}

### Variable Scope
The biggest difference between `var` and `let` is that `var` variables are scoped to the _function_ in which they are declared, while `let` variables are scoped to the _block_ in which they are declared.  One of the easiest examples to see this behavior is in a simple `for` loop.

{{< code js >}}
function loopStuff() {
  for (var i = 0; i < 5; i++) {
    // do stuff in the loop
  }
  console.log(i)
}

loopStuff() // => 5

function loopMoreStuff() {
  for (let i = 0; i < 5; i++) {
    // do stuff in the loop
  }
  console.log(i)
}

loopMoreStuff() // => Uncaught ReferenceError: i is not defined
{{< /code >}}

In the function `loopStuff`, `var i` is still available outside the `for` loop so it can be logged to the console.  It is scoped to the function itself.

In the function `loopMoreStuff`, `let i` is not available outside the block it is scoped to (the `for` loop).

The main difference between `const` and `var`/`let` is that `const` cannot be reassigned.
{{< code js >}}
let variableOne = 4
variableOne = 5

var variableTwo = 4
variableTwo = 5

const variableThree = 4
variableThree = 5 // => Uncaught TypeError: Assignment to constant variable
{{< /code >}}

{{% aside tip "Default to using const" %}}
Always use `const` as your default way to declare variables, unless you know specifically that you will need to reassign it, in which case use `let`.  You should rarely, if ever, use `var`.  For further reading, check out [this article](https://medium.com/javascript-scene/javascript-es6-var-let-or-const-ba58b8dcde75)
{{% /aside %}}

### DOM
If we start with the following markup:
{{< code html >}}
&lt;div id=&quot;my-div&quot;&gt;&lt;/div&gt;
{{< /code >}}
We can add additional markup to it programmatically using JavaScript.  One way is to create new HTMl elements using `document.createElement`, and adding them by using `appendChild`.  Styling of the element can even be changed by manipulating the element's `style` property.

{{< code js >}}
// create an h1 and modify text content and color
const heading = document.createElement('h1')
heading.textContent = "This is the best heading I've ever seen"
heading.style.color = "red"

// get a reference to the existing div and add the heading as a child element
const div = document.querySelector('#my-div')
div.appendChild(heading)
{{< /code >}}

This will produce the following markup:
{{< code html >}}
&lt;div id=&quot;my-div&quot;&gt;
  &lt;h1 style=&quot;color: red;&quot;&gt;This is the best heading I've ever seen&lt;/h1&gt;
&lt;/div&gt;
{{< /code >}}

## Presentations

* [Review: HTML and the DOM](/02-html-dom.pdf)

## Projects

### Person Stats

[Morning](https://github.com/xtbc17s3/person-stats/tree/0bf23bc0b2f996a8718ff9d70812902a4ba68a08) | [Afternoon](https://github.com/xtbc17s3/person-stats/tree/6996de3bb9db203a8a0a1417d7fc365e76034916)

## Homework

* Create a new function called `renderColor` that returns a `div` element.
* Call that function when adding that `div` to `colorItem`.

### Bonus Credit

* Create a new function called `renderListItem`.
* Use it to create list items for each stat.

### Super Mega Bonus Credit

* Create a new function called `renderList`.
* Use it to create the list for each person's stats.
* Call `renderListItem` from `renderList`, not directly from `handleSubmit`.

### Super Mega Bonus Credit Hyper Fighting

* Be Strong!  Do not resort to the use of `innerHTML`.  Keep using `document.createElement`.
