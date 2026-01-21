## Closure

### What is a Closure?

A closure is a feature where an inner function "remembers" and retains access to
variables from its outer (enclosing) function, even after that outer function
has finished executing.

### Key Characteristics

- Lexical scoping: Access to data is defined by where a function is written, not
  where it is called.
- Memory persistence: The inner function keeps references to outer variables, so
  they are not garbage collected.
- Primary use: Data encapsulation and state persistence.

```javascript
function func1() {
  const  count = 20;

  function func2() {
    return count;
  };

  return func2()
}

const counter = func1();
console.log(func1()); // 20
console.log(func1()); // 20
```

### Common Use Cases

**1) Data encapsulation (private variables)**

```javascript
function createUser() {
  let password = "secret";

  return {
    checkPassword(input) {
      return input === password;
    },
    changePassword(newPassword) {
      password = newPassword;
    },
  };
}

const user = createUser();
console.log(user.checkPassword("secret")); // true
```

**2) State persistence (counter)**

```javascript
function makeCounter() {
  let count = 0;

  return function () {
    count += 1;
    return count;
  };
}

const counter = makeCounter();
console.log(counter()); // 1
console.log(counter()); // 2
```

**3) Function factory (customized functions)**

```javascript
function makeMultiplier(factor) {
  return function (value) {
    return value * factor;
  };
}

const double = makeMultiplier(2);
console.log(double(5)); // 10
```

**4) Memoization (cache results)**

```javascript
function memoize(fn) {
  const cache = {};

  return function (value) {
    if (cache[value] !== undefined) {
      return cache[value];
    }
    const result = fn(value);
    cache[value] = result;
    return result;
  };
}

const square = memoize((n) => n * n);
console.log(square(4)); // 16
console.log(square(4)); // 16 (from cache)
```

**5) Async callbacks (keep outer variables)**

```javascript
function delayedMessage(message, delayMs) {
  setTimeout(function () {
    console.log(message);
  }, delayMs);
}

delayedMessage("Hello after 1s", 1000);
```

**6) Run a function only once**

```javascript
function once(fn) {
  let called = false;
  let result;

  return function (...args) {
    if (!called) {
      called = true;
      result = fn(...args);
    }
    return result;
  };
}

const init = once(() => "initialized");
console.log(init()); // "initialized"
console.log(init()); // "initialized"
```
