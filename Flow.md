# Flow

> Is a static type checker for javascript. Made by Facebook.

## Why Flow ?

> Easy to install, is a checker not compiler and therefore doesn´t appear as a separate language. Also you could use or not is completely independient. Typescript is also a great product but for me seems quite intrusive. Flow is extremely fast.

* Flow is designed to quickly find errors in JavaScript applications.
* Works without any type annotations.
* Very good at inferring types.
* If present, type annotations can very easily be removed by babel for runtime.

## Instalation

> [Flow Instalation](https://flow.org/en/docs/install/)

## Types

```javascript
(1234: number);
('hi': string);
(true: boolean);
([1, 2]: Array<number>);
([1, 'Hi']: Array<mixed>)(({ prop: 'value' }: Object));
(function method() {}: Function);
```

## Type aliases

```javascript
type Person = {
  name: string,
  age: number,
  isAdmin: boolean,
  likes: Array<string>
};
function greet(user: Person) {
  console.log('hello', user.name);
}
```

## Maybe types

```javascript
type Album = {
  name: ?string
};
```

## Optional properties

```javascript
type Album = {
  name?: string
};
```

## Extra object fields

```javascript
type Artist = {
  name: string,
  label: string
};
const a: Artist = {
  name: 'Miguel Migs',
  label: 'Naked Music'
};
```

## Enums

```javascript
type Suit = 'Diamonds' | 'Clubs' | 'Hearts' | 'Spades';

const countries = {
  US: 'United States',
  IT: 'Italy',
  FR: 'France'
};

type Country = $Keys<typeof countries>; // US | IT | FR
```

## Generics

> Generics are a way of abstracting a type away. Returns the of whatever type has passed (polymorphic type).

```javascript
function<T>(param: T): T {
  // ...
}
```

## Typeof

> The typeof operator returns the Flow type of a given value to be used as a type.

## Union Types

```javascript
type Action = number | string
type Direction = 'left' | 'right
```

## Importing and exporting types

```javascript
import type { MyObject } from './types';
export type MyObject = {
  /* ... */
};
```

## Utility Types

```javascript
$Keys<T>
$Values<T>
$ReadOnly<T>
$Exact<T>
```

## Adding Flow in real proyect

> Add a file for types and named **_types.flow.js_** this is where your export all the types of functions, objects literals, arrays etc...

> Ignore dist/.\* in the .flowconfig does means it will not check in the dist folder which

## More info: RTFM

> > [Flow](https://flow.org/en/docs/)
