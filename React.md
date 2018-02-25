# What ?

> Is a JavaScript library for building user interfaces (UIs).

# Why ?

> It's a pain change the Dom as it is. The DOM (Document Object Model) is an object-oriented representation of the web page, which can be modified with a scripting language such as JavaScript. It's quite expensive manipulating the DOM because is based on a tree structure and make changes continuasly, finding a node in the middle of a web page is of course risky and time consuming. No even talk about events, ajax...etc. So one of the problem React solves for us is instead of manually find the node to do something, React takes care of that in a very declaritive way. However the DOM api methods are called under the hood.

```javascript
const weirdlist = document.querySelectorAll(".weird");
Array.from(weirdlist, element => {
  element.classList.add("++weird");
  weirdlist.onclick = function onclick(event) {
    alert("Old school");
  };
});
```

> Note: I am not disregarding the DOM as it is, in fact, my purpose is make the point for large applications where the HTML is huge and many operations are taking place.

## Wait, Components and virtual DOM

> Yeah, React make the life easy for us. Components and virtual DOM. As the html is becoming bigger React approach is to cut it into small components, reusable, manteinable and very flexible. A React Component is just a function which has some inherit methods from React base class.

> React use the concept of classes, however we could make it composable (see Class.md) because of his benefits.

```javascript
class Button extends React.Component {
  render() {
    return <button>A button</button>;
  }
}

ReactDOM.render(<Button />, document.getElementById("root"));
```

(Composable way)

```javascript
/* index.js */
import React from "react";
import { render } from "react-dom";

const App = createApp(React);

render(<App />, document.getElementById("root"));
/* app.js */
import buttonFactory from "components/title";

export default React => {
  const Button = buttonFactory(React);
  return (
    <div>
    <p>Loren ipsum</p>
    <Button/>
    </div>
  );
};

/* buttonFactory.js */

export default React => {
  const button = () => {
    return <button>A button</button>;
  };
  return button;
};
```

> React use JSX which looks similar to HTML but transpile in JavaScript.

* Use inherit methods to transform into elements (React.createElement('button', null, 'A button'))
* To use a class which is a reserved word in js, React use className.
* To write actual js has to be in {}

# Flow data in React

> Props down, events up

* Props => Is an object with data so it's what a component receives, look similar to HTML element attributes.

```javascript
/* In the button file*/
function Button(props) {
  return <button>{props.text}</button>;
}
const button = <Button text="A button" />;
ReactDOM.render(button, document.getElementById("root"));
```

* Events

* In JavaScript, class methods are not bound by default. The event handler has to be a method on the class in a class base React App. Or it could be:

```javascript
function handleClick(e) {
  e.preventDefault();
  console.log("The button was clicked.");
}

<button onClick={someFun}>Activate Lasers</button>;
```

#Components types

> We could difer two type of components: stateful/smart and stateless/dumb.

> Smart components

* Describe how things work
* Provide no DOM markup or styles
* Data fetching

> Presentional components

* Describe how things look
* No app dependencies, awesome
* Receive only props, providing data and callbacks
* Own state but only UI stuff

# State

> Is an object with data. Each component can have state, it is private and fully controlled by the component. A feature available only to classes. The only place where you can assign this.state is the constructor.

```javascript
/* Example by React documentation */
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = { date: new Date() };
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

> To change the state of the component

```javascript
this.setState({
  date: new Date(99, 5, 24)
});
```

* The only place where you can assign this.state is the constructor.
* As this.props and this.state may be updated asynchronously, we must not rely on their values. Use a second form of setState() that accepts a function rather than an object. That function will receive the previous state as the first argument, and the props at the time the update is applied as the second argument.

```javascript
/* Example by React documentation */
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```

* When you call setState(), React merges the object you provide into the current state.
* Calling setState will only update the properties passed as an argument, not replace the entire state object.

# Handle events

* Parent components can pass callback functions as props to child components to allow two-way communication. Better approach. This way allow us to isolate bugs and sync components with the same data. This keeps apps modular and fast.

# Lifecycle methods

> Lifecycle methods in React are functions that get called while the component is rendered for the first time or about to be removed from the DOM.

* Mounting means rendering for the first time.

> Mounting:

* constructor()
* componentWillMount()
* render()
* componentDidMount()

> Update

* An update can be caused by changes to props or state.

* componentWillReceiveProps()
* shouldComponentUpdate()
* componentWillUpdate()
* render()
* componentDidUpdate()

> Unmounting

* componentWillUnmount()

> Error Handling

* componentDidCatch()

_See more in https://reactjs.org/docs/react-component.html_

[Continue 2](React2.md)
