# Primitive values

* Boolean true or false
* Number Any integer or floating-point numeric value
* String A character or sequence of characters delimited
  by either single or double quotes
* Null A primitive type that has only one value, null
* Undefined Is the value assigned to a variable that is not
  initialized

> Key points

* When you assign a primitive value to a variable, the value is
  copied into that variable.
* null is an empty object pointer, making "object" a logical
  return value. WEIRD but usefull in the future.
* The best way to identify primitive types is with the typeof operator.
* strings, numbers, and Booleans actually have methods. Although they are PRIMITIVE VALUES.
  # Reference Types
* known as objects which is a list of name/value pairs, Functions are objects which you can call.

> Constructing objects

* new Object() | new _Built-in-type_() | new _Constructor-name_()
* {}
  > Built-in Types
* Array An ordered list of numerically indexed values
* Date A date and time
* Error A runtime error
* Function A function
* Object A generic object
* RegExp A regular expression
  > Types of properties
* Data properties => contain the value
* Accessor properties => doesn´t contain the value. Follows how memory works by using getters and setters.
  If you define only a getter, then the property becomes read-only, and attempts to write to it will fail silently in nonstrict mode and throw an error in strict mode. If you define only a setter, then the property becomes write-only,and attempts to read the value will fail silently in both strict and nonstrict modes.

> Data Property Attributes

* Every property has its own attibutres internally and we can change the default behavior. By default all of the booleans are true and the value is set it automatically.
  * [[Enumerable]] => holds a boolean to make it or not iterable.
  * [[Configurable]] => holds a boolean. If false, you cannot delete a property. controls the writability of a property’s meta-data.
  * [[writable]] => (ONLY IN DATA PROPERTIES) => holds a boolean indicating whether the value of a property can be changed.
  * [[value]] => (ONLY IN DATA PROPERTIES) hold the property’s value, its data.
  * **proto**: null => not accepting inherit properties.
  * value: 'static' => all default properties to false.
    > Custom property(ies)
* defineProperty => This method is not inherit by the constructor therefore we can use it by "Object.defineProperty()". This method accepts three arguments: the object that owns the property, the property name, and a property descriptor object containing the attributes to set.

* defineProperties => This method is not inherit by the constructor therefore we can use it by "Object.defineProperties()". the object to work on and an object containing all of the property information. The keys of that second argument are property names, and the values are descriptor objects defining the
  attributes for those properties.

_Usefull methods when working with custom properties_ =>

* Object.getOwnPropertyDescriptor(object, "property to look for")

> Preventing Object Modification

* Every object has the [[Extensible]] property, by default is true. Of course we can change it.

* Three ways to accomplish this:
  _ Object.preventExtensions() => This method accepts a single argument, which is the object you want to make nonextensible.you’ll never be able to add any new properties to it again.
  _ Object.seal() => This method accepts a single argument,a sealed object is nonextensible, and all of its properties are nonconfigurable. \* Object.freeze => This method accepts a single argument, you can’t add or remove properties, you can’t change properties types, and you can’t write to any data properties. For deep freeze (nested objects) consider a custom function.
  > Key points
* Holds a pointer (or reference) to the location in memory where the object exists. Means it only exist once in MEMORY while the primitives are free moving the address of where they´re live.
* JavaScript is a garbage-collected language, so you don’t really need to
  worry about memory allocations when you use reference types. But setting objet to null helps to free up that memory space.
* Javascript allows to modify the object whereever you wish but try to avoid mutations unless you´ve got a reason for that.
* The best way to identify reference types is by using instanceof because as every object is a pointer typeof could sometimes may no be explicity.
* Identifying Arrays as each web page has its own global scope, instanceof dosen´t work because it references an instance from another frame. For arrays use Array.isArray(_yourArray_);
* The best way to detect properties in most cases is using the _**in**_ operator, but remember as object goes by reference, javascript engine will search in all the references instances of that object in something call _prototype chaining_ or _prototypal inheritance_ until it finds it. If not, will return undefined. Having said that there is an inherit method in every object hasOwnProperty() passing the property to look for in that specific object, returns true if it exits.

# Special reference type: Functions

* Functions are objects which you can call. Has internally [[call]] property.
  > Constructing
* Declarations => function nameOfFuntion(){}
* Expression => stored in variable.
  * Diferences: - Function declarations are hoisted to the top of the context when the
    code is executed. Means the function is known ahead of time. - Function expressions are NOT hoisted. Declare first, use later.

> Change this

* Functions can be used in many different contexts, and they need to be able to work in each situation.
* Call() method.
  Call that function with this context and these parameters.
  ```javascript
   thatFunction.call(object/function/built-in-type, parameters)
  ```
* Apply() method.
  Same as call method but you could pass an array of parameters.
  ```javascript
    thatFunction.apply(object/function/built-in-type, ["parameter1", ....])
  ```
* Bind() method.
  The only diference is it will set permantly the same this as you bounded.

> Key points

* Parameters => Are actually stored as an array-like structure
  called arguments. But they are NOT an instance of Array.
* When a function executes, it gets the _this_ property. Do not overthink this :D

# Constructors and Prototypes

* A constructor is a simple function that is used with new to create an object. There is several built-in constructors such Object, Array, and Function. But, of course, you could create your own constructor.
* The new operator automatically creates an object of the given type and
  returns it.
  > Key points
* Use instanceof to check what type of instance
* Constructors allow you to configure object instances with the same
  properties. You could just define properties e.g Object.defineProperty().
* Almost every new instance/object has a prototype ("prototype inheratence"). Which is share throught all the instances of that constructor.
* To indentify a prototype property consider using a custom function.
* As the prototype property contains a pointer any changes to the prototype are immediately available on any instance referencing it.

# Inheritance

* The concept of prototype chaining or prototypal inheritance. Prototype properties are automatically available on object instances.

> Methods Inherited from Object.prototype

* hasOwnProperty() Determines whether an own property with the given name exists
* propertyIsEnumerable() Determines whether an own property is enumerable
* isPrototypeOf() Determines whether the object is the prototype of another
* valueOf() Returns the value representation of the object. Method gets called whenever an operator is used on an object.
* toString() Returns a string representation of the object. The toString() method is called as a fallback whenever valueOf() returns a reference value instead of a primitive value.
  > Constructor Stealing
* The situation context will be two diferent contructors. To use methods and declarations defined in the constructor you want to still from you just need to change the context of this with call() or apply() in the constructor stoler.
  > Key points
* If you do define a valueOf() method, keep in mind that you’re not changing how the operator works, only what value is used with the operator’s default behavior.
  > Constructor Inheritance
* Sometimes you want to share a prototype with another constructor. However seeting the prototype of one object as a new instance of the second constructor cause side-effects such as changing the prototype of the second constructor prototype or adding extra weigth to it. Therefore, is better to stablish an object in the middle which is a copy of the second construtor proto and we can then change the proto of the first constructor. By using Object.create() and re-stablishing the constructor because when you change the proto, the constructor has been overwritten. E.g:
  `javascript Object.create(secondConstructor.proto, { constructor: { configurable: true, enumerable: true, value: firstConstructor, writable: true} })`
  PD: I did not include patterns due to its complexity must be in another summary.
  > Book: The Principles of Object-Oriented JavaScript,
  > I highly recommend to read this book as contains detail explanations and easy examples to go along with.
