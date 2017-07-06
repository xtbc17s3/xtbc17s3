+++
date = "2017-07-06T10:17:36-04:00"
title = "Day 7: Props and Destructuring"
prev = "week2/day6/"
toc = true
weight = 3

+++

<date>Thursday, July 6, 2017</date>

## Lecture Videos

Morning: [Day 7, part 1](https://www.youtube.com/watch?v=GZirEkQ9Mp0&list=PLuT2TqJuwaY882Figl-Tr-VXWweaeS45B&index=77)

Afternoon: [Day 7, part 1](https://www.youtube.com/watch?v=xc0dCoiMP3s&list=PLuT2TqJuwaY_yOPNQJLn2Ya_hfes8g2fv&index=84)

## Topics

### [React](https://facebook.github.io/react/)

* Methods as props
* [Controlled](https://facebook.github.io/react/docs/forms.html) vs [Uncontrolled](https://facebook.github.io/react/docs/uncontrolled-components.html) Forms
* [Controlled and uncontrolled form inputs in React don't have to be complicated](https://goshakkk.name/controlled-vs-uncontrolled-inputs-react/)
* Component lifecycle methods ([Docs](https://facebook.github.io/react/docs/react-component.html))
  * [`componentDidMount()`](https://facebook.github.io/react/docs/react-component.html#componentdidmount)


### JavaScript

* Destructuring assignment
* Spread operator
* Property initializers (and arrow functions)

### Firebase [↓](#firebase)

* Getting started
* Re-base for syncing React state with Firebase

## Examples

### React

#### Methods as props

Sometimes one component needs to update another component's state. It can't do that directly, but it can call a method from that other component if it's available via a prop.

[**Try this example live on CodePen**](https://codepen.io/dstrus/pen/bWzWew?editors=1010)

{{< code lang="jsx" line="5,23" class="line-numbers" >}}
import React from 'react'
import ReactDOM from 'react-dom'

const PartyButton = ({ celebrate, celebrations }) =&gt; {
  return &lt;button onClick={celebrate}&gt;Party! {celebrations}&lt;/button&gt;
}

class App extends React.Component {
  constructor() {
    super()
    this.state = {
      celebrations: 0,
    }
    this.celebrate = this.celebrate.bind(this)
  }

  celebrate() {
    const celebrations = this.state.celebrations + 1
    this.setState({ celebrations })
  }

  render() {
    return &lt;PartyButton celebrate={this.celebrate} celebrations={this.state.celebrations} /&gt;
  }
}

ReactDOM.render(&lt;App /&gt;, document.querySelector('main'))
{{< /code >}}

#### Component lifecycle methods

**`componentDidMount()`** is invoked immediately after a component is mounted. Initialization that requires DOM nodes should go here.

{{< code jsx >}}
import React, { Component } from 'react'

class MyComponent extends Component {
  componentDidMount() {
    this.nameInput.focus()
  }

  render() {
    return (
      &lt;input 
        ref={(input) =&gt; { this.nameInput = input; }} 
        defaultValue="will focus"
      /&gt;
    )
  }
}
{{< /code >}}

### JavaScript (ES6+)

#### Destructuring assignment

[Destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) syntax is a JavaScript expression that makes it possible to unpack values from arrays, or properties from objects, into distinct variables.

{{< code js >}}
const myObj = {
  a: true,
  b: 'Destructuring!'
}

let { a, b } = myObj

console.log(a) // => true
console.log(b) // => 'Destructuring!'
{{< /code >}}

#### Spread operator

The [*spread operator*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator) was added in ES6 to allow an expression to be expanded in places where multiple arguments (for function calls) or multiple elements (for array literals) or multiple variables (for destructuring assignment) are expected.

{{< code js >}}
function myFunc (x, y, z) {
  console.log(x)
  console.log(y)
  console.log(z)
}
const args = [1, 2, 3]

myFunc(...args) // the spread '...args' applies the items in args to the three arguments in myFunc
// => 1
// => 2
// => 3
{{< /code >}}

It is also an easy way to make copies of iterable objects

{{< code js >}}
const ary = [1, 2, 3]
const aryCopy = [...ary] // makes a copy of ary
{{< /code >}}

If you are in a project using Babel (like a React project created with `create-react-app`), you can also use the `object-rest-spread-transform` to apply this same method to objects.

{{< code js >}}
this.state = {'a': true, party: 'hard'}
const stateCopy = {...this.state} // makes a copy of this.state
{{< /code >}}

#### Property initializers

From the proposal:

> "Class instance fields" describe properties intended to exist on instances of a class (and may optionally include initializer expressions for said properties).

We can take advantage of this in React.

[**Read more: Using ES7 property initializers in React**](https://babeljs.io/blog/2015/06/07/react-on-es6-plus)

We can use a property initializer to set the initial value of state without writing a constructor:

{{< code jsx >}}
class Song extends React.Component {
  state = {
    versesRemaining: 5,
  }
}
{{< /code >}}

We can even set default props and use those in the initial state:

{{< code jsx >}}
class Song extends React.Component {
  static defaultProps = {
    autoPlay: false,
    verseCount: 10,
  }
  state = {
    versesRemaining: this.props.verseCount,
  }
}
{{< /code >}}

{{% aside danger "Subject to  minor changes" %}}
[Property initializers](https://github.com/tc39/proposal-class-public-fields) are a [Stage 2 proposal](https://tc39.github.io/process-document/) for ECMAScript, meaning that it's still a _draft_ and is subject to minor changes before becoming standardized. Facebook itself is already using these techniques in production, however.
{{% /aside %}}

#### Property initializers + arrow functions

Combining property initializers and arrow functions gives us a convenient way to auto-bind `this`, since arrow functions inherit `this` from the scope in which they are declared (lexical scoping):

{{< code jsx >}}
class Something extends React.Component {
  handleButtonClick = (ev) => {
    // `this` is bound correctly!
    this.setState({ buttonWasClicked: true });
  }
}
{{< /code >}}

### Firebase

#### Getting Started

[Firebase](https://firebase.google.com/) is a real-time database hosted by Google.  In addition to the database, it also provides features of authentication, analytics, cloud storage, and hosting.  For _Noteherder_, we synced the `state` of our app to our database on Firebase.  This allowed all of our data to be persisted, even after page refreshes.

[Re-base](https://github.com/tylermcginnis/re-base) is an open source package that allows easy syncing of local state with a Firebase database. Add rebase to your project with one of the following commands:

{{< term >}}
yarn add re-base               # add package using yarn
npm install re-base            # add package using npm
{{< /term >}}

Once you have re-base installed, setup is easy!  First, create a new project on Firebase, then click on "Add to a web app" to see your JavaScript config object.  Next, initialize a Firebase app and database in your project using the config object, and provide the database to re-base.

{{< code js >}}
import Rebase from 're-base'
import firebase from 'firebase/app'
import database from 'firebase/database'

const app = firebase.initializeApp({
  apiKey: "YOURAPIKEY",
  authDomain: "YOURAUTHDOMAIN",
  databaseURL: "YOURDATABASEURL",
  projectId: "YOURPROJECTID",
  storageBucket: "YOURSTORAGEBUCKET",
  messagingSenderId: "YOURSENDERID"
})

const db = database(app)
const base = Rebase.createClass(db)

export default base
{{< /code >}}

Finally, call `base.syncState` to sync your app's local state with Firebase.  The first argument to `syncState` is the name of the Firebase endpoint you want to sync, and the second is a configuration object.

{{< code js >}}
base.syncState('myFavoriteEndpoint', {
  context: this,
  state: 'items'
})
{{< /code >}}

Now, any time we update the state of our app, the changes will sync with Firebase in real time.

{{% aside info "More Re-base Options" %}}
Re-base can do much more than just syncing state.  There are methods for `fetch`, `push`, `post`, etc.  To find out more about what all you can do with re-base, check out the [README](https://github.com/tylermcginnis/re-base#re-base)
{{% /aside %}}

## Projects

* Noteherder [morning](https://github.com/xtbc17s3/noteherder/tree/e67f31206646aa16a64c7c6fae982d620cf64fc6) | [afternoon](https://github.com/xtbc17s3/noteherder/tree/60c7f78c25f54c47a3a7ce6d87560779507f2efe)

## Homework

### Practice!  

* Finish any levels of homework for the week that are not yet complete.  
* Re-watch any lecture videos about concepts you are not 100% clear on.  
* Try out the React tutorial at [https://reacttraining.com/](https://reacttraining.com/).