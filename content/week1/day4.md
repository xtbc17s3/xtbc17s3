+++
date = "2017-06-29T13:29:29-04:00"
title = "Day 4: Cloning Elements and Data Attributes"
toc = true
weight = 4
prev = "week1/day3"

+++

<date>Thursday, June 29, 2017</date>

## Lecture Videos

Morning:

[Playlist](https://www.youtube.com/watch?v=1tMQlcsg0jc&index=19&list=PLuT2TqJuwaY882Figl-Tr-VXWweaeS45B) | [Day 4, Part 1](https://www.youtube.com/watch?v=6VZmXEDS9Xo&list=PLuT2TqJuwaY882Figl-Tr-VXWweaeS45B&index=35)

Afternoon:

[Playlist](https://www.youtube.com/playlist?list=PLuT2TqJuwaY_yOPNQJLn2Ya_hfes8g2fv) | [Day 4, Part 1](https://www.youtube.com/watch?v=OgEhC3PwpMQ&index=40&list=PLuT2TqJuwaY_yOPNQJLn2Ya_hfes8g2fv)

## Topics

### DOM Manipulation
* [data attributes](https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Use_data_attributes)
* [parentElement](https://developer.mozilla.org/en-US/docs/Web/API/Node/parentElement)
* [childNodes](https://developer.mozilla.org/en-US/docs/Web/API/Node/childNodes)
* [firstChild](https://developer.mozilla.org/en-US/docs/Web/API/Node/firstChild)
* [firstElementChild](https://developer.mozilla.org/en-US/docs/Web/API/ParentNode/firstElementChild)
* [insertBefore](https://developer.mozilla.org/en-US/docs/Web/API/Node/insertBefore)
* [closest](https://developer.mozilla.org/en-US/docs/Web/API/Element/closest) (experimental)

### Array methods
* [`Array.prototype` documentation on MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/prototype?v=control)
* [`unshift()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift?v=control)
* [`reverse()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse?v=control)

### Foundation
* [Button](http://foundation.zurb.com/sites/docs/button.html)
* [Button Group](http://foundation.zurb.com/sites/docs/button-group.html)
* [Un-bulleted List](http://foundation.zurb.com/sites/docs/typography-helpers.html#un-bulleted-list)

## Examples

### Data Attributes

HTML5 gave us a way to save extra information on a standard HTML Element via the `data-*` attributes. Basically, you can add any arbitrary information you want, prefixing the name of the attribute with `data-`.  This data is then accessible through JavaScript via the `someElement.dataset` object, or through CSS via `attr(data-*)`.

{{< code html >}}
&lt;div
  id="my-div"
  data-name="Awesome div"
  data-id="div-1234"
  data-color="blue"
  data-marshmallows="yummy"&gt;
&lt;/div&gt;
{{< /code >}}

{{< code js >}}
const myDiv = document.querySelector('#my-div')

myDiv.dataset.name            // "Awesome div"
myDiv.dataset.data-id         // "div-1234"
myDiv.dataset.color           // "blue"
myDiv.dataset.marshmallows    // "yummy"
{{< /code >}}

{{< code css >}}
#my-div {
  background-color: attr(data-color);
}

div[data-id='div-1234'] {
  height: 400px;
  width: 400px;
}
{{< /code >}}

### `.cloneNode`
Sometimes it may be easier to clone an existing node rather than build an entirely new one, especially if the markup is complex.  In our 'Michael Bay-watch' project, we kept a hidden `li` in the DOM that we cloned every time we needed to render a new list item.  

{{< code html >}}
&lt;li class="flick grid-x align-middle template"&gt;
  &lt;span class="flick-name cell auto"&gt;sdfkjhgds&lt;/span&gt;
  &lt;span class="actions button-group cell shrink"&gt;
    &lt;button class="fav button warning"&gt;fav&lt;/button&gt;
    &lt;button class="remove button alert"&gt;del&lt;/button&gt;
  &lt;/span&gt;
&lt;/li&gt;
{{< /code >}}

{{< code css >}}
/* hides any 'li' with a 'template' class */
li.template {
  display: none;
}
{{< /code >}}

{{< code js >}}
const list = document.querySelector('ul')
const templateItem = document.querySelector('li.template')

// Make a copy of the templateItem
// Pass 'true' as an argument to clone all children as well
const newItem = templateItem.cloneNode(true)

// remove 'template' class so it is no longer hidden
newItem.classList.remove('template')

list.appendChild(newItem)
{{< /code >}}

### Array methods

{{< code js >}}
const ary = [1, 2, 3, 4, 5]

ary.unshift(0)

console.log(ary)    // [0, 1, 2, 3, 4, 5]

ary.shift()         // 0

console.log(ary)    // [1, 2, 3, 4, 5]

ary.reverse()

console.log(ary)    // [5, 4, 3, 2, 1]
{{< /code >}}

## Presentations

* [Scale of Awesomeness](/awesomeness.pdf)

## Projects

Michael Bay-watch: [Morning](https://github.com/xtbc17s3/baywatch/tree/40397999bcca7d670908c4c048be3f55c042b4c1) | [Afternoon](https://github.com/xtbc17s3/baywatch/tree/3466cb3ee0120eafc4413eb8e496c66c861be4e1)

## Homework

* Finish implementing _move up_ and _move down_.

### Bonus Credit

* Allow users to edit the names of existing flicks. Wouldn't it be nice if we could make that `span`'s **content editable**?

### Super Mega Bonus Credit

* Persist the data using `localStorage`. When you refresh the page, your flicks should still be there.

### Super Mega Bonus Credit Hyper Fighting

* Allow users to filter the list of flicks.
