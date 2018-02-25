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

### Examples:

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

> First of all we need to inserted into the middleware chain. Sagas (processes) are started, Middleware chain passed a copy of any actions dispatched

## Steps

1. Middleware initialized
2. Sagas initialized
3. Middeware added to store
4. Action is passed from middleware to saga
5. Saga executes, creates side-effects
6. Process repeats indefinitely

**Will continue soon**

[Part 1](Redux-middleware.md)
