# Lifecycle

<!-- (https://media.giphy.com/media/3oEjI0zBPtJFnfLCRW/giphy.gif) -->
> As React uses the concept of components and the virtual DOM, we can interact with the component itself. Lifecycle methods are to be used to run code and interact with your component at different points in the component's life. These methods are based around a component Mounting, Updating, and Unmounting.

Here a nice diagram: <[Imgur](https://i.imgur.com/KGqOm2V.jpg)>

# Component Creation (Mount)

> When React creates an instance of a component and inserts it into the DOM (mounting), the following methods are called:

* constructor() =>

  > Setting default state/ default props, after super is called because the subclass instance "is-a" instance of the superclass, and must be a properly set-up superclass object. So therefore the superclass's constructor needs to run first to set up the superclass's invariant.

  ```javascript
  MyReactClass.defaultProps = {
    name: "Ismy",
    initialCount: 0
  };
  ```

* componentWillMount()

  > This function can be used to make final changes to the component before it will be added to the DOM.

* render()

  > This function should be a pure function of the component's state and props. It returns a single element, which represents the component during the rendering process and should either be a representation of a native DOM component or a composite component. If nothing should be rendered, it can return null or undefined.

* componentDidMount()

  > The component has been mounted and you are now able to access the component's DOM nodes, e.g. via refs. This method should be used for:

  * Preparing timers
  * Fetching data
  * Adding event listeners
  * Manipulating DOM elements

# Component Update

* componentWillReceiveProps(nextProps)

> This is the first function called on properties changes. When component's properties change, React will call this function with the new properties. You can access to the old props with this.props and to the new props with nextProps.

* shouldComponentUpdate(nextProps, nextState)

> By default, if another component / your component change a property / a state of your component, React will render a new version of your component. In this case, this function always return true. But should re-render? This is more for optimization. Returning false if it doesn't need to re-render.

```javascript
componentShouldUpdate(nextProps, nextState){ return this.props.name !== nextProps.name ||
this.state.count !== nextState.count; }
```

> If true the following lifecycle methods are called

* componentWillUpdate(nextProps, nextState){}

  > Changes aren't in DOM, so you can do some changes just before the update will perform. You cannot use this.setState().

* render()

  > There's some changes, so re-render the component.

* componentDidUpdate(prevProps, prevState)

  > DOM is refreshed, so you can do some work on the DOM here.

# Component Removal

* componentWillUnmount()

  > This method is called before a component is unmounted from the DOM. It is a good place to perform cleaning operations like:

  * Removing event listeners.
  * Clearing timers.
  * Stopping sockets.
  * Cleaning up redux states.
