+++
date = "2017-05-15T11:13:46-04:00"
title = "Day 6: React"
prev="/week2/day5/"
toc = true
weight = 2

+++

<date>Wednesday, July 5, 2017</date>

## Lecture Videos

Pending

## Topics

### [React](https://facebook.github.io/react/)

* Using `map` with components
* Stateless Functional Components
  * [Functional Stateless Components in React](http://javascriptplayground.com/blog/2017/03/functional-stateless-components-react/)
  * [Functional Components vs. Stateless Functional Components vs. Stateless Components](https://tylermcginnis.com/functional-components-vs-stateless-functional-components-vs-stateless-components/)
  * [Nine Wins You Might Have Overlooked](https://hackernoon.com/react-stateless-functional-components-nine-wins-you-might-have-overlooked-997b0d933dbc)

### JavaScript

* Named and default exports
* Named and default imports

### Package Managers

* [NPM](https://www.npmjs.com/) - Node Package Manager
* [Yarn](https://code.facebook.com/posts/1840075619545360) - Facebook's version of npm

## Examples

### React

#### Using `map` with Components

**[See this example live on CodePen →](https://codepen.io/dstrus/pen/gWZVOQ?editors=0010#0)**

**Person.js**

{{< code jsx >}}
class Person extends React.Component {
  render() {
    return (
      &lt;li&gt;Hello, {this.props.person.name}!&lt;/li&gt;
    )
  }
}

export default Person
{{< /code >}}

**PersonList.js**

{{< code jsx >}}
import Person from './Person'

class PersonList extends React.Component {
  render() {
    const people = [
      { name: 'Seth', hair: 'blonde' },
      { name: 'Nichole', hair: 'long' },
      { name: 'Davey', hair: 'long gone' }
    ]
    return (
      &lt;div&gt;
        &lt;h2&gt;People&lt;/h2&gt;
        {
          people.map((person => &lt;Person person={person} /&gt;))
        }
      &lt;/div&gt;
    )
  }
}

export default PersonList
{{< /code >}}

#### Stateless Functional Components

Not every React Component needs to have state.  Many simply render a bit of `props` and UI.  For such components, we don't need to instantiate a whole class that inherits from `React.Component`, we can simply write a function that accepts `props` as an argument and returns the markup we need.

For instance, in the previous example, the `Person` component can easily be re-written as a Stateless Functional Component.

{{< code jsx >}}
function Person (props) {
  return (
    &lt;li&gt;Hello, {props.person.name}!&lt;/li&gt;
  )
}

// Or...

const Person = (props) => &lt;li&gt;Hello, {props.person.name}!&lt;/li&gt;
{{< /code >}}

### JavaScript (ES6+)

#### Named and default exports and imports

Prior to ES6, there were many competing ways to export and import JavaScript modules.  The most common were [CommonJS](https://webpack.github.io/docs/commonjs.html) and [AMD](http://requirejs.org/docs/whyamd.html).  Luckily ES6 defined a specification for standardizing module export and import.

There are two types of exports from any JS file - *named* and *default*.  The important thing to remember is that there can only be *one* default export per module, but there can be as many named exports as you want.

**myModule.js**
{{< code js >}}
export const myNumber = 8

export function sayHi () {
  console.log('hello')
}

export default class MyClass {
  add (a, b) {
    return a + b
  }
}
{{< /code >}}

The main difference is how they are imported.  Default exports get the most concise syntax:

{{< code js >}}
import MyClass from 'myModule'

const classInstance = new MyClass()
classInstance.add(1, 2) // => 3
{{< /code >}}

{{% aside info "Default import naming" %}}
Since there can be only one default export per module, the name by which you import the default export is not important - you can name it whatever you want.  For instance, instead of importing as `MyClass`, we could have said `import LuftBallons from 'myModule'`, and it would have worked just fine.  To read more about default and named exports, click [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export).
{{% /aside %}}

Named exports get a slightly more verbose syntax for importing, and the names _are_ important (otherwise it can't determine what you want to import).

{{< code js >}}
import { myNumber, sayHi } from 'myModule'

console.log(myNumber) // => 8

sayHi() // => 'hello'
{{< /code >}}

If you need to import a named export under a different name&mdash;if, for example, you have another import or local variable with the same name&mdash;you can specifiy a different name using _as_.

{{< code js >}}
import { myNumber as num, sayHi as yo } from 'myModule'

console.log(num) // => 8

yo() // => 'hello'
{{< /code >}}

You can also combine default and named imports in the same line.

{{< code js >}}
import MyClass, { myNumber, sayHi } from 'myModule'
{{< /code >}}

### Package Managers

#### Node Package Manager (npm)

Node Package Manager hosts almost half a million packages of free, reusable JavaScript code and is the largest software registry in the world. It allows you to easily add any module to your project, and it will install the requested package, as well as any required dependencies of that package.

{{< term >}}
npm install react
{{< /term >}}

#### Yarn

[Yarn](https://code.facebook.com/posts/1840075619545360) is Facebook's version of npm, designed to improve performance and resolve several important issues.  The key differences are:

* Deterministic installation - packages will always install in the same order on every machine
* `yarn.lock` - this lockfile locks dependency versions for consistency and security
* Local cache of downloaded packages - faster and can still work with no internet connection after initial installation
* Parallel installation - Dependency installation can happen in parallel, greatly increasing speed

To install yarn (npm was already installed as part of setup instructions), type the following command (Mac):

{{< term >}}
brew install yarn
{{< /term >}}

Or on Windows, [download the installer](https://yarnpkg.com/latest.msi).

Once installed, you can use yarn with following commands:

{{< term >}}
yarn
# installs all packages and dependencies listed in your project's package.json

yarn add {package_name}
# installs a new package and adds it to package.json

yarn start
# starts your local development web server (in project from create-react-app)
{{< /term >}}

## Projects

* Noteherder [morning](https://github.com/xtbc17s3/noteherder/tree/26ce21771d9ebc84a4db4ca83e330f20748ca10a)

## Day 7 Homework

* Load data in the form when a note is clicked in the list.

### Bonus Credit

* Add a note to the list when the form is submitted.

### Super Mega Bonus Credit

* Make the _delete_ button work.

### Super Mega Bonus Credit Hyper Fighting

* Make the "+" button in the sidebar clear out the form so a new note can be added.

### Links that may help you

* [Controlled](https://facebook.github.io/react/docs/forms.html) vs [Uncontrolled](https://facebook.github.io/react/docs/uncontrolled-components.html) Forms
* [Controlled and uncontrolled form inputs in React don't have to be complicated](https://goshakkk.name/controlled-vs-uncontrolled-inputs-react/)
