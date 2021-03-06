# Just Another React Tutorial

### What is React?

* React is a JavaScript Library created by Facebook
* Write organized code
* Write code quickly
* Utilizes components
* Virtual DOM efficiency

---

### What We're Building

* In this tutorial we'll be building a web app that will search for movies
* We'll be using the OMDB API
* Clone this repo onto your computer
* Then grab all the dependencies using:

---

### Other Companion Technologies

* OMDB API - I picked this because it is a free api that does not require a key. However, IMDB does not allow people to take their images so keep that in mind if you are trying to utilize OMDB for hosting an actual movie app
* Webpack - We use this to bundle our modules. When you run `npm start` you are actually starting the "webpack-dev-server" which will take care of everything for us. If you were to build your own full stack application you will need to build a `webpack.config.js` file and hook it up to your server. 
* ES6 - The newest version of JavaScript. Also known as `es2015`
* JSX - A preprocessor that allows us to write XML syntax inside of our JavaScript files. Since we are using a Virtual DOM JSX makes it look as close to writing HTML as possible.
* Axios - A promise based http client. Make XMLHttpRequests
* Babel - Transform our ES6 and JSX to JavaScript
* Node - A runtime environment for JavaScript. 
	* From [https://nodejs.org/en/](https://nodejs.org/en/)

```
Node.js® is a JavaScript runtime built on Chrome's V8 JavaScript engine. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient. Node.js' package ecosystem, npm, is the largest ecosystem of open source libraries in the world.
```
* npm - the package manager for node modules

---

### 00 The Master Branch

```
npm install
```
* This command is a node command that will download all the node modules listed in the `package.json` file
* If you don't have npm then... you should probably download yourself npm
* Now let's run the server

```
npm start
```
* Now open up your browser and visit `localhost:8080`
* This is our end goal. Or as far as this tutorial will take you. Hopefully you can rebuild this into your own apps and incorporate newer concepts. I'll suggests some later on. 

### 01 The Set Up

* Lets run throught our files and folders
* `package.json` - again this is used to tell people your dependencies for this app. We used it to install the correct node modules
* `app/.babelrc` - To use babel to transform our items we have to pass in "react" and "es2015"
* `index.html` - has some cdns for jquery, materialize. Also has only a single element with an id of `app`. We're going to target this in our JS file

##### `index.js` - The Important Stuff!

```
import React from 'react';
import {render} from 'react-dom';

function HomeComponent(props){
	return(
		<div>
			<h1>Hello World!!!</h1>
			<h3>You're using REACT!!!!</h3>
		</div>
	)
}
const HomeContainer = React.createClass({
	render(){
		return(
			<HomeComponent/>
		)
		
	}
})
render(
	<HomeContainer/>,
	document.getElementById('app')
)
```
* the imports are ES6 syntax. We are grabbing what we want from the node modules we installed earlier

##### Containers vs Components

* Many tutorials will use some of the following technologies
	* `Containers vs Components`
	* `Smart and Dumb Components`
	* `Containers vs Stateless Presentational Components`
* We'll stick with the first one
* Our Containers will hold all the `state` and the `logic` for that piece of the Virtual DOM
* Our Components will be logicless and stateless. Their only goal is to present the element to the page. You can put your styling here. 

##### Rest of the code

* `const` - an ES6 syntax that we are using to declare an object instead of using `var`. Const also tells us that this object is `immutable`
* `render(){}` - ES6 syntax. This just means `function render(){}`
* `<HomeComponent/>` - JSX syntax inside of the `return / render` of Container
* All const/containers must have a render function with a return statement, returning an object that represents the virtual DOM
* The last render is coming from the `react-dom` node module and we are passing it all of our objects to render to our element with an id of `app`

### 02 Separation of Concerns

* Now in this branch we're going to separate everything out. 
* We learned about Containers and Components but now we're going to give them their own folders. 
* In a larger app you might even break these up further depending on now many components you'll have
* Since these are all new files we'll have to export and import them into each other
* `HomeContainer`

```
import React from 'react';
import HomeComponent from '../components/HomeComponent';

export default HomeContainer;
```
* Import the HomeComponent for use
* Export the const object so the `index.js` file can use it
* `HomeComponent`

```
module.exports = HomeComponent;
```
* We are exporting our `Stateless Presentational Component` so the HomeContainer can use it
* `index.js` Now Our Index.js File looks like this!!!

```
import React from 'react';
import {render} from 'react-dom';

import HomeContainer from './containers/HomeContainer';


render(
	<HomeContainer/>,
	document.getElementById('app')
)
```

##### Misc.

* Now you can start seeing the organizational piece. 
* Utilizing components in this manner lets us keep them small and also makes them reusable


### 03 State and Props

* If we keep our logic in one place and the styling in another place how do the two talk to each other?
* `State and Props` - These are the meat and potatoes of React, or if you're vegetarian, lettuce and tomatoes. 
* Your `State` is similar to props but acts as a private object that is only accessible by that component.

##### HomeContainer

* Inside our container we will use a built in function called `getInitialState`
* `getInitialState()` 
	* ES6 for function getInitialState()
	* Always takes a return
	* Create a JavaScript Object
	* This object is immutable, we'll cover how to change these later
	* You can pass this whole object or just specific parts to the component
* `handleUserSubmit()` 
	* This is our own function built to handle an event
	* This event listener will be passed down to the component
* Now when we render our `HomeComponent` we'll use JSX syntax to pass in the props. 
* Feel free to check out the console.log for "this.state"

##### HomeComponent

* Inside our HomeComponent, along with the styling, we can use the props that were passed in to show on the page.

```
<h2>The name from our props is {props.name}</h2>
```
* props.name was accessible because inside our HomeContainer we rendered the HomeComponent like this

```
	render(){
		console.log(this.state);
		return(
			<HomeComponent 
				data = {this.state}
				name = {this.state.yourName}
				onUserSubmit = {this.handleUserSubmit}/>
		)
	}
```

##### Misc.

* When creating and passing functions that will handle events best practice is to use `"handle"` when creating the event listener and `"on"` when passing it as a prop
* `event.preventDefault()` is used to prevent the form from reloading

### 04 Ajax and Axios

* Alright so this app isn't going to just stop at console logging
* We can grab the user input but lets start sending it to the OMDB API
* Again, we want to organize our code into small files with a specific kind of structure
* For these AJAX calls we're going to use a node module called `Axios`
* These calls will be in the `helpers` file
* We're going to import and export them the same as we do with everything else

##### Misc.

* In our container you'll see `let movieTitle` inside the handleUserSubmit.
	* let is another way to declare a variable instead of `var`
	* `let` allows us to create variables that will not get `hoisted`





##### Things to write about

* What is React
* How is it different from other frameworks
* What are the technologies listed above
* A step by step tutorial on what we are building
* Always start with set up structure
* Then build your shit with hello world
* Include luxury goals, what to learn next
	* react router
	* actually hosting it on your own express server
	
##### Clone this Repo

##### NPM Install yo shit

##### Take a look at the file structure

* webpack.config.js
	* Webpack input and output
* index.html
	* script tag targets dist/index_bundle.js	
* .babelrc
	* used to convert react and es6 	
* package.json


##### Where do these fit

* Component life cycle
* Virtual DOM
* State to props


##### Steps

* Branch 1 - The Set Up (includes Hello World)
* Branch 2 - Containers vs Components (build out the interface)
* Branch 3 - State and Props and Event listeners
* Branch 4 - Final touches
* Maybe show the idea of container components all in the index.js file?
* Then make a new branch breaking all that up?
* then make a new branch making the axios calls?