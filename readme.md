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

## JSX Syntax Changes from HTML

- The "class" attribute is replaced with "className".
- The "for" attribute is replace with "htmlFor".
