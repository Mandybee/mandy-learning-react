# Learning React!

This little project is me following along in a Udemy class: [Available here](https://www.udemy.com/course/react-for-the-rest-of-us/). I'm going to (hopefully) keep a list of notes here about things I learn along the way!

## Getting Started...

The intro to React was given as a little crash course using CodePen.io. [Mandy's React Codepen](https://codepen.io/mandybee/pen/zYqMeEX)

After playing around with syntax and such for a bit, we moved on to create our own app locally.

## Setting up a local environment

I'm using Visual Studio Code to build this. It has built-in support for JSX, woo!

To make my life easier, the instructor suggests adding this to the settings.json for VSC to make all js files open using the "javascript react" formatting:

```json
"files.associations": {
    "*.js": "javascriptreact"
}
```

### Node.js, NPM

To get our React project started, we run the following to initialize npm (which sets up the package.json):

    npm init -y

### Install React

And then we install the React library:

    npm install react react-dom

### Install Webpack

And we need webpack to bundle up our JS w/dependencies, transpile JSX for browsers, and also allow us to view our app as we work.

    npm install webpack webpack-cli webpack-dev-server

Create new file in project: webpack.config.js

[Get the webpack config code for this](https://github.com/LearnWebCode/react-course)

_There's also a course on workflow tasks and such, so he does not quite explain most of this in detail._

And last but not least...

    npm install @babel/core @babel/preset-env @babel/preset-react babel-loader

Add a command to the package.json in "scripts" to set up our server:

```json
"dev": "webpack-dev-server"
```

To run our task:

    npm run dev

Our site is now available at: [localhost:3000](http://localhost:3000) And it'll refresh the page when we make edits to our site!

To have our browser only load JS changes (rather than refreshing page), add this to the end of our site's JS:

```javascript
if (module.hot) {
  module.hot.accept();
}
```

### Create HTML, CSS, and main JS Files

Once React is installed and we have all of our packages, we can get started! The first thing the instructor tells us to do is create an "app" folder. In this folder, we'll put all of our site files (aside from the config stuff we created).

For our project, we'll need one html document (index.html). For this project, we'll pull in Public Sans (google font), Bootstrap (a CSS framework), Fontawesome icons, and then our own CSS and JS files. Aside from these files, the html document will only have a <div> with an ID on the page, where our React component will be built.

Along with our html document and css document, we need to create a JS document (we called ours "Main.js").

To include our Javascript, we link to "/bundled.js", which is hidden in our local files, but is created by webpack and is available for our app on the web server.

## Setting Up Our JS

Our Main.js file needs to start with the following:

```javascript
import React from "react";
import ReactDOM from "react-dom";
```

Then we'll create a function for our main HTML output:

```jsx
function Main() {
  return (
    <>
      <h1>This is my site!</h1>
    </>
  );
}
```

And render that onto the page with:

```javascript
ReactDOM.render(<Main />, document.querySelector("#app"));
```

Woot! All done. lol Okay... just kidding.

## Create a "components" folder

Instead of jamming all of our code into one file, we need to separate our JS into small pieces. At the time of writing this (a bit after having set up our site), I have the following files in my components folder: _About.js, Container.js, Footer.js, Header.js, HomeGuest.js, Terms.js_

Each new component JS file needs to start with:

```javascript
import React from "react";
```

Then contains the function with all of that component's JS. And then it should end with:

```javascript
export default functionName;
```

Back in our Main.js, we have to import our components to use them:

```javascript
// Components
import Header from "./components/Header";
import HomeGuest from "./components/HomeGuest";
import Footer from "./components/Footer";
import About from "./components/About";
import Terms from "./components/Terms";
```

Then we can include them in our Main() function like:

```jsx
<Header />
<HomeGuest />
<Footer />
```

## Creating a Snippet in Visual Studio Code

Code > Preferences > User Snippets

Type in "React" to find 'javascriptreact' and it'll create a JSON file to edit.

From the repo, I grabbed some code to paste in there to create a new [React component snippet](https://github.com/LearnWebCode/react-course/blob/master/vscode-react-component.txt).

You can easily create your own snippets using this app: [snippet generator](https://snippet-generator.app) It can generate Visual Studio Code, Sublime Text, and Atom snippets!

## Routing / Client-side Rendering

Okay, I thought this was totally neato. (Yep, we're back in the 90's.) I remember a developer I worked with many years ago set up a site that did this -- one HTML page, many URLs, different content. It seemed like total magic. Honestly, it still does, but I don't necessarily have to know how to write all of this stuff from scratch in order to use it, right??

So, first, we'll need react-router-dom.

    npm install react-router-dom

### Set Up Main() For Routing

After importing React and ReactDOM, import these:

```javascript
import { BrowserRouter, Switch, Route } from "react-router-dom";
```

In Main()'s return, we're going to overhaul our JSX.

```jsx
function Main() {
  return (
    <BrowserRouter>
      <Header />
      <Switch>
        <Route path="/" exact>
          <HomeGuest />
        </Route>
        <Route path="/about-us" exact>
          <About />
        </Route>
        <Route path="/terms" exact>
          <Terms />
        </Route>
      </Switch>
      <Footer />
    </BrowserRouter>
  );
}
```

<BrowserRouter> wraps around our whole layout.

<Header> and <Footer> will be on all pages, so they're children of BrowserRouter.

The <Switch> element handles routing for a section.

Each <Route> within a <Switch> is a new 'page' that will be served up based on the URL in the browser window (or links selected within the app). Between the <Route> tags, we can include content to show on the page.

_This was already in the webpack config code taken from GitHub, but I think it's important to note that this line in the webpack.config.js makes sure that we can, in fact, visit "localhost:3000/about-us" and have it work. It forces the browser to head to index.html rather than trying to find the index file in an "about-us" folder._

```javascript
historyApiFallback: {
  index: "index.html";
}
```

### Changing Links

In our Footer.js, we had some links written in regular old HTML. To set up routing, we went in and changed all of the <a> tags to <Link> tags and all of the "href" attributes to "to" attributes. Our "to" attributes are pointed to "/", "/about-us", and "/terms".

At the top of our Footer.js, we included:

```javascript
import { Link } from "react-router-dom";
```

We did similar stuff to our Header.js, with its one link.

#### Updating Page Titles

Very simply, you could add the following in your component's function:

```javascript
useEffect(() => {
  document.title = "Terms & Conditions | ComplexApp";
  window.scrollTo(0, 0);
}, []);
```

And add ", {useEffect}" into the "import React..." line at the top.

BUT! This creates a function that we'd just be repeating over and over again on every page we create, so it's not the best way to do it...

See "Composition" section for the better way to do this.

## JSX Syntax Changes from HTML

- The "class" attribute is replaced with "className".
- The "for" attribute is replace with "htmlFor".

## Reusable and flexible containers

We created a Container.js file to hold the following code:

```jsx
import React, { useEffect } from "react";

function Container(props) {
  return (
    <>
      <div className={"container py-md-5 " + (props.wide ? "" : "container--narrow")}>{props.children}</div>
    </>
  );
}

export default Container;
```

We use the "props" variable to pass through variables from our components where <Container> is used. Most of our pages will use the "container--narrow" class for the containing div, so we've added the conditional to our Container.js to show "container--narrow" unless the "wide" property is set to true.

On our HomeGuest.js, we've added "wide={true}" to the Container tag. Voila!

## Composition

Because many pages have similar content (such as needing to set the page title), we'll create a new component (Page.js), that we'll use in our page components. This will import the "Container" and create a new Page component that we can leverage.

```jsx
import React, { useEffect } from "react";
import Container from "./Container";

function Page(props) {
  useEffect(() => {
    document.title = `${props.title} | ComplexApp`;
    window.scrollTo(0, 0);
  }, []);
  return <Container wide={props.wide}>{props.children}</Container>;
}

export default Page;
```

On our page components (so far, Terms and About), instead of importing Container, we'll import Page, and instead of using the <Container> component, we'll use <Page>.

```jsx
<Page title="Terms & Conditions">
```

And if we needed to use a wide layout, we could add the "wide" property to <Page> here as well.

## Setting Up a Backend

For this course, the instructor has created a backend api for us. I have no idea what this really entails, but all we needed to do was download the 'backend-api' folder from the repo, create a .env file with our connection string, port, and jwtsecret, run an "npm install" command, and voila. Well, and set our database up in order to actually have a connection string that worked.

For this app, I've signed up for a MongoDB account, created a Cluster, and then a new Database (ReactCourse) and Collection (users).

## Creating Our Sign Up Form

In HomeGuest.js, we have the html for a signup form that needs to actually do something. In Lesson 26, we install Axios to deal with our form request (instead of using the Fetch API -- noted as the instructor's preference and no further information about the differences given. I found [an article that explains this](https://www.geeksforgeeks.org/difference-between-fetch-and-axios-js-for-making-http-requests/) though!)

In our HomeGuest.js, we import Axios at the top of our document:

```javascript
import Axios from "axios";
```

And within our HomeGuest() function, we add this async function to handle the submission.

```javascript
async function handleSubmit(e) {
  e.preventDefault();
  try {
    await Axios.post("http://localhost:8080/register", { username: "Test", email: "test@test.com", password: "superlongpassword123" });
    console.log("User was successfully created");
  } catch (e) {
    console.log("There was an error submitting the form.");
  }
}
```

To our <form> element, we add "onSubmit={handleSubmit}" to tell it to run the handleSubmit() function.

In Lesson 27, we get to the React part of dealing with a form (the above is all regular JS stuff).

First, we create three pieces of "state". We need to import {useState} into our file in order to use... state. :)

```javascript
import React, { useState } from "react";
```

Then within our HomeGuest() function, we'll set up our variables that will "use state". The useState() function returns an array with two items: current value and a function we can call to update the value.

```javascript
const [username, setUsername] = useState();
const [email, setEmail] = useState();
const [password, setPassword] = useState();
```

To listen for a change, we add the following to our <input> fields:

```javascript
onChange={e => setUsername(e.target.value)}
```

This updates the username (and other vars) whenever the fields are changed, so when you hit submit, they're ready for you!

We updated the post object to contain the variables we made, and tah-dah! This goes to our MongoDB.

```javascript
await Axios.post("http://localhost:8080/register", { username, email, password });
```

## Logging In

We are moving the login <form> into another component named HeaderLoggedOut.js. Again, to import a component into another component (like the HeaderLoggedOut component into the Header component), we need to import it at the top of the component, like so...

```javascript
import HeaderLoggedOut from "./HeaderLoggedOut";
```

And we need to call it within our return, like so...

```jsx
<HeaderLoggedOut />
```

As with our signup form, we add onChange properties to each input in our form, and create a function that runs when the form is submitted (using the onSubmit property on the <form> tag).

```javascript
async function handleSubmit(e) {
  e.preventDefault();
  try {
    const response = await Axios.post("http://localhost:8080/login", { username, password });

    if (response.data) {
      console.log(response.data);
    } else {
      console.log("Incorrect username/password");
    }
  } catch (e) {
    console.log("There was an error logging in.");
  }
}
```

Again, we didn't build our backend, so I have no idea what "/login" is doing, but it looks like it's checking our database for a username, then whether the password works!

## Changing the Login Area Once Logged In

We create a HeaderLoggedIn.js file to show the username, an icon, and a log out button.

Within our Header.js, we add a condition to show HeaderLoggedIn or HeaderLoggedOut depending on the state of "loggedIn".

```jsx
const [loggedIn, setLoggedIn] = useState();

{
  loggedIn ? <HeaderLoggedIn setLoggedIn={setLoggedIn} /> : <HeaderLoggedOut setLoggedIn={setLoggedIn} />;
}
```

In our HeaderLoggedIn.js and HeaderLoggedOut.js, we need to make sure the main function has "props" passed to it.

    function HeaderLoggedOut(props) { }

In our login form, if the response returns data, we update the LoggedIn state using the setLoggedIn function to true.

    props.setLoggedIn(true);

In our user data (HeaderLoggedIn.js), we add a function to the Sign Out button to change our LoggedIn state to false.

    onClick={() => props.setLoggedIn(false)}

I have to say, all of this inline code has me screaming. lol My philosophy has always been html, css, and js should all be separate! But perhaps that's an archaic way to look at things and I should get with the times. :D

## Persisting State

We'll be storing our login into local storage so that when we refresh the page, our logged in state is persistent.

To store data, we use this:

```
localStorage.setItem("nameOfData", dataToBeStored)
```

In our HeaderLoggedOut.js, we stored these pieces:

    localStorage.setItem("complexappToken", response.data.token);
    localStorage.setItem("complexappUsername", response.data.username);
    localStorage.setItem("complexappAvatar", response.data.avatar);

We can use Developer Tools to view this stored info. In Chrome, go to the Application tab and find Local Storage in the left sidebar.

In _Header.js_, we pass the following into useState for _loggedIn_.

    Boolean(localStorage.getItem("complexappToken"));

This checks to see if "complexappToken" is set in Local Storage and returns true if it does, setting loggedIn to true.

To remove items from local storage, we create this function in HeaderLoggedIn.js:

    function handleLogout() {
        props.setLoggedIn(false);
        localStorage.removeItem("complexappToken");
        localStorage.removeItem("complexappUsername");
        localStorage.removeItem("complexappAvatar");
    }

And have our Sign Out button run it onClick.

    <button onClick={handleLogout} className="btn btn-sm btn-secondary">
        Sign Out
    </button>

## Lifting States Up

We have set up our loggedIn state within our Header.js, so if we want to show something in our content area based on whether or not we are logged in, we can't, so we need to "lift up" the loggedIn state to our Main.js.

So, we moved this from Header.js to Main.js (making sure to import {useState} in Main.js):

      const [loggedIn, setLoggedIn] = useState(Boolean(localStorage.getItem("complexappToken")));

Our <Header /> now needs to be passed in that state, so we update it to:

    <Header loggedIn={loggedIn} setLoggedIn={setLoggedIn} />

And in our Header.js, we needed to update two lines. First, we need to pass props into our component function.Then we need to update any references to loggedIn or setLoggedIn so that they are methods/properties of props.

    {props.loggedIn ? <HeaderLoggedIn setLoggedIn={props.setLoggedIn} /> : <HeaderLoggedOut setLoggedIn={props.setLoggedIn} />}

And then to actually change our content area, we replace <HomeGuest /> in Main.js with a conditional:

    {loggedIn ? <Home /> : <HomeGuest />}

_The instructor has mentioned that passing down states into components, possibly many layers deep, is clunky. There are state management libraries (like Redux) that developers can use to get around this, but apparently there are now some built-in tools that we'll learn about later._

## Setting Axios baseURL

So that we don't need to include "http://localhost:8080" at the beginning of every Axios request, we can set a property called baseURL in our Main.js:

```javascript
import Axios from "axios";
Axios.defaults.baseURL = "http://localhost:8080";
```

Then all Axios requests can look something like:

```javascript
await Axios.post("/login", { username, password });
```

## Creating a New Post

In our next lesson, we create a new component for creating a post (CreatePost.js). We set this up like we've set up other components, linked to it from our HeaderLoggedIn component, and set up the route for it in Main.js.

Our form has two fields, so we create two pieces of state (I hope that's the correct way to name them...).

    const [title, setTitle] = useState();
    const [body, setBody] = useState();

And we create an asynchronous function for handling our form submission:

    async function handleSubmit(e) {
        e.preventDefault();
        try {
        await Axios.post("/create-post", { title, body, token: localStorage.getItem("complexappToken") });
        console.log("New post was created.");
        } catch (e) {
        console.log("There was a problem.");
        }
    }

We pass the token through to the backend because that's what proves that we're us! (Which makes me wonder... is localStorage the best way to pass something like that? I could create that key/value in my browser and just pretend I was someone logged in... assuming I could find that token in the first place, I guess.)

### Redirecting to new page on submit

We can use withRouter to have React redirect us to a new page url.

    import { withRouter } from "react-router-dom";

And change:

    export default CreatePost;

to:

    export default withRouter(CreatePost);

This gives properties to CreatePost() that we can pass in.

In our handleSubmit() function, we can add this to redirect us to a specific url after our post has been submitted:

    props.history.push("/post/2343"); // This is just hard-coded for now to see how this redirect works.

Every time we send a post to MongoDB, it sends back an ID in the Axios response. To leverage this, we will save our Axios.post as a variable (a const to be specific).

    const response = await Axios.post("/create-post", { title, body, token: localStorage.getItem("complexappToken") });

And use that response in our redirect:

    props.history.push(`/post/${response.data}`);

## Adding "Flash" Messages

We want to create an alert that displays at the top of the page when the user successfully creates a post. Because this is something we'd want to use throughout the site, we'll include the code for this in our Main.js so that it can be used in multiple components.

So we create a new component, FlashMessages.js. We import it into Main.js. In Main.js, we add a new state:

```jsx
const [flashMessages, setFlashMessages] = useState([]);

<FlashMessages messages={flashMessages} />;
```

To add a new message to our FlashMessages component, we create a function in Main.js that adds a new message to the end of the previous array.

```javascript
function addFlashMessage(msg) {
  setFlashMessages(prev => prev.concat(msg));
}
```

To use this function from within CreatePost, we can pass it as a property. In our JSX, this looks like:

```jsx
<CreatePost *addFlashMessage*={addFlashMessage} />
```

And in our CreatePost.js, we add this in our handleSubmit() function:

```javascript
props.addFlashMessage("Post successfully created!");
```

But, we'll probably want to use this message in many components. And this is where "context" will come in.

## Context

"An elegant way to pass/share data". Oooh, aaah.

This lesson was really difficult for me to wrap my head around (maybe because we only create something called an "ExampleContext"???), but I think I'm understanding a little better once I finally got through it all. Instead of passing states through our Main.js down to different components, we can use Contexts to pass around states instead. This is especially helpful when you have nested components, like HeaderLoggedIn.js and HeaderLoggedOut.js within Header.js.

To create a Context, first we need to create a new file. We create ExampleContext.js outside of the components folder.

```jsx
import { createContext } from "react";

const ExampleContext = createContext();

export default ExampleContext;
```

In Main.js, we import this new context file. Within the return for Main.js, we add the Context Provider (following tag) around our <BrowserRouter>:

```jsx
<ExampleContext.Provider value={addFlashMessage}></ExampleContext.Provider>
```

The opening tag can contain a value property and any child component can access the value, no matter how many layers deep. We can remove "addFlashMessage={addFlashMessage}" from the <CreatePost> component.

To leverage the value we added to the Context, we need to import our ExampleContext into our CreatePost.js.

```javascript
import ExampleContext from "../ExampleContext";
```

Within our CreatePost(), we will add the context as a variable:

```javascript
const addFlashMessage = useContext(ExampleContext);
```

And within our handleSubmit, we can remove "props" from props.addFlashMessage().

We can also pass setLoggedIn through our Context. You can pass multiple values through a Context:

```jsx
<ExampleContext.Provider value={{addFlashMessage, setLoggedIn}}>
```

In CreatePost.js, we have to update our addFlashMessage to the following (with {curly braces}), so that we can destructure the object that ExampleContext is returning.

```javascript
const { addFlashMessage } = useContext(ExampleContext);
```

And within Header.js, where we were originally passing setLoggedIn to HeaderLoggedIn and HeaderLoggedOut, we can remove those values. In HeaderLoggedIn.js and HeaderLoggedOut.js, we'll need to import useConText, the Example Context, and create a const variable with the context.

```jsx
import React, { useEffect, useContext } from "react";
import ExampleContext from "../ExampleContext";

const { setLoggedIn } = useContext(ExampleContext);
```

## useReducer

Sibling/cousin to useState. "[UseReducer] is usually preferable to useState when you have complex state logic that involves multiple sub-values or when the next state depends on the previous one." [ReactJS Docs](https://reactjs.org/docs/hooks-reference.html#usereducer)

In the course, the instructor gives us this example reducer:

```javascript
function ourReducer(state, action) {
  switch (action.type) {
    case "login":
      return { loggedIn: true, flashMessages: state.flashMessages };
    case "logout":
      return { loggedIn: false, flashMessages: state.flashMessages };
    case "flashMessage":
      return { loggedIn: state.loggedIn, flashMessages: state.flashMessages.concat(action.value) };
  }
}
const [state, dispatch] = useReducer(ourReducer, initialState);
```

To start using ourReducer, we need to replace our ExampleContext with two separate contexts, one for our state and one for our dispatch. This will stop our app from getting too bogged down, since not all components will need to watch state or change state.

```jsx
<StateContext.Provider value={state}>
<DispatchContext.Provider value={dispatch}>
```

Within our header components, we will need to import our DispatchContext and change the way we 'dispatch' actions.

```javascript
import DispatchContext from "../DispatchContext";
```

```javascript
appDispatch({ type: "flashMessage", value: "Post successfully created!" });
```

```javascript
appDispatch({ type: "logout" });
```

## Immer

Because modifying state using useReducer can get clunky (because you have to pass through all states each time), we're going to replace useReducer with useImmerReducer.

This allows us to simplify ourReducer:

```javascript
function ourReducer(draft, action) {
  switch (action.type) {
    case "login":
      draft.loggedIn = true;
      break;
    case "logout":
      draft.loggedIn = false;
      break;
    case "flashMessage":
      draft.flashMessages.push(action.value);
      break;
  }
}
const [state, dispatch] = useImmerReducer(ourReducer, initialState);
```

To leverage Immer, install it:

    npm immer use-immer

And import it into Main.js:

    import { useImmerReducer } from "use-immer";

## Restructuring Our Login

When a user logs in or out, we're using local storage to save information about them (the username, a token, and user image). When they log in, this is handled in the HeaderLoggedIn.js, when they log out, it's handled in the HeaderLoggedOut.js. This is messy and we want to update our app to do the lifting from within Main.js instead, so we'll use State to handle it!

In Main.js, we'll add a user object to our initialState:

```javascript
user: {
  token: localStorage.getItem("complexappToken"),
  username: localStorage.getItem("complexappUsername"),
  avatar: localStorage.getItem("complexappAvatar")
}
```

In ourReducer in Main.js, we add: draft.user = action.data; to the "login" case. This adds a user state with the data passed through the DispatchContext.

In HeaderLoggedIn and HeaderLoggedOut, we'll remove the localStorage settings. In HeaderLoggedOut.js, we add "data: response.data" to our appDispatch.

```javascript
appDispatch({ type: "login", data: response.data });
```

We want to keep our reducers "pure" and only include "reactish" things (or state), so we don't save our user data to local storage within ourReducer, we will useEffect and watch for changes to the loggedIn state to set localStorage instead.

```javascript
useEffect(() => {
  if (state.loggedIn) {
    localStorage.setItem("complexappToken", state.user.token);
    localStorage.setItem("complexappUsername", state.user.username);
    localStorage.setItem("complexappAvatar", state.user.avatar);
  } else {
    localStorage.removeItem("complexappToken");
    localStorage.removeItem("complexappUsername");
    localStorage.removeItem("complexappAvatar");
  }
}, [state.loggedIn]);
```

In Home.js, we'll import the StateContext, create an "appState" const variable to use that context, and replace the username from localStorage to read from appState.

```javascript
import StateContext from "../StateContext";
const appState = useContext(StateContext);

{appState.user.username}
```

## Pulling In Data from URL (useParams)

We can get parameters from the URL using react-router-dom. For our Profile page, our URL will be something like, "/profile/mandybee". To grab "mandybee", use the useParams() function.

```javascript
import { useParams } from "react-router-dom";

const { username } = useParams();
```

## State / useState

To create a new state, you'll set it up as a new array that contains two things: the current status of the state, and a function that manipulates that state. The array equals "useState()" and you can pass the initial value of the state into the function. Some examples:

```javascript
const [isLoading, setIsLoading] = useState(true);
const [posts, setPosts] = useState([]);
const [title, setTitle] = useState();
```

To check the state, you can use the first part of the array. For example: 

```jsx
if (isLoading) return <div>Loading...</div>;
```

To alter the state, use the second part of the array:

```javascript
setPosts(response.data);
setIsLoading(false);
```

You never want to manipulate the state directly (in my examples, that would be "isLoading", "posts", or "title"). Always use the function set up ("setIsLoading", "setPosts", "setTitle") and pass the new state through that.

## useEffect

useEffect takes two parameters: a function to run, and an array of dependencies to watch for changes. If the array is empty, it'll only run upon loading. This is an example

```javascript
useEffect(() => {
    document.title = `${props.title} | ComplexApp`;
    window.scrollTo(0, 0);
  }, [props.title]);
```

## Outputting Markdown Formatting

We use the react-markdown package to easily allow our posts to use Markdown!

```jsx
import ReactMarkdown from "react-markdown";

<ReactMarkdown source={post.body} allowedTypes={["paragraph", "strong", "emphasis", "text", "heading", "list", "listItem"]} />
     
```

