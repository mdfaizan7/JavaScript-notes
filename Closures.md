# Closures

Closures are the bane of any JavaScript programmers. This is mostly because it is a very difficult concept to learn and its always taught poorly.

So let’s see what are closures? It’s a behavior whereby functions that refer to variables declared by parent functions, still exist. This is mainly because of scoping.

Now lets see what is actually the problem with closures:

```jsx
function foo() {
    const arr = []

    for (var i = 0; i < 5; i++) {
        arr.push(function () {
            console.log(i)
        })
    }

    return arr
}

const ar = foo()

ar[0]() // 5
```

Now in the above example, we expected the output of `0`. But the above code console logged `5`. So what happened?

So that there is one JavaScript programmer's worst enemy, because its somewhat unexpected. But in this article, we will see that this is actually expected.

So closures are basically a function that has access to some variables that might have already left scope.

So lets see another example:

```jsx
function hello() {
    const msg = 'hello'

    function sayHelloFunction() {
        console.log(msg)
    }

    return sayHelloFunction
}

const sayHello = hello()

console.log(msg) // error: msg is not defined

sayHello() // hello
```

Now lets dig a little deeper into this:

```jsx
function hello() {
    const msg = 'hello'

    function sayHelloFunction() {
        console.log(msg)
    }

    return sayHelloFunction
}

const sayHello = hello()

console.log('typeof msg: ', typeof msg)

console.log(sayHello)
```

This will give an output of:

```
typeof msg: undefined

function sayHello() {
  console.log(msg)
}
```

So this right here is a closure, because we can see the variable called `msg`, when `sayHello` is invoked, does not actually exist.

Yet, `sayHello` still has access to `msg` variable because it was invoked within scope when that function was created.

So this is what a closure is.

So coming back to our previous example (where it console logged 5 instead of 0), let’s see why this bug exists.

```jsx
function foo() {
    const arr = []

    for (var i = 0; i < 5; i++) {
        arr.push(function () {
            console.log(i)
        })
    }

    return arr
}

const ar = foo()

ar[0]() // 5

// now here if we try to console log `i` we will get an undefined, 
// because it is not in the scope.

console.log(i) // error: i is not defined
```

But as we discussed earlier, because of closures, the functions can have access to variables which they should not have.

Now lets see this again:

```jsx
function foo() {
    const arr = []

    for (var i = 0; i < 5; i++) {
        arr.push(function () {
            console.log(i)
        })
    }

    console.log(i) // 5

    return arr
}

const ar = foo()

ar[0]() // 5
```

So now as we know the lifecycle of var is till the time function ends, that's why the value of `i` persists even after the loop has ended.

And this is what is shown and accessed when we try to call the first function of the array.

So instead of using a var here, what happens when we use a `let`?

```jsx
function foo() {
    const arr = []

    for (let i = 0; i < 5; i++) {
        arr.push(function () {
            console.log(i)
        })
    }

    console.log(i) // error: i is not defined

    return arr
}

const ar = foo()

ar[0]() // 0
```

So we see that using `let` can fix our little bug there.

Now there is also an alternate way to make closures, i.e, by an IIFE or Immediately Invoked Function Expression.