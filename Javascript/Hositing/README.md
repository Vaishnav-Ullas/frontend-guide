## Hoisting

Hoisting is JavaScript's behavior of moving variable declarations to the top of
their scope at runtime. Only the declaration is lifted (the initialization stays
in place), which can lead to `undefined` values if you access a variable before
its assignment.

It happens during JavaScript's compilation phase, before the code is executed.
The engine scans the scope, creates bindings for `var`, `let`, and `const`, and
sets them up in memory. `var` is initialized to `undefined`, while `let`/`const`
exist but are not initialized, so they are in the temporal dead zone until their
declaration is evaluated.
### Variable Hoisting


```javascript

console.log(title); //ReferenceError: title is not defined

```

In JavaScript, variable declarations using `var` are hoisted to the top of their scope. This means that the declaration is processed before any code is executed, but the assignment (initialization) happens in place. That’s why, in the following example:

```javascript
console.log(title); // undefined

var title = "Hoisting";
```

Here is how the code is treated by the engine:

```javascript
var title; // declaration is hoisted
console.log(title); // undefined
title = "Hoisting"; // assignment stays in place
```

Here’s what happens:
- The declaration `var title` is hoisted to the top, so the variable `title` exists before any code runs, but its value is `undefined` until the assignment line is reached.
- When `console.log(title)` is executed, `title` exists but hasn't been assigned `"Hoisting"` yet, so it logs `undefined`.

This illustrates variable hoisting: only the declaration, not the assignment, is moved to the top.

When you use `let` or `const`, the declaration is still hoisted, but it is not
initialized. The variable exists in a "temporal dead zone" from the start of the
block until its declaration line runs, so accessing it before that point throws a
`ReferenceError` instead of returning `undefined`. This behavior is intentional to
catch mistakes early and avoid bugs from using variables before they are ready.

```javascript
console.log(name); // ReferenceError 

let name = "Hoisting";
```




What happens when the same variable name is used inside and outside a function?

If you declare a `var` with the same name inside a function, it is hoisted to the
top of that function and shadows the outer variable. That means the inner `size`
exists as `undefined` before the `if` check runs.

```javascript
var defaultColor = "Blue";
var color = "Red";

function pickColor() {
  if (!color) {
    var color = defaultColor;
  }
  return color;
}

console.log("Picked color is " + pickColor());
console.log("Your color is " + color);
```

Here is how the code is treated by the engine:

```javascript
// How the engine sees it after hoisting:
var defaultColor, color;

defaultColor = "Blue";
color = "Red";

function pickColor() {
  var color; // hoisted declaration (function-scoped)
  if (!color) {
    color = defaultColor;
  }
  return color;
}

console.log("Picked color is " + pickColor());
console.log("Your color is " + color);
```

What happens:
- A key point about `var` is that it is function-scoped. This means a variable declared with `var` is only visible within the function in which it is defined (or globally if declared outside any function), not just within the nearest block like an `if` statement or loop.
- In the example above, the `var color` inside `pickColor` is scoped to the entire function. This is why it shadows the outer `color` for all code within the function.
- Inside `pickColor`, `var color` is hoisted, so `color` starts as `undefined`.
- The function returns `"Blue"`, but the global `color` is still `"Red"`.

### Function Hoisting

Like variables, Function declarations are fully hoisted. This means the entire function body is
available before the line where it appears, so you can call it earlier in the
same scope.

```javascript
sayHello(); // "Hello from a hoisted function"

function sayHello() {
  console.log("Hello from a hoisted function");
}
```

Here is how the code is treated by the engine:

```javascript
function sayHello() {
  console.log("Hello from a hoisted function");
}

sayHello(); // "Hello from a hoisted function"
```

Function expressions are not hoisted the same way. Only the variable
declaration is hoisted, so calling it before assignment results in an error.

```javascript
sayGoodbye(); // TypeError: sayGoodbye is not a function

var sayGoodbye = function () {
  console.log("Goodbye");
};
```

Here is how the code is treated by the engine:

```javascript
var sayGoodbye; // declaration is hoisted

sayGoodbye(); // TypeError: sayGoodbye is not a function

sayGoodbye = function () {
  console.log("Goodbye");
};
```

The `TypeError` happens because, with function expressions assigned to `var`, only the variable declaration (`var sayGoodbye`) is hoisted to the top, 
not the function definition itself. When the engine processes the code, `sayGoodbye` exists but is set to `undefined` until the assignment happens. 
So, when you try to call `sayGoodbye()` before the assignment, you are essentially doing `undefined()`, which throws a `TypeError` because `undefined` is not a function.

What happens when you have the same name for variable and functions?

When a variable and a function declaration use the same name in the same scope, both are hoisted, but the function declaration takes priority in the initial hoisting phase. However, if the variable is assigned a value later, it will overwrite the function.

Consider this example:

```javascript
console.log(foo); // [Function: foo]

function foo() {
  console.log("I am the function");
}

var foo = 42;

console.log(foo); // 42
```

How the JavaScript engine interprets the code after hoisting:

```javascript
function foo() {
  console.log("I am the function");
}
var foo;

console.log(foo); // [Function: foo]

foo = 42;

console.log(foo); // 42
```

**Explanation:**
- The function declaration (`function foo`) and the variable declaration (`var foo`) are both hoisted.
- The function declaration is hoisted before the variable declaration and assigned to `foo`.
- The variable declaration does **not** overwrite the function, but any assignment to `foo` during execution will.
- Before assignment, `foo` refers to the function. After `foo = 42;`, it refers to the number `42`.



