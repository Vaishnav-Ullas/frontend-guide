## JavaScript Basics

Welcome to the JavaScript Basics repository! This document serves as a quick
reference guide and introduction to the core concepts of JavaScript, the
programming language of the web.

Whether you are a complete beginner or looking for a quick refresher, this
guide covers the essential building blocks you need to start coding.

### Table of Contents

- Variables
- Data Types
- Operators
- Control Flow
- Functions
- Arrays and Objects
- ES6+ Features
- How to Run

### Variables

Variables are containers for storing data values. In modern JavaScript, we
primarily use `let` and `const`.

- `var`: Function-scoped (older way, avoid using if possible)
- `let`: Block-scoped, can be reassigned
- `const`: Block-scoped, cannot be reassigned

```javascript
let userName = "Alice";
userName = "Bob"; // Allowed

const pi = 3.14159;
// pi = 3.14; // Error: Assignment to constant variable.
```

### Data Types

JavaScript variables can hold different types of data.

**1) Primitive Types**

- String: Text data (e.g., "Hello")
- Number: Integers and floating-point numbers (e.g., 42, 3.14)
- Boolean: True or false values (e.g., true, false)
- Null: Represents "nothing" or "empty" intentionally
- Undefined: A variable that has been declared but not assigned a value

**2) Reference Types**

- Objects: Key-value pairs
- Arrays: Ordered lists

```javascript
let name = "John"; // String
let age = 30; // Number
let isStudent = false; // Boolean
let car = null; // Null
let future; // Undefined
```

### Operators

Operators are used to perform operations on variables and values.

| Type | Examples | Description |
| --- | --- | --- |
| Arithmetic | `+`, `-`, `*`, `/`, `%` | Basic math operations |
| Assignment | `=`, `+=`, `-=` | Assigns values to variables |
| Comparison | `==`, `===`, `>`, `<` | Compares two values (`===` checks type and value) |
| Logical | `&&` (AND), `||` (OR), `!` (NOT) | Combines boolean expressions |

Pro tip: Always use strictly equals `===` instead of `==` to avoid unexpected
type coercion issues.

### Control Flow

Control how your code executes based on conditions.

**If / Else**

```javascript
let hour = 10;

if (hour < 12) {
  console.log("Good morning!");
} else {
  console.log("Good afternoon!");
}
```

**Loops**

```javascript
// For loop
for (let i = 0; i < 5; i++) {
  console.log("Iteration number: " + i);
}

// While loop
let i = 0;
while (i < 5) {
  console.log(i);
  i++;
}
```

### Functions

Functions are blocks of code designed to perform a particular task.

**Function Declaration**

```javascript
function greet(name) {
  return "Hello, " + name;
}

console.log(greet("Alice"));
```

**Arrow Functions (ES6)**

```javascript
const add = (a, b) => a + b;
console.log(add(5, 10)); // Output: 15
```

### Arrays and Objects

**Arrays**

```javascript
const fruits = ["Apple", "Banana", "Cherry"];

console.log(fruits[0]); // Accessing: Apple
fruits.push("Orange"); // Adding to the end
```

**Objects**

```javascript
const person = {
  firstName: "John",
  lastName: "Doe",
  age: 50,
};

console.log(person.firstName); // Dot notation
console.log(person["lastName"]); // Bracket notation
```

### ES6+ Features

Modern JavaScript (ES6 and later) introduced powerful features to write cleaner
code.

**Template Literals**

```javascript
let name = "World";
console.log(`Hello, ${name}!`);
```

**Destructuring**

```javascript
const { firstName, age } = person;
```

**Spread Operator (`...`)**

```javascript
const numbers = [1, 2, 3];
const moreNumbers = [...numbers, 4, 5];
```

### How to Run

Use any JavaScript runtime like Node.js:

```bash
node file.js
```
