# A deep intro to middlewareee

> First of all, I will try my best to fully understand middleware in redux. Middleware is a function. What a surprise. Something between action and reducer. Better

# How

> Every time we dispatch an action, the middleware world knows it.

```javascript
//Source code
// Whatever enhancer is given will be invoking with the store context.
// The main reason is to be able to get current state as dispatch real actions.
// Otherwise we will need to access the parent scope and mess up with that.
enhancer(createStore)(reducer, preloadedState);
```

![IMG](https://media.giphy.com/media/Ysce790SgjJK0/giphy.gif)

> Enhancer is an Higher Order Function. By default Redux supplied a applyMiddleware enhancer to deal with multiple functions (middlewares). From now on, redux will be based mainly in FP.

# Why

> Well, there is times when you may need to dispatch an action which is promise, same delay or even not a plain object. The solution is a Middleware. Remember the director before the assembly begins.

# Let's dig into the source code

> So the first thing, ENHANCER. The most used is applyMiddleware by default.

```javascript
const store = createStore(
  reducer,
  preloadState,
  applyMiddleware(middle1, middle2, middle3)
);

//
applyMiddleware(middle1, middle2, middle3)(createStore)(
  reducer,
  preloadedState
); // Here is how looks like behind the scenes
//
```

```javascript
// Official source code
export default function applyMiddleware(...middlewares) {
  //receiving middlewares
  return createStore => (reducer, preloadedState, enhancer) => {
    // returning two executions to be able to access the store and so on...
    const store = createStore(reducer, preloadedState, enhancer); // create a copy of the store
    let dispatch = store.dispatch; // copy of a dispatch. Rembember is copying the address of the real dispatch object
    let chain = [];

    const middlewareAPI = {
      getState: store.getState,
      dispatch: action => dispatch(action)
    };
    chain = middlewares.map(middleware => middleware(middlewareAPI)); // Here they are passing the {getState, dispatch} to each middleware

    // COMPOSITION !
    // Because we want every middleware to know what's going on with the state and also able to dispatch accions
    // Bear in mind the accion could be literally anything. We can deal with that :)
    // Calling dispatch(accion). It's ultimately doing this:
    // store.dispatch(middle1(middle2(middle3(accion)))) Executing from rigth to left
    dispatch = compose(...chain)(store.dispatch);

    // Every enhancer returns the store and extension of the dispatch method.
    return {
      ...store,
      dispatch
    };
  };
}
```

# Key points

* Each middleware requires no knowledge of what comes before or after it in the chain.
* Mainly is to support asynchronous actions
* Isolate API communications
* Manage side-effects
* Pre-process actions
* Track or debug an application

  # Build a middleware

* Each middleware as we've learnt is giving first an obj with getState and dispatch.
* Then, thanks to composition receives the nextDispatch funtion in the nth middleware
* Finally the accion. Yes, it is a mind blown

```javascript
const dummyMiddleware = ({ getState, dispatch }) => nextDispatch => accion => {
  console.log("An accion is comming", accion);
  // Do something with the accion... E.g If accion it is a function (thunk), promise deal with it.
  let result = nextDispatch(accion); // will dispatch to real store

  // We could check the current state via getState after dispatching
  console.log("current state now", getState());

  return result; // to continue the chain
};
```

**Easy intro, I hope it helps**

# Most used middlewares.

* Redux Thunk => Allows functions to be passed to dispatcher
* Redux Saga => Runs asynchronous processes
* Redux Promise => Allows promises to be passed to dispatcher
* Redux Logger => Logs actions to the console
* Redux Undo => Revert to previous application states

[Part 2](redux-common-middlewares.md)
