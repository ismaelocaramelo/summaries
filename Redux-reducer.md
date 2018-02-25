# Reducer

![IMG](https://media.giphy.com/media/26ufe34jLiGEOqyM8/giphy.gif)

> Is a pure function. It's responsable to actually define how the state will change. Returns a new state.

# Create a reducer

> When dispathing an action calls the currentReducer and passed two arguments (currentState, action).

```javascript
// From official documentation
function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, { visibilityFilter: action.filter });
    default:
      return state;
  }
}
```

> Well tipically a reducer is made of a switch statement, however this can lead to repetition reassigning the variable, but we know it has to be a pure function so always the same outcome in other words a new fresh object. It's pretty fast because it only evaluates once. However we would a functional approach because it's more maintanable and modular. So we will create a factory reducer!

```javascript
export const createReducer = (initialState, handlers) => {
  return function reducer(state = initialState, action) {
    if (handlers.hasOwnProperty(action.type)) {
      return handlers[action.type](state, action);
    } else {
      return state;
    }
  };
};

export const currentUser = createReducer(null, {
  [UPDATE_STATUS](state, action) {
    return state.set(`status`, action.status); // With inmutable.js
  }
});
```

**Step By Step**

1. The accion call the reducer always like this reducer(previousState, accion). For instance:

```javascript
currentUser(previousState, accion);
```

2. We call the factory reducers to create our user reducer so it needs a initialState and the actual reducers which is an object with methods to reduce the state (handlers).

```javascript
createReducer(null, {
  [UPDATE_STATUS](state, action) {
    return state.set(`status`, action.status);
  }
});
```

3. The factory reducers as a factory returns a reducer. But remember are also called with (previousState, accion) so:

```javascript
createReducer(null, {
  /* handlers */
})((previousState, accion));

// Explaining in more detail
export const createReducer = (initialState, handlers) => {
  //state = initialState is because needs a default value by default null
  //it will be override with previousState
  //to access the previousState and acccion we have to return the first function which ends the first execution and we can gain access to the second scope(previousState, acccion)
  return function reducer(state = initialState, action) {
    //Here we just check if the type of accion exists and then invoke it
    //If not return the same state
    if (handlers.hasOwnProperty(action.type)) {
      return handlers[action.type](state, action);
    } else {
      return state;
    }
  };
};
```

4. Return the new state

# Let's combine multiple reducers

> Redux has a utility named combineReducers, which takes an object with multiple reducers. But it has some limitations which lead us to build our own combine reducers. This is usually the actual "reducer" passed to the store.

# Why ?

> Imagine each reducer is responsable to reduce determine action. Remember we want things modular and isolate as possible.

# How it is implemented?

> Take configuration object. Always returns state

```javascript
export const combineReducers = () => (state, action) => ({
  // State is sliced for various reducers
  user: currentUser({ users: state.users }, action)
});
```

## Advantages

* Enforces best practices
* Simple and easy to understand

## Disadvantages

* Single reducer can’t work with multiple slices of state
* Does not work on non-object states
* Does not work with immutable.js-based states
* Not suitable for (most) very large projects

# Custom own reducer

```javascript
export const combineReducers = config => {
  return (state, action) => {
    return Object.keys(config).reduce((state, key) => {
      const reducer = config[key];

      const previousState = state.get(key);

      const newValue = reducer(previousState, action);

      if (!newValue) {
        throw new Error(
          `A reducer returned undefined when reducing key::"${key}"`
        );
      }
      return state.set(key, newValue);
    }, state);
  };
};

export const reducer = combineReducers({
  currentUser
  //... more reducers
});
```

**I hope it helps :)**

[Continue 4](Redux-Middleware.md)
