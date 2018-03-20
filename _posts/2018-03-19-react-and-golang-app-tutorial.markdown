---
layout: post
title:  "React and Golang App Tutorial"
date:   2018-3-19 
tags: react javascript golang tech 
---
## Part 1  
This series will walk through the building of an application with React and Golang.  
The app will be a real-time messaging app in the style of Slack.

---  

## Installation and Setup

### Install Node Package Manager  
Node package manager (npm) is automatically installed with Node.js.  

Check your version of npm:  
```bash
$ npm -v  
```

Run the command to install npm update the latest version of npm:  
```bash  
$ npm install npm@latest -g
```  

If you don't have npm or Node.js installed, then you're out of luck.  
Just kidding. You can install Node.js from [here](https://nodejs.org/en/download/).  

### Install React  

```bash  
$ npm install -g create-react-app  
```

### Create React App  
Now that you have the React CLI tool installed, 
you can run the command to create your React app!  
```bash  
$ create-react-app my-app  
```  
Change into the directory:  
```bash  
$ cd my-app/  
```  
Fire it up (locally)!  
```bash  
$ npm start  
```
You can now view your app in the browser at http://localhost:3000/  

###### Pro Tip: Enter `Control + C` to return to the command mode in terminal  

## Planning the User Interface  

Our app will have three major sections to the UI:  
* Message ~~Window~~ Component  
* User ~~Window~~ Component  
* Channel ~~Window~~ Component  

In React, the convention is to call parts of the UI its "components".  

Our *Channel Section Component* will feature two sub-components:  
* Channel List Component  
* Channel Form Component  

The *Channel List Component* will feature a single sub-component:  
* Channel Component  

We want to create a React component that creates an HTML list tag with the channel name.
```html  
<li>Channel Name</li>  
```  
Like that!  

## Class and Component Inheritance  
To achieve this, we will use the *class* feature of ES2015 to extend the React Component class.  
This is similar to the *Inheritance* feature in Object-Oriented Programming.
A `render(){}` method must be implemented to use a React component.  
Our render method will return a list tag with a hard-coded string.  

Go to the file _./src/App.js_ and edit the return value from this:  

```javascript  
# ./src/App.js  

class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h1 className="App-title">Welcome to React</h1>
        </header>
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
      </div>
    );
  }
}
```
To this:  

```javascript  
# ./src/App.js  

class App extends Component {
  render() {
    return(
      <li>Channel Name</li>  
    )
  }
}
``` 
This seeming mix-up of javascript and html code is JSX, 
an "XML-like syntax extension to ECMAScript".  
The JSX code will will be transpiled by Babel, and become JavaScript that makes a call to a list tag and load it into the browser. 

Next, change the component name of `App` to `Channel` so that the file looks like this:   

```javascript  
# ./src/App.js  

import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class Channel extends Component {
  render() {
    return (
      <li>Channel Name</li>
    );
  }
}

export default Channel;  
```

Continue refactoring with the renaming of `App` to `Channel` in _my-app/src/index.js_ so that it looks like this: 

```javascript  
# ./src/index.js  

import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import Channel from './App';
import registerServiceWorker from './registerServiceWorker';

ReactDOM.render(<Channel />, document.getElementById('root'));
registerServiceWorker();
```  
Fire up node with command `$ npm start` and test in the browser.
The list tag should render with a simple string "Channel Name".

## Properties  
React *properties* offer a way to pass information to Components.  
Communication of properties is bi-directional:  
information can be passed from one component to another.  

How do we use properties? Simple.

When the component is called, we add an arbitrarily named attribute or property with some value.


```javascript
# ./src/index.js  

ReactDOM.render(<Channel />, document.getElementById('root'));
...
```
Becomes this:
```javascript
# ./src/index.js  

ReactDOM.render(<Channel someAttr="Some Value"/>, document.getElementById('root'));
...
```
Let's do this:
```javascript
# ./src/index.js  

ReactDOM.render(<Channel name="Cool Channel Name"/>, document.getElementById('root'));
...
```
To use the property, React uses the `props` object chained together as `this.props.someAttr`.
Change the `<li>Channel Name</li>` code on `App.js` to this:
```javascript
<li>{this.props.name}</li>
```
###### Note the curly braces! Important with React, so that it is interpreted as an expression and not a string.

## Review  
We have installed React and created our first React app which includes the following functionality:  
1. A simple Channel component.
2. The channel is passed a name, which is a property.
3. The channel component renders a name in a list tag.  

Next, we will add event listeners so that an event is triggered to display messages for a particular channel when it is clicked.