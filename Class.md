## Class in Js

A class is just a funtion which is an object than you can call. Nothing more, nothing less than syntactical sugar prototype inheritance.

```javascript
class Human {}
typeof Human; // "function"
```

## Classical Inheritance

> There is couple of ways:

```javascript
function Human(name, age) {
  // construct every instance passing with these properties
  this.name = name;
  this.age = age;
}

// Adding methods to the prototype so any instance could access it

Human.prototype.speak = function() {
  alert("My name is" + this.name);
};

const human1 = new Person("Tomas", "12");

// Invoke methods like this
human1.speak();
```

```javascript
class Human {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  speak() {
    alert("My name is" + this.name);
  }
}

const human1 = new Person("Tomas", "12");

human1.speak();
```

## Extend a subclass

> Each represent a different way to achieve the same:

```javascript
class Author extends Human {
  constructor(name, books) {
    super(name);
    this.books = books;
  }
}
```

```javascript
function Author(name, books) {
  Human.call(this, name);
  this.books = books;
}

//Make author class inherit from Human
Author.prototype = new Human();
//When you change the prototype to be an instance, the constructor refers to that Object. So we have to manually set it back to be the this subclass.
Author.prototype.constructor = Author;
```

```javascript
function extend(subClass, superClass) {
  var F = function() {};
  F.prototype = superClass.prototype;
  subClass.prototype = new F();
  subClass.prototype.constructor = subClass;
}

function Author(name, books) {
  Human.call(this, name);
  this.books = books;
}

extend(Author, Human);
```

## Static methods

> Static methods are often used to create utility functions. They cannot be called from the instances.So they operate on the class-level instead of the instance-level.

```javascript
class Author extends Human {
  constructor(name, books) {
    super(name);
    this.books = books;
  }
  static getBooks() {
    return ...this.books;
  }
}

const person1 = new Author('Tomas', ['El libro de la selva', 'Juana de Arco'])

person1.getBooks(); // Error
Author.getBooks(); // Acierto / sucess
```

```javascript
class Author extends Human {
  constructor(name, books) {
    super(name);
    this.books = books;
  }
}

Object.defineProperty(Author, "getBooks", {
  value: function() {
    return this.books;
  },
  writable: false,
  enumerable: true,
  configurable: false
});
const person1 = new Author("Tomas", ["El libro de la selva", "Juana de Arco"]);

person1.getBooks(); // Error getBooks is not a function
Author.getBooks(); // Acierto / sucess
```

## Prototypal Inheritance && Composition

> JavaScript’s base on prototypes not classes. Classes create Hierarchical class taxonomies which lead us to serial drawbacks:

* The tight coupling problem
* Inflexible hierarchy problem
* The duplication by necessity problem
* The Gorilla/banana problem. You want the banana but isntead you've got the gorilla holding the banana within the jaile.

## How we could achieve it using Prot. Inheritance

> Composition and Pure functions

```javascript
const sayMyName = state => ({
  speak: () => alert("My name is" + state.name)
});

const Human = (name, age) => {
  let state = {
    name,
    age
  };
  return Object.assing({}, state, sayMyName(state));
};
const Author = (name, books) => {
  let state = {
    name,
    books
  };
  return Object.assign({}, state, sayMyName(state));
};
```

> Composition has less tight coupling than Classes, using the factory pattern. All of the most popular libraries and frameworks for JavaScript make heavy use of factory functions.
