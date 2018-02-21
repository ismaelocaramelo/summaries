# Understanding Redux

<!-- (https://media.giphy.com/media/3ohhwkXgsM2rMOyRi0/giphy.gif) -->

# Everything starts from a store.

> Store is a function that takes three parameters (reducer, preloadState, enhancer) and returns and object with a couple of methods to interact with the store (suscribe, dispatch, replaceReducer, getState).

**I will cover reducer and enhancer in separate files as it's a complex topic**

* The reducer is a must when creating store. The others arguments are optional however in large applications becomes necessary.

```javascript
import { createStore } from "redux";

const store = createStore(reducer); //Simple store
```

> The store acts as hold tree state. Let's imagine is a factory of cars. The factory has mechanisms (reducers) to take all the pieces that are already there and assembly them into cars. The factory may have these pieces ready to use or may not (preloadState). And finally every good factory has a director (middleware) who decide what to do before the piece is assembly in the line of production. Each parameter will be covered later.

> The store allows you read the state, dispatch actions and subscribe to changes.

# Outcome of the store

> Dispatch

* Is a method of the store. Is the only way to change the state. This method has one argument, an action.

```javascript
const action = { type: "SOME_TYPE", payload: "Hey, a simple Action" };
store.dispatch(action);
```

* Returns the action itself.

* The action must to be a plain object. Every action must have a type otherwise it will throw an error and optionally a payload.

* If you want to dispatch a function to do async stuff which it may be a promise of something else, has to be processed in a middleware to resolve it and then chain it into the next middleware.

* Soooo dispatch is synchronous by default.

* It is also responsable for call the listener that you may registered in the suscribe method. Inside that listener you may call getState to see the current state. Avoid dispatch from that listener because the subscriptions are snapshotted just before every `dispatch()` call. If you suscribe after an action has been dispatch it will not get the latest state.

> suscribe

* Store method. Here you can listen for actions that currently happen. Passing a callback (listener) as argument. This listerner will be pushed to an array of literners and eventually invoke when a dispatch is called.

* Returns a unsubscribed function which does that, eliminates the subscription of the array of listeners.

```javascript
const dummyHandle = () => {
  document.body.innerText = store.getState();
};
// Register the callback before a dispatch is called to get the latest state
let unsubscribe = store.subscribe(dummyHandle);

// To unsubscribe
unsubscribe();
```

> replaceReducer

* Well is very declarative. Takes the next reducer for the store to use. But I'd prefer another way to handle reducers, which is commonly used.
