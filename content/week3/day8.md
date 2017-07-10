+++
date = "2017-07-10T09:16:42-04:00"
title = "Day 8: Firebase Authentication"
toc = true
weight = 1
prev = "week3"

+++

<date>Monday, July 10, 2017</date>

## Lecture Videos

Morning: [Day 8, part 1](https://www.youtube.com/watch?v=FE55eeOqMrs&list=PLuT2TqJuwaY882Figl-Tr-VXWweaeS45B&index=87)

Afternoon: [Day 8, part 1](https://www.youtube.com/watch?v=C33IfJfRhU8&index=95&list=PLuT2TqJuwaY_yOPNQJLn2Ya_hfes8g2fv)

## Topics

### Firebase Authentication

* 'firebase/auth'
* `authWithPopup`
* signing in and out
* handling auth state changes
* database rules

## Examples

### Authentication

Firebase isn't just a real-time database.  It can also provide authentication services via email/password, phone, or common third-party services like Github, Facebook, and Google. For _Noteherder_, we set up authentication via Google.

#### Step 1: Enable Google authentication in Firebase

Go to your Firebase console and click on the "Authentication" tab in the "Develop" sidebar, then click on "Sign-in method".  You'll see a list of the authentication methods allowed by Firebase.  Click on "Google" and then enable the toggle switch.

<div class="img firebase-enable-google"><span>Click the switch to enable.  Whew!  That was difficult.</span></div>

#### Step 2: Add Firebase auth to your app

_Note: This step assumes you already have your Firebase database added to your app._ 

Import `firebase/auth` into your app's firebase setup.  Enable firebase auth and also create an instance of `GoogleAuthProvider`.

**base.js**

{{< code lang="js" line="4,17,18" class="line-numbers" >}}
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

export default Rebase.createClass(db)
{{< /code >}}

#### Step 3: Set up the SignIn Component

Import `auth` and the `googleProvider` into whatever component handles the sign-in process.  Call `signInWithPopup` on the `auth` object, passing the provider as a parameter.  This will launch a popup screen that will prompt the user to sign in using the provider you have specified.

**SignIn.js**

{{< code jsx >}}
import React from 'react'
import { auth, googleProvider } from './base'

const SignIn = () => {
  const authenticate = (provider) => {
    auth.signInWithPopup(provider)
  }

  return (
    &lt;button className="SignIn" onClick={() => authenticate(googleProvider)}&gt;
      Sign In With Google
    &lt;/button&gt;
  )
}

export default SignIn
{{< /code >}}

#### Step 4: Handling auth state changes (and page refreshes)

Once the user has authenticated via the popup, the state of our authorization has changed (we now have an authenticated user).  Other events that can cause auth state changes are signing out, timeouts, and page refreshes.  We should probably set up something to listen for these events.  In the `componentWillMount` lifecycle hook that runs when the Component is first getting loaded, we can call the `onAuthStateChanged` method provided on the global `auth` object to set up such a listener.

**App.js**

{{< code js >}}
// ...

componentWillMount() {
  auth.onAuthStateChanged(
    (user) => {
      if (user) {
        // finish signing in
        this.authHandler(user)
      } else {
        // finished signing out
        this.setState({ uid: null })
      }
    }
  )
}

// ...
{{< /code >}}

#### Step 5: Finishing sign-in

What the `authHandler` callback does is up to you, but for _Noteherder_, we had it do pretty typical things - save the user ID to state, and initialize syncing our local state for 'notes' with the data stored on Firebase.

**App.js**

{{< code js >}}
// ...

authHandler = (user) => {
  this.setState(
    { uid: user.uid },
    this.syncNotes
  )
}

// ...
{{< /code >}}

#### Step 6: Signing out

Signing out when using Firebase for authentication is also simple - just call `auth.signOut()`!  Once the promise returned by `signOut` has resolved, you can handle any additional cleanup.  In _Noteherder_, we stop syncing with Firebase and set `state.notes` back to an empty object.

**App.js**

{{< code js >}}
// ...

signOut = () => {
  auth
    .signOut()
    .then(
      () => {
        // stop syncing with Firebase
        base.removeBinding(this.ref)
        this.setState({ notes: {} })
      }
    )
}

// ...
{{< /code >}}

### Rules

For your Firebase database, you can set up rules (written in JSON) that specify the conditions under which data is allowed to be read or written.  By default, a newly generated project will require that a user be authenticated to read or write _any_ data.

{{< code js >}}
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}
{{< /code >}}

If you do not have authentication set up yet, these values can be set to `true`.  This allows _anyone_ to read or write any data in the database.  This can be convenient, but probably not a good idea long-term (and you _will_ get a warning if you do that).

Additional rules can be applied per endpoint:

{{< code js >}}
{
  "rules": {
    "emails": {
      ".read": true,
      ".write": "auth != null"
    },
    "texts": {
      ".read": true,
      ".write": "auth != null"
    },
    "users": {
      "$userId": {
        ".read": "auth != null && auth.uid == $userId",
        ".write": "auth != null && auth.uid == $userId"
      }
    }
  }
}
{{< /code >}}

The above rules translate to:

* texts and emails can be read by anyone, but only written by authenticated users
* users data can be read and written only by an authenticated user whose `uid` matches the `$userId` of that item

## Projects

Noteherder: [Morning](https://github.com/xtbc17s3/noteherder/tree/7a281036a303ed63a69163d938ad9db7b257ad39) | [Afternoon]()

## Homework

Add another authentication method through Firebase (like Github, Facebook, or Twitter)

