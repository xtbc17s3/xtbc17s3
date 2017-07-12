+++
date = "2017-07-12T11:27:08-04:00"
title = "Day 10: Routing in Noteherder"
toc = true
weight = 3
prev = "/week3/day9"

+++

<date>Wednesday, July 12, 2017</date>

## Lecture Videos

Morning: [Day 10, part 1](https://www.youtube.com/watch?v=1Q0aR_l_3ss&index=107&list=PLuT2TqJuwaY882Figl-Tr-VXWweaeS45B)

Afternoon: Coming soon...

## Topics

### React Router
* Redirect component
* Passing props to components in `Route`s

### Deployment of React app with routing
* github pages
* firebase

## Examples

### React Router

#### Redirect component

The `Redirect` component provided by React Router allows us to cause the user to navigate to a new location.  Just drop in a `Redirect` component and pass it a `to` prop to tell it where to redirect, and you're good to go!

{{< code jsx >}}
&lt;Redirect to='/widgets' /&gt;
{{< /code >}}

A common use case for a `Redirect` is to send a user to different locations based on whether they are logged in or not.

{{< code jsx >}}
import { Route, Redirect } from 'react-router'

&lt;Route exact path="/" render={() => (
  loggedIn ? (
    &lt;Redirect to="/dashboard"/&gt;
  ) : (
    &lt;PublicHomePage/&gt;
  )
)}/&gt;
{{< /code >}}

The code above translates to: "If the path is at the root route ('/'), redirect to '/dashboard' if the user is logged in.  Otherwise, render the PublicHomePage."

#### Passing props to routed components

Here's a basic `Route` that renders a `Stuff` component when the path matches `'/stuff'`:

{{< code jsx >}}
&lt;Route path="/stuff" component={Stuff} /&gt;
{{< /code >}}

When `Stuff` is rendered, it will automatically receive `props` from the router that include `match`, `history`, and `location`.  But what if we want to pass additional props to `Stuff`?  It turns out, we can use the `render` prop for `Route` instead, which allows us to pass additional props when the anonymous function returns some JSX.

{{< code jsx >}}
&lt;Route path="/stuff" render={(props) => (
  &lt;Stuff name="Bob" {...props} /&gt;
)} /&gt;
{{< /code >}}

In this example, the anonymous function in the `render` for the `Route` receives an argument of `props` (which includes `match`, `history`, and `location`).  Our `render` method returns some JSX - specifically, the `<Stuff />` component.  We pass along our existing `props` to `Stuff` via Object spread, as well as an additional `name` prop.

### Deployment of a React app with routing

Now that Noteherder has routing, deployment gets a bit more complicated.  Our app is a single-page application, so the server that our app is deployed to needs to always return `index.html` (the one page in our single-page app).  However, if we type `https://{yourdomainhere}/notes` in the address bar, the server will try to find a file called `notes.html` to send back (which it won't find, of course).  So how do we get it to always respond with `index.html` and let our client-side router handle the routing?  There are a few options:

1. Configure the server to always return `index.html`, no matter what is requested. (ideal solution)
2. Use `HashRouter` instead of `BrowserRouter`
3. Make a custom `404` page that is a copy of `index.html`.

#### Github Pages

Github Pages does not allow for server configuration, so option #1 is out.  We _can_, however, do options #2 or 3. For our in-class example, we chose to use `HashRouter` to solve this problem. Simply swap out `BrowserRouter` for `HashRouter` in `index.js` and re-deploy.  This will make your in-app routes appear after a `#` in the URL, which prevents the server from trying to respond with a different page.  For example, `https://myusername.github.io/noteherder/notes` becomes `https://myusername.github.io/noteherder/#/notes` when using `HashRouter`.

**index.js**

{{< code lang="jsx" line="3" >}}
import React from 'react'
import ReactDOM from 'react-dom'
import { HashRouter as Router, Route } from 'react-router-dom'


import './index.css'
import App from './App'
import registerServiceWorker from './registerServiceWorker'

ReactDOM.render(
  &lt;Router&gt;
    &lt;Route component={App} /&gt;
  &lt;/Router&gt;, 
  document.getElementById('root')
)
registerServiceWorker()
{{< /code >}} 

#### Firebase

Firebase _does_ allow for the server to be configured to always return `index.html`, so that is the option we chose during class.  To do so, we first need to install Firebase's CLI tools.

{{< term >}}
npm install -g firebase-tools
{{< /term >}}

Once the Firebase tools are installed, we can use them to login, initialize a firebase project, and deploy.  (Note, make sure you have made a production build with `npm run build` before deploying)

{{< term >}}
firebase login
firebase init
firebase deploy
{{< /term >}}

{{% aside info "Firebase Init" %}}
`firebase init` will prompt you to answer a bunch of questions about how you want to configure the app you are deploying.  For more information about how to answer those questions, check out the `create-react-app` README [here](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md#firebase)
{{% /aside %}}

## Projects

Noteherder: [Morning](https://github.com/xtbc17s3/noteherder/tree/5f0ff6fab2950f638672fdffba0fb726a6d0794a) | [Afternoon](https://github.com/xtbc17s3/noteherder/tree/3bfcc65f56dd8291c1793fd126fdc1cb62642e85)

## Homework

* Add an `updatedAt` field to notes (updating its value every time you save the note).

### Super Mega Bonus Credit

* Sort the notes in the list with the most recently updated at the top.
