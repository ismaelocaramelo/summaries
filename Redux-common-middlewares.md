![IMG](https://media.giphy.com/media/l4v8p7s9Ylopq/giphy.gif)

# Thunk

> Thunk is a function which return a fucntion. This allows us to dispath a functions, mainly to perfom asynchronous actions. Also has the same power as any other middleware getting the state of the store and dispaching actions.

# How it is implemented?

```javascript
function createThunkMiddleware(extraArgument) {
  return ({ dispatch, getState }) => next => action => {
    if (typeof action === "function") {
      return action(dispatch, getState, extraArgument); // If the action it's a thunk we invoked passing those arguments (dispatch, getState, extraArgument)
    }

    return next(action); // continue with the chain
  };
}

const thunk = createThunkMiddleware();
// Storing the actual function to allow passing a parameter which will be passing to the action.
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

> Saga is kind of a long running process which waits for actions and respond them with a new actions, side effects.

# How ?

> First of all we need to inserted into the middleware chain. Sagas (processes) are started, middleware chain passed a copy of any actions dispatched.

# Why ?

> We love to look at things which are declarative and sync looking. Saga is based on generators (see [AsyncThings](AsyncThings.md) for understanding generators). To recapitulate, generators suspend the child process to yield something (api request, delay stuff...).

## Steps

1.  Middleware initialized
2.  Sagas initialized
3.  Middeware added to store
4.  Action is passed from middleware to saga
5.  Saga executes, creates side-effects
6.  Process repeats indefinitely

# Interesting

> There is times when a side effect comes truth. I mean, something that change the state outside the scope. Well, redux thunk does I nice job because it is simple as that, however no easy to test. E.g: test on promise. So remember Thunk will do the action but you will need to manually resolve it. Thanks to generators, which you can pause and resume an execution, each saga could take that for job for you.

# How Redux-saga manage the side-effects

> In a very smart way saga introduces Effects. Effects are plain objects that hold a description of the acccion. Why? Even do you still could yield async actions, effects come very handy due to you will only test if that async action is called with the right function and with the right arguments.

```javascript
// [...]
// call is one the Effect so it will transform the fecth
// into a plain obj and the middleware will do take that for you.
assert.deepEqual(
  iterator.next().value,
  call(Api.fetch, "/products"),
  "fetchProducts should yield an Effect call(Api.fetch, './products')"
);
```

> So saying that, we need to try that everything we yield is handling by a effect to avoid overengineering in a test context.

# How to build a saga ?

> Each saga will perfom an isolate task. Normally, you will have a saga which will be the watcher and another saga which will be the worker. The watcher will watch for a specific type of action. The worker will perfom the actual action (promise, delay functions... etc).

```javascript
import { takeLatest, call, put } from "redux-saga/effects";

// watcher saga: watches for actions dispatched to the store, starts worker saga
export function* watcherSaga() {
  // takeEvery is an Effect which means for every action invoke the workerSaga
  // We want the saga runs forever in the apps life
  // We could just set a true while and accept any action
  // takeEvery that's that behind the scenes, runs forever
  yield takeEvery("API_CALL_REQUEST", workerSaga);
}

// worker saga: makes the api call when watcher saga sees the action
function* workerSaga() {
  try {
    // call(fn, ...args)
    const data = yield call(fetch, "/ismaelocaramelo", {
      method: "POST",
      body: "Yoooo"
    });
    // put is another Effect, at end of the day is dispatching the action and become testable
    yield put({ type: "API_CALL_SUCCESS", data });
  } catch (error) {
    yield put({ type: "API_CALL_FAILURE", error });
  }
}
```

# Hold on, there is more!

### Take control over the actions:

> What if I just want to control the flow and avoid an infinite loop (takeEvery), allowing the generator to be garbage collected and take more control on actions. Take Effect comes to the rescue it wll suspend the Generator until a matching action is dispatched. It's like saying "ey no more actions are allowed until I completely dispatch it".

**(From redux-saga) In the case of takeEvery, the invoked tasks have no control on when they'll be called. They will be invoked again and again on each matching action. They also have no control on when to stop the observation.**

**Examples from redux saga doc**

```javascript
function* watchFirstThreeTodosCreation() {
  for (let i = 0; i < 3; i++) {
    const action = yield take("TODO_CREATED");
  }
  // After three actions of "TODO_CREATED" will dispatch and action:
  yield put({ type: "SHOW_CONGRATULATION" });
}
```

```javascript
// Ey buddy, looks syncronous!!!
function* loginFlow() {
  while (true) {
    yield take("LOGIN");
    // ... perform the login logic
    yield take("LOGOUT");
    // ... perform the logout logic
  }
}
```

### Run tasks in Parallel (Blocking way)

> As you know javascript is a single thread language, well thanks to generators we could fake and the benefits of multiple threads. To tell redux that we want to launch multiple things at the same time,we have to wrap the calls and passed to yield.

```javascript
import { all, call } from 'redux-saga/effects'

const [users, repos] = yield all([
  call(fetch, '/users'),
  call(fetch, '/repos')
])
```

# Run tasks in Parallel (Non-blocking way 1)

> Fork is useful when a saga needs to start a non-blocking task.

```javascript
function* fetchAll() {
  const task1 = yield fork(fetchResource, "users");
  const task2 = yield fork(fetchResource, "comments");
  yield call(delay, 1000);
}

function* fetchResource(resource) {
  const { data } = yield call(api.fetch, resource);
  yield put(receiveData(data));
}

function* main() {
  yield call(fetchAll);
}
```

# Run tasks in Parallel (Non-blocking way 2)

> Spawn is a non-blocking call, it differs from fork is next points:

* 'fork' creates a child from the calling process. While 'spawn' creates a new child at the root of the graph.

* When you 'fork' another process, the parent process will wait until the 'forked' process is finished. Also every exception will bubble up from the child to the parent and can be catched in the parent.

* 'spawned' process though will not block the parent, so the next 'yield' statement is called immediately. Also the parent process will not be able to catch any exceptions that happen in the 'spawned' process.

**Credits to this post [link](https://stackoverflow.com/questions/43259301/whats-the-difference-between-fork-and-spawn-in-redux-saga)**

**I really encourage you to follow the documentation as well as the example in [Redux-doc](https://redux-saga.js.org/docs/introduction/) to fully understand it. Good luck**

[Part 1](Redux-middleware.md)
