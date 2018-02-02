## Javascript the unknow world

> Javascript is sync but can do async operations. As a single-thred language only one line of code can be executed at any given time. This could a limitation in some scenarios but hopefully there is some patterns for writing asynchronous code. Most

## Callback

> Hell. Start point. When this is done, execute this thing. Callbacks are so powerfull feature but it can be tedious.

```javascript
function one() {
  setTimeout(function() {
    console.log("A");
    setTimeout(function() {
      console.log("B");
      setTimeout(function() {
        console.log("C");
        setTimeout(function() {
          console.log("D");
        }, 2000);
      }, 2000);
    }, 2000);
  }, 2000);
}
```

## Promises

> Meh. Base on callbacks but composable, so we can chain the result AWESOME. To understand promises imagine if you want to order a special drink in a bar, first you ask for it then, they tell you to wait for it (pending), it may take a while and it could have two endings they did it (resolve) they told you run out of it (reject)

```javascript
const areWePoor = false;

const promiseADrink = new Promise(function(resolve, reject) {
  if (!areWePoor) {
    resolve("Here you are");
  } else {
    reject(new Error("Sorry bro"));
  }
});
promiseADrink()
  .then(function(result) {
    // Do something with the result
  })
  // We could keep chaining the result .then(previous result)
  .catch(function(error) {
    // Handle error
  });
```

## Genarators

> Generators are functions that works as a factory for iterators, means that it return a new Generator Object. So that lead that these functions mantain their own state. The object returned is itereble and has a property:

* next => A function which gives you and object containing the value and the status of the function. The value is the one yield inside the function and the status is a boolean false if the iteration is still have some values to yield. The status is true otherwise, and by the way the value is undefined because there is no more yield values.

> It's so powerfull make it look like sync, yield is the stop thing that literally pause the function do whatever you do and then come back at the same point and yield the result. HOWEVER is still weird to write.

```javascript
function promiseADrink() {
  return new Promise(function(resolve, reject) {
    resolve("Anyway give him!"); //Asume we're always resolve
  });
}

// Lets create the generator
function* drinkGenerator() {
  // Handle the promise
  try {
    var answer = yield promiseADrink(); // stop and yield with that
    console.log(answer);
  } catch (e) {
    console.log(e);
  }
}

// Lets run the generator

function runDrinkGenerator(drinkGenerator) {
  const instance = drinkGenerator();
  const result = instance.next(); // A promise !!
  function handleResult(result) {
    if (result.done) return result.value;

    return result.value.then(
      function(value) {
        return handleResult(instance.next(value));
      },
      function(error) {
        return handleResult(instance.throw(error));
      }
    );
  }
}
```

> Weird as nothing. Hopefully they come up with some verbose stuff.

## Async / await

> Is the next generator of generators xD. It has the same purpose for controlling asynchronous flow-control in JavaScript. A function that return a Promise, what a surprise. It very syntactic sugar, useful to deal with promises. The execution of the async function is paused until the promise is resolve.

```javascript
function promiseADrink() {
  return new Promise(function(resolve, reject) {
    resolve("Anyway give him!"); //Asume we're always resolve
  });
}

async function askDrink() {
  try {
    const result = await promiseADrink();
    return result;
  } catch (err) {
    return throw err;
  }
}

askDrink();
```

![IMG](https://media.giphy.com/media/l44QCLYsKeedfN5ni/giphy.gif)
