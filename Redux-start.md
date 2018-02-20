# Redux

![IMG](https://media.giphy.com/media/XxTQLNIGgI7sY/giphy.gif)

> The only source of truth, management library application-level state.

# Why ?

> As the app tends to be composable and the communication is not always directly/related seems quite overwhelming try to manage the state of the application. Redux has become a very handy solution, here it's principles:

* The state of your whole application is stored in an object tree within a single store.

* The only way to change the state is to emit an action, an object describing what happened.

* Changes are made with pure functions

Diagram: [Imgur](https://i.imgur.com/qYK0QJZ.png)


# Let's follow the diagram.

1. Create a Redux store with createStore.

```javascript
import { createStore } from 'redux'
import todoApp from './reducers'
let store = createStore(todoApp, { inistialStateVariable: "derp"})
```

2. Use connect to connect component to Redux store and pull props from store to component.
```javascript
import { connect } from 'react-redux'
const VisibleTodoList = connect( mapStateToProps, mapDispatchToProps)(TodoList)
export default VisibleTodoList
```
3. Define actions that allow your components to send messages to the Redux store.

```javascript
/*
 * action types
 */
export const ADD_TODO = 'ADD_TODO'
export function addTodo(text) { return { type: ADD_TODO, text }}
```
4. Reducers (pure functions)

```javascript
function todoApp(state = initialState, action) { 
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
    return Object.assign({}, state, { visibilityFilter: action.filter})
  default: 
    return state
  } 
}
```

# Benefits

1. Maintainability => The structure makes the code easier to maintain, because the codes tends to be consistent.

2. Real time with dev tools => Track actions and state changes in real time.




