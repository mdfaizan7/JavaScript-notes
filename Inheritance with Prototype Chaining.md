# Inheritance with Prototype Chaining

## Inheriting Properties

---

- JavaScript Objects have their own dynamic properties, and have link to the prototype object.
- The property of an object in JavaScript is not only available on that object, but also to the prototype of that object, the prototype of a prototype, and so on until a name is reached which is similar to the name of the property in that object.

Let us see some examples to understand better:

```jsx
let foo = function() {
    this.a = 2
    this.b = 4
}

// let us make a new object from  this function
let obj = new foo() // {a: 2, b: 4}

// lets add some properties to the function foo prototype.
foo.prototype.c = 6

console.log(obj.c) // 6
// Is there a property c on object obj? No, check it's prototype 
// Is there a property c on obj.[[prototype]]? Yes

foo.prototype.a = 10

console.log(obj.a) // 2
// Is there a property a on object obj? Yes

console.log(obj.d) // undefined
// Is there a property d on object obj? No, check it's prototype 
// Is there a property d on obj.[[prototype]]? No, check it's prototype
// Is there a property d on obj.[[prototype]].[[Prototype]]? No,check it's prototype
// obj.[[prototype]].[[prototype]].[[prototype]] is null
// no property found, return undefined
```

## Inheriting Methods

---

- JavaScript does not have methods like other programming languages like Java does.
- Here, any function/method can be added as a property into any object.
- Any inherited function acts like just any other property that the object has.
- When an inherited function is executed, the value of `this` points to the inheriting object, not to the prototype object where the function is an own property.

```jsx
var obj = {
  a: 2,
  b: function() {
    return this.a*2
  }
}

console.log(obj.b()) // 4
// When calling obj.b() in this case, 'this' refers to obj

var p = Object.create(obj)
// p is an object that inherits from obj

p.a = 4 // creates a property 'a' on p
console.log(p.b()) // 5
// when p.b is called, 'this' refers to p.
// So when p inherits the function b of obj, 
// 'this.a' means p.a, the property 'a' of p
```

## Prototypes in JavaScript

---

- In JavaScript, functions are able to have properties. All functions have a special default property named `prototype`.
- There is just one exception, the arrow functions does not have a default property.

```jsx
function hello(){}

console.log(hello.prototype)
```

As seen above, the function `hello` have a property named prototype.

After running this command, the console should give an output like this:

```jsx
{
    constructor: ƒ hello(),
    __proto__: {
        constructor: ƒ Object(),
        hasOwnProperty: ƒ hasOwnProperty(),
        isPrototypeOf: ƒ isPrototypeOf(),
        propertyIsEnumerable: ƒ propertyIsEnumerable(),
        toLocaleString: ƒ toLocaleString(),
        toString: ƒ toString(),
        valueOf: ƒ valueOf()
    }
}
```

We can also add properties to the prototype of `hello`:

```jsx
function hello(){}

hello.prototype.a = 10

console.log(hello.prototype)
```

This will result in:

```jsx
{
    a: 10,
    constructor: ƒ doSomething(),
    __proto__: {
        constructor: ƒ Object(),
        hasOwnProperty: ƒ hasOwnProperty(),
        isPrototypeOf: ƒ isPrototypeOf(),
        propertyIsEnumerable: ƒ propertyIsEnumerable(),
        toLocaleString: ƒ toLocaleString(),
        toString: ƒ toString(),
        valueOf: ƒ valueOf()
    }
}
```

We can now use the `new` keyword to make an instance of the `hello` function:

```jsx
function hello(){}
hello.prototype.a = 10

let helloInstance = new hello()
helloInstance.say = 'hello'

console.log(helloInstance)
```

This will result in:

```jsx
{
    say: "hello",
    __proto__: {
        a: 10,
        constructor: ƒ doSomething(),
        __proto__: {
            constructor: ƒ Object(),
            hasOwnProperty: ƒ hasOwnProperty(),
            isPrototypeOf: ƒ isPrototypeOf(),
            propertyIsEnumerable: ƒ propertyIsEnumerable(),
            toLocaleString: ƒ toLocaleString(),
            toString: ƒ toString(),
            valueOf: ƒ valueOf()
        }
    }
}
```

We notice that the `__proto__` of `helloInstance` is `hello.prototype`. But how does this work? 

Lets see this:

1. When you try to access some property of `helloInstance`, the browser first looks if the `helloInstace` have that property or not. 
2. If the property is not in `helloInstance`, then the browser will look into the property `__proto__` of `helloInstance`. If the property is found here, then this value is used.
3. If the `__proto__` of `helloInstance` does not have this prototype, then the property is looked into the `__proto__` of the `__proto__` of the `helloInstance`.
4. *Iff,* the whole prototype chain is looked through, and we do not have any more `__proto__` to look through, then only the browser says that there is no such property and returns `undefined`.

## Different Ways to Create Object & Prototype Chain

---

### Objects Created With Syntax Constructs:

```jsx
var obj = {a: 1}
// The prototype chain looks like:
// obj ---> Object.prototype ---> null

var b = ['yo', 'whadup', '?']
// Arrays inherit from Array.prototype 
// The prototype chain looks like:
// b ---> Array.prototype ---> Object.prototype ---> null

function foo() {
  return 2
}
// Functions inherit from Function.prototype 
// foo ---> Function.prototype ---> Object.prototype ---> null
```

### With a Constructor:

```jsx
function Graph() {
  this.vertices = []
  this.edges = []
}

Graph.prototype = {
  addVertex: function(v) {
    this.vertices.push(v)
  }
};

var g = new Graph()
```

### With `Object.create()` method:

[Understanding the difference between Object.create() and new SomeFunction()](https://stackoverflow.com/questions/4166616/understanding-the-difference-between-object-create-and-new-somefunction)

ℹ️

```jsx
var a = {a: 1}
// a ---> Object.prototype ---> null

var b = Object.create(a)
// b ---> a ---> Object.prototype ---> null
console.log(b.a) // 1 (inherited)

var c = Object.create(b)
// c ---> b ---> a ---> Object.prototype ---> null

var d = Object.create(null)
// d ---> null
```

### With `class` keyword:

```jsx
class Polygon {
  constructor(height, width) {
    this.height = height
    this.width = width
  }
}

class Square extends Polygon {
  constructor(sideLength) {
    super(sideLength, sideLength)
  }

  get area() {
    return this.height * this.width
  }

  set sideLength(newLength) {
    this.height = newLength
    this.width = newLength
  }
}

var square = new Square(2)

console.log(square.area) // 4
square.sideLength = 3
console.log(square.area) // 9
```