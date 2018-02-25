# Communication

![IMG](https://media.giphy.com/media/AGhJK54sWB5io/giphy.gif)

> The art of communication between components.

# Types

> Parent to Child Components,
> Child to Parent Components,
> Not-related Components,

* Remember: Props down events up

# Parent to Child Components

> The parent renders a child component and passes to it some props.

```html
<div>
  <Child message="message for child" />
</div>
```

# Child to Parent Components

> Sending data back to the parent, pass a function as a prop from the parent component to the child component, and the child component calls that function. Remember to bound the context because class in js does not bound by default.

```javascript
import React from "react";
class Parent extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
    this.outputEvent = this.outputEvent.bind(this); // bounded !
  }
  outputEvent(event) {
    this.setState({ count: this.state.count++ });
  }
  render() {
    const variable = 5;
    return (
      <div>
        Count: {this.state.count}
        <Child clickHandler={this.outputEvent} />
      </div>
    );
  }
}
class Child extends React.Component {
  render() {
    return <button onClick={this.props.clickHandler}>Add One More</button>;
  }
}
export default Parent;
```

# Not-related Components

> If a component does not have parent-child relationship. The only way is by subscribing to events, and the other writes into. They are based in the observer pattern. More info: [Here](https://github.com/millermedeiros/js-signals/wiki/Comparison-between-different-Observer-Pattern-implementations)

> However, we want things modular as possible, manteinable and easy to implement. If we apply the observer pattern is likely that a party of events will take place. So somehow we need a mechanisms to reduce time consuming and responsible to handle the state of the application. Redux which I will cover later is mainly the best approach.

[Continue 4](React4.md)
