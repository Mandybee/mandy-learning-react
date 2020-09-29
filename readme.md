# Learning React!

This little project is me following along in a Udemy class: [Available here](https://www.udemy.com/course/react-for-the-rest-of-us/). I'm going to (hopefully) keep a list of notes here about things I learn along the way!

## Getting Started...

The intro to React was given as a little crash course using CodePen.io. [Mandy's React Codepen](https://codepen.io/mandybee/pen/zYqMeEX)

After playing around with syntax and such for a bit, we moved on to create our own app locally.

## Setting up a local environment

I'm using Visual Studio Code to build this. It has built-in support for JSX, woo!

To make my life easier, the instructor suggests adding this to the settings.json for VSC to make all js files open using the "javascript react" formatting:

    "files.associations": {
        "*.js": "javascriptreact"
    }

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

    "dev": "webpack-dev-server"

To run our task:

    npm run dev

Our site is now available at: [localhost:3000](http://localhost:3000) And it'll refresh the page when we make edits to our site!

To have our browser only load JS changes (rather than refreshing page), add this to the end of our site's JS:

if (module.hot) {
module.hot.accept();
}

### Create HTML, CSS, and main JS Files

Once React is installed and we have all of our packages, we can get started! The first thing the instructor tells us to do is create an "app" folder. In this folder, we'll put all of our site files (aside from the config stuff we created).

For our project, we'll need one html document (index.html). For this project, we'll pull in Public Sans (google font), Bootstrap (a CSS framework), Fontawesome icons, and then our own CSS and JS files. Aside from these files, the html document will only have a <div> with an ID on the page, where our React component will be built.

Along with our html document and css document, we need to create a JS document (we called ours "Main.js").

To include our Javascript, we link to "/bundled.js", which is hidden in our local files, but is created by webpack and is available for our app on the web server.

## Setting Up Our JS

Our Main.js file needs to start with the following:

    import React from "react";
    import ReactDOM from "react-dom";

Then we'll create a function for our main HTML output:

    function Main() {
        return (
            <>
                <h1>This is my site!</h1>
            </>
        );
    }

And render that onto the page with:

    ReactDOM.render(<Main />, document.querySelector("#app"));

Woot! All done. lol Okay... just kidding.

## Create a "components" folder

Instead of jamming all of our code into one file, we need to separate our JS into small pieces. At the time of writing this (a bit after having set up our site), I have the following files in my components folder: _About.js, Container.js, Footer.js, Header.js, HomeGuest.js, Terms.js_

Each new component JS file needs to start with:

    import React from "react";

Then contains the function with all of that component's JS. And then it should end with:

    export default functionName;

Back in our Main.js, we have to import our components to use them:

    // Components
    import Header from "./components/Header";
    import HomeGuest from "./components/HomeGuest";
    import Footer from "./components/Footer";
    import About from "./components/About";
    import Terms from "./components/Terms";

Then we can include them in our Main() function like:

    <Header />
    <HomeGuest />
    <Footer />

### Creating a Snippet in Visual Studio Code

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

    import { BrowserRouter, Switch, Route } from "react-router-dom";

In Main()'s return, we're going to overhaul our JSX.

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

<BrowserRouter> wraps around our whole layout.

<Header> and <Footer> will be on all pages, so they're children of BrowserRouter.

The <Switch> element handles routing for a section.

Each <Route> within a <Switch> is a new 'page' that will be served up based on the URL in the browser window (or links selected within the app). Between the <Route> tags, we can include content to show on the page.

\*This was already in the webpack config code taken from GitHub, but I think it's important to note that this line in the webpack.config.js makes sure that we can, in fact, visit "localhost:3000/about-us" and have it work. It forces the browser to head to index.html rather than trying to find the index file in an "about-us" folder.

    historyApiFallback: { index: "index.html" }

### Changing Links

In our Footer.js, we had some links written in regular old HTML. To set up routing, we went in and changed all of the <a> tags to <Link> tags and all of the "href" attributes to "to" attributes. Our "to" attributes are pointed to "/", "/about-us", and "/terms".

At the top of our Footer.js, we included:

    import { Link } from "react-router-dom";

We did similar stuff to our Header.js, with its one link.

#### Updating Page Titles

Very simply, you could add the following in your component's function:

    useEffect(() => {
        document.title = "Terms & Conditions | ComplexApp";
        window.scrollTo(0, 0);
    }, []);

And add ", {useEffect}" into the "import React..." line at the top.

BUT! This creates a function that we'd just be repeating over and over again on every page we create, so it's not the best way to do it...

See "Composition" section for the better way to do this.

## JSX Syntax Changes from HTML

- The "class" attribute is replaced with "className".
- The "for" attribute is replace with "htmlFor".

## Reusable and flexible containers

We created a Container.js file to hold the following code:

    import React, { useEffect } from "react";

    function Container(props) {
        return (
            <>
                <div className={"container py-md-5 " + (props.wide ? "" : "container--narrow")}>{props.children}</div>
            </>
        );
    }

    export default Container;

We use the "props" variable to pass through variables from our components where <Container> is used. Most of our pages will use the "container--narrow" class for the containing div, so we've added the conditional to our Container.js to show "container--narrow" unless the "wide" property is set to true.

On our HomeGuest.js, we've added "wide={true}" to the Container tag. Voila!

## Composition

Because many pages have similar content (such as needing to set the page title), we'll create a new component (Page.js), that we'll use in our page components. This will import the "Container" and create a new Page component that we can leverage.

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

On our page components (so far, Terms and About), instead of importing Container, we'll import Page, and instead of using the <Container> component, we'll use <Page>.

    <Page title="Terms & Conditions">

And if we needed to use a wide layout, we could add the "wide" property to <Page> here as well.

## Setting Up a Backend

For this course, the instructor has created a backend api for us. I have no idea what this really entails, but all we needed to do was download the 'backend-api' folder from the repo, create a .env file with our connection string, port, and jwtsecret, run an "npm install" command, and voila. Well, and set our database up in order to actually have a connection string that worked.

For this app, I've signed up for a MongoDB account, created a Cluster, and then a new Database (ReactCourse) and Collection (users).

## Creating Our Sign Up Form

In HomeGuest.js, we have the html for a signup form that needs to actually do something. In Lesson 26, we install Axios to deal with our form request (instead of using the Fetch API -- noted as the instructor's preference and no further information about the differences given. I found [an article that explains this](https://www.geeksforgeeks.org/difference-between-fetch-and-axios-js-for-making-http-requests/) though!)

In our HomeGuest.js, we import Axios at the top of our document:

    import Axios from "axios";

And within our HomeGuest() function, we add this async function to handle the submission.

    async function handleSubmit(e) {
        e.preventDefault();
        try {
            await Axios.post("http://localhost:8080/register", { username: "Test", email: "test@test.com", password: "superlongpassword123" });
            console.log("User was successfully created");
        } catch (e) {
            console.log("There was an error submitting the form.");
        }
    }

To our <form> element, we add "onSubmit={handleSubmit}" to tell it to run the handleSubmit() function.

In Lesson 27, we get to the React part of dealing with a form (the above is all regular JS stuff).

First, we create three pieces of "state". We need to import {useState} into our file in order to use... state. :)

    import React, {useState} from "react";

Then within our HomeGuest() function, we'll set up our variables that will "use state". The useState() function returns an array with two items: current value and a function we can call to update the value.

    const [username, setUsername] = useState();
    const [email, setEmail] = useState();
    const [password, setPassword] = useState();

To listen for a change, we add the following to our <input> fields:

    onChange={e => setUsername(e.target.value)}

This updates the username (and other vars) whenever the fields are changed, so when you hit submit, they're ready for you!

We updated the post object to contain the variables we made, and tah-dah! This goes to our MongoDB.

    await Axios.post("http://localhost:8080/register", { username, email, password });

## Logging In

We are moving the login <form> into another component named HeaderLoggedOut.js. Again, to import a component into another component (like the HeaderLoggedOut component into the Header component), we need to import it at the top of the component, like so...

    import HeaderLoggedOut from "./HeaderLoggedOut";

And we need to call it within our return, like so...

    <HeaderLoggedOut />

As with our signup form, we add onChange properties to each input in our form, and create a function that runs when the form is submitted (using the onSubmit property on the <form> tag).

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

Again, we didn't build our backend, so I have no idea what "/login" is doing, but it looks like it's checking our database for a username, then whether the password works!
