+++
date = "2017-05-30T09:40:47-04:00"
title = "Day 9: Routing and Fetching"
toc = true
weight = 2
prev = "/week3/day8"

+++

<date>Tuesday, July 11, 2017</date>

## Lecture Videos

Morning:

* [Day 9, part 1](https://www.youtube.com/watch?v=TvDN8nil0Iw&index=100&list=PLuT2TqJuwaY882Figl-Tr-VXWweaeS45B)

Afternoon:

* [Day 9, part 1](https://www.youtube.com/watch?v=gTbojf2H55E&index=107&list=PLuT2TqJuwaY_yOPNQJLn2Ya_hfes8g2fv)

## Topics

### Firebase Authentication

* Signing in with GitHub

### Routing

* React Router v4
  * `<Router />`
  * Links and NavLinks
  * Routes
  * `history`

### Fetching Data

* The Fetch API
* Promises
* Parsing the response

## Examples

### Signing in with GitHub

#### Step 1: Get your authorization callback URL from Firebase

Navigate to your project in Firebase console.  Click on the 'Authenticate' tab on the left and then on the GitHub logo.  Copy the authorization callback URL.

{{% aside info "Copying the callback URL" %}}
Use the _copy to clipboard_ button in the Firebase console. If you try to manually highlight the callback URL, you are likely to accidentally select some hidden elements, and your authentication will not work.
{{% /aside %}}

<div class="img firebase-enable-github"><span>The data for the Client ID and Client Secret will be generated in the next step.</span></div>

#### Step 2: Register your app in Github

Log in to GitHub and click on 'Settings'.  On the left hand side, click on 'OAuth Applications' under the 'Developer settings' menu.  Register a new app and fill out the form.

<div class="img github-oauth-new-registration"><span>Use the Authorization callback URL from step 1</span></div>

After successfully registering the app, you'll be taken to your new app's settings page.

<div class="img github-oauth-secrets"><span>Seeeeecrets...  (Don't worry, this app has been deleted. Never post your app secrets publicly.)</span></div>

#### Step 3: Enable Github authentication in Firebase

Go back to the GitHub authentication tab in Firebase and fill in the Client ID and Client Secret that you got from registering your app with GitHub.

<div class="img firebase-github-secrets"><span>More Seeeeecrets...  (But seriously, don't share your secrets)</span></div>

#### Step 4: Add the GitHub auth provider to your app

_Note: This step assumes you already have Firebase auth configured in your app._ 

Create and export an instance of `GithubAuthProvider`.

**base.js**

{{< code lang="js" line="19" >}}
import Rebase from 're-base'
import firebase from 'firebase/app'
import database from 'firebase/database'
import 'firebase/auth'

const app = firebase.initializeApp({
  apiKey: "YOURAPIKEY",
  authDomain: "YOURAUTHDOMAIN",
  databaseURL: "YOURDATABASEURL",
  projectId: "YOURPROJECTID",
  storageBucket: "YOURSTORAGEBUCKET",
  messagingSenderId: "YOURSENDERID"
})

const db = database(app)

export const auth = app.auth()
export const googleProvider = new firebase.auth.GoogleAuthProvider()
export const githubProvider = new firebase.auth.GithubAuthProvider()

export default Rebase.createClass(db)
{{< /code >}}

#### Step 5: Set up the SignIn Component

Add the `githubProvider` to the imports in your `SignIn` component. Call `signInWithPopup` on the `auth` object, passing the provider as a parameter.  This will launch a popup screen that will prompt the user to sign in using the specified provider.

**SignIn.js**

{{< code lang="jsx" line="2,5,6,7,15,16,17" >}}
import React from 'react'
import { auth, googleProvider, githubProvider } from './base'

const SignIn = () => {
  const authenticate = (provider) => {
    auth.signInWithPopup(provider)
  }

  return (
    &lt;div className="SignIn"&gt;
      &lt;button onClick={() => authenticate(googleProvider)}&gt;
        Sign In With Google
      &lt;/button&gt;

      &lt;button onClick={() => authenticate(githubProvider)}&gt;
        Sign In With GitHub
      &lt;/button&gt;
    &lt;/div&gt;
  )
}

export default SignIn
{{< /code >}}

### Routing

[React Router](https://github.com/ReactTraining/react-router) provides a routing solution that allows us to change what UI we render based on the current URL.  The router is a _Higher Order Component_ that wraps a React app and allows us to navigate without additional requests and responses to and from the server.

#### Router Setup

Setting up React Router is easy.  For web projects, install `react-router-dom`

{{< term >}}
yarn add react-router-dom  # install react router with yarn
{{< /term >}}

or

{{< term >}}
npm install --save react-router-dom  # install react router with npm
{{< /term >}}

Then, in your `ReactDOM.render` call, attach the Router as your base element, wrapping the root-level `<App />` component.  The whole app is now contained within the Router component, so we can take advantage of it anywhere.

**index.js**

{{< code jsx >}}
import React from 'react'
import ReactDOM from 'react-dom'
import { BrowserRouter as Router } from 'react-router-dom'
import App from './App'

ReactDOM.render(
  &lt;Router&gt;
    &lt;App /&gt;
  &lt;/Router&gt;,
  document.getElementById('root')
)
{{< /code >}}

#### Routes

The core of React Router is the `<Route />` component.  It allows you to specify what UI to render when a particular URL is matched.  For instance, if we wanted to render a `<Users />` component when we matched a `/users` URL, we could make the following Route:

{{< code jsx >}}
&lt;Route path='/users' component={Users} /&gt;
{{< /code >}}

If you don't want to render a whole component, a Route can alternatively accept a `render` prop, which accepts a function that returns JSX:

{{< code jsx >}}
&lt;Route path='/users' render={() => &lt;h1&gt;Users Path!&lt;/h1&gt;} /&gt;
{{< /code >}}

One important thing to keep in mind is that if we define a Route's path as `/users`, that will match both `/users` _and_ `/users/123`, because both begin with `/users`.  If we want the Route to match only when the path is _exactly_ `/users`, we can add the prop `exact` to our Route component.

{{< code jsx >}}
&lt;Route exact path='/users' component={Users} /&gt;
{{< /code >}}

#### Links

React Router also provides `<Link />` and `<NavLink />` components to make it easy to generate links to Routes. If we want to generate a Link that goes to `/about`, we can do the following:

{{< code jsx >}}
&lt;Link to='/about'&gt;About&lt;/Link&gt;
{{< /code >}}

NavLinks are similar, but provide some additional functionality.  The main difference is that they will add an `activeClassName` to the rendered link if the current URL matches the `to` property of the NavLink.  This allows active links to be styled differently than inactive links.

{{< code jsx >}}
&lt;NavLink to='/'&gt;Home&lt;/NavLink&gt;    // rendered link tag will have '.active' class when URL is '/'
{{< /code >}}

#### History

The `history` object is maintained and updated by the Router to keep track of where the user has navigated within the app.  It is passed to every component contained within the Router as part of the component's `props`.  It has a variety of helpful properties and methods that provide information and navigation. Here are just a few:

{{< code js >}}
history.length             // number of history entries
history.location           // provides the current location
history.push(path)         // navigates to a new path
history.go(n)              // navigates n steps through history stack
history.goBack()           // go back one step (history.go(-1))
history.goForward()        // go forward one step (history.go(1))
history.block(prompt)      // block navigation
{{< /code >}}

### Fetch

> The Fetch API provides a JavaScript interface for accessing and manipulating parts of the HTTP pipeline, such as requests and responses.  It also provides a global `fetch()` method that provides an easy, logical way to fetch resources asynchronously across the network.

[_MDN - Using Fetch_](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)

If we need to get data from a remote server (or send some to one), there are several ways to do it.  In vanilla JS, there is `XMLHttpRequest`, jQuery provides `$.ajax`, and there are a variety of other packages and libraries that provide their own version.  Luckily, there is a new kid in vanilla JS town - the _Fetch API_.

`fetch()` is a globally available, easy to use way to asynchronously send and receive data.  The simplest usage of fetch is to simply provide it with the URL of the request, and it will perform a `GET` request by default.  The `fetch()` function returns a promise that resolves when the data is received.  Once it is received, we can process and use the data with functions provided to the promise's `.then()` callbacks.

{{< code js >}}
fetch('https://api.mywebsite.com/users')    // fetch users data from 'mywebsite' api
  .then(response => response.json())        // parse the response json into JavaScript object(s)
  .then(users => console.log(users))        // log the parsed users to the console
  .catch(error => console.warn(error))      // if any errors occur, log them to the console
{{< /code >}}

{{% aside info "Fetch does more than just fetch" %}}
If no second argument is provided to `fetch()`, it defaults to a standard `GET` request.  However, the second argument can be a configuration object, allowing it to use different HTTP methods, set Headers, include Credentials, etc.  To find out more, check out [the docs](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
{{% /aside %}}

## Projects

* Noteherder [morning](https://github.com/xtbc17s3/noteherder/tree/065eaa988e6aea91e094fada7d87f83c9c1cb32c) | [afternoon](https://github.com/xtbc17s3/noteherder/tree/afternoon)
* API Party [morning](https://github.com/xtbc17s3/api-party/tree/6383ffdf020958cad75c7562282aceb17ac03de0) | [afternoon](https://github.com/xtbc17s3/api-party/tree/c3c4e22cde4eb22dc1d79ea5c57a0a970d0a0ab0)

## Homework

Extend the API-Party app by adding at least one additional route that gets data from a public API.

* A link to the route should appear in the header along with the 'Github' and 'NASA' links
* When the link is clicked, it should be styled to show that it is active
* The new component should fetch data from a public API
* Some interesting data from the API should be presented
* The data should look pretty (style it with CSS)

### Bonus Credit

* Accept user input to refine the data you request from the API

### Super Mega Bonus Credit

* Add additional routes and APIs

### Super Mega Bonus Credit Hyper Fighting

* Figure out something interesting to do with the data on your own.  Make a graph, render a map, add child routes, go nuts!
