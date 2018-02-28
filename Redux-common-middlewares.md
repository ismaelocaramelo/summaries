![IMG](https://media.giphy.com/media/l4v8p7s9Ylopq/giphy.gif)

# Thunk

> Thunk is a function which return a fucntion. This allows us to dispath a functions, mainly to perfom asynchronous actions. Also has the same power as any other middleware getting the state of the store and dispaching actions.

# How it is implemented?

```javascript
function createThunkMiddleware(extraArgument) {
  return ({ dispatch, getState }) => next => action => {
    if (typeof action === "function") {
      return action(dispatch, getState, extraArgument); // If the accion it's a thunk we invoked passing those arguments (dispatch, getState, extraArgument)
    }

    return next(action); // continue with the chain
  };
}

const thunk = createThunkMiddleware();
// Storing the actual function to allow passing a parameter which will be passing to the accion.
thunk.withExtraArgument = createThunkMiddleware;
// E.g applyMiddleware(thunk.withExtraArgument(/*anything*/))
// so:
// action(dispatch, getState, anything)
export default thunk;
```

# Why ?

> Your application may need async task such as fetch users, promises... etc. Thunk middleware provide that dispatching actions as if it were normal actions

### Example:

```javascript
function complexSynchronousThunk(someValue) {
  return (dispatch, getState) => {
    dispatch(someBasicActionCreator(someValue));
    dispatch(someThunkActionCreator());
  };
}

store.dispatch(complexSynchronousThunk(34));
```

**This is just to grap the concept of thunk**

# Saga

> Usefull to manages Side-Effects (API, DB, logs, etc.). Saga means a long story with events. A long running process.

# Eih ?

> Saga is kind of a long running process which waits for accions and respond them with a new accions, side effects.

# How ?

> First of all we need to inserted into the middleware chain. Sagas (processes) are started, middleware chain passed a copy of any actions dispatched.

# Why ?

> We love to look at things which are declarative and sync looking. Saga is based on generators (see [AsyncThings](AsyncThings.md) for understanding generators). To recapitulate, generators suspend the child process to yield something (api request, delay stuff...).

## Steps

1. Middleware initialized
2. Sagas initialized
3. Middeware added to store
4. Action is passed from middleware to saga
5. Saga executes, creates side-effects
6. Process repeats indefinitely

**I really encourage you to follow the documentation as well as the example in [Redux-doc](https://redux-saga.js.org/docs/introduction/) to fully understand it. Good luck**

[Part 1](Redux-middleware.md)
