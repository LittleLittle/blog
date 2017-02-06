title: Function Scope v.s. Block Scope
date: 2017-02-05 16:42:50
tags: JavaScript
---
ES2015(aka ES6) introduced a new keyword `let` that declares a new variable in block scope. Before that, most JavaScript variables are function scope or global scope.

# Function Scope
Following example shows the scope of variable `num` declared by `var`.
```javascript
(function () {
  {
    var num = 1;
    console.log(num);  // 1
  }
  console.log(num);  // 1. Same variable as defined in {}.
}());
```

# Block Scope
Following example shows the scope of variable `num` declared by `let`.
```javascript
(function () {
  {
    let num = 1;
    console.log(num);  // 1
  }  // num goes out of scope.
  console.log(num);  // ReferenceError: num is not defined.
}());
```

# Hoisting
Variables declared by `var` are hoisted.
```javascript
(function () {
  console.log(num);  // undefined
  {
    var num = 1;
    console.log(num);  // 1
  }
}());
```

Variables declared by `let` are not hoisted.
```javascript
(function () {
  console.log(num);  // ReferenceError: num is not defined.
  {
    let num = 1;
    console.log(num);  // 1
  }
}());
```

# Block Scope Before ES2015
Variables declared in the `catch` clause of `try...catch` statement is block scoped.

# Useful Links
- [You Don't Know JS: Scope & Closures](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch3.md).
- [MDN: let statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let).
- [JavaScript variables lifecycle: why let is not hoisted](https://rainsoft.io/variables-lifecycle-and-why-let-is-not-hoisted/).
