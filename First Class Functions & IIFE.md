# First Class Functions & IIFE

- In Programming, a function is said to be a **First Class Function** when that function is treated like any other variable.
- In a language that supports First Class Functions, a function can be passed as an argument, can be returned by a function, and can be assigned to a variable.

## Examples

I would recommend that you open the console of your browser and try to run the code yourself and see what happens. Maybe you could also experiment with it 😉

### Assign a function to a variable:

```jsx
const foo = function() {
  console.log("function foo called")
}

foo() // function foo called
```

- Even if you assign a name to the function, the function can still be called with the variable assigned.
- This is quite helpful while debugging the code.
- This does not effect the way the function is invoked.

### Passing a function as argument:

```jsx
function sayHello() {
   return "Hello, "
}

function greeting(helloMessage, name) {
  console.log(helloMessage() + name)
}

greeting(sayHello, "JavaScript!") // Hello, JavaScript!
```

- Here we are treating the `sayHello` function as an argument and passing it in the `greeting` function.
- The function that is passed as an argument is known as a **Callback function**. Here, `sayHello` is a **Callback function.**

### Returning a function:

```jsx
function greeting(name) {
  return function() {
    console.log("Hello, " + name)
  }
}

const greet = greeting("JavaScript!") 

greet()              // Hello, JavaScript!
greeting()()         // Hello, JavaScript!
```

- A function that returns a function is known as a **Higher Order function**. Here, the function `greeting` is a **Higher Order function.**

# IIFE (Immediately Invoked Function Expression)

- IIFE's are also known as Self Executing Anonymous Functions.
- IIFE are JavaScript functions that are invoked as soon as they are defined.

```jsx
(function() {
  ...statements
})()

// structure of an iife
```

IFFE contans two major parts:

1. The first part is an anonymous function which is enclosed within a set of parentheses or gouping operator `()`.
2. The second part contains the immidieatly invoked function expression `()`.

### Some Examples:

The function becomes a function expression that is immediately executed. The variable within the expression can not be accessed from outside it.

```jsx
(function () {
    var aName = "Barry";
})();
// Variable aName is not accessible from the outside scope
aName // throws "Uncaught ReferenceError: aName is not defined"
```

Assigning the IIFE to a variable stores the function's return value, not the function definition itself.

```jsx
var result = (function () {
    var name = "Barry"; 
    return name; 
})(); 
// Immediately creates the output: 
result; // "Barry"
```

---

Some Useful Articles/Links

[Why use an IFFE closure over a regular closure?](https://stackoverflow.com/questions/50979943/why-use-an-iffe-closure-over-a-regular-closure)

[JavaScript Weekly: An Introduction to First-Class Functions](https://medium.com/launch-school/javascript-weekly-an-introduction-to-first-class-functions-9d069e6fb137)

[First-class Function](https://developer.mozilla.org/en-US/docs/Glossary/First-class_Function)

[JavaScript, ES6 - Lecture 1 - CS50's Mobile App Development with React Native](https://www.youtube.com/watch?v=3Ay2lS6tm4M&list=PLhQjrBD2T382gdfveyad09Ierl_3Jh_wR&t=14s)

[Benefit of Immediately-invoked function expression (IIFE) over a normal function](https://stackoverflow.com/questions/37021349/benefit-of-immediately-invoked-function-expression-iife-over-a-normal-function)

---