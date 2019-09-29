---
title: "ES2018 Best Practices (1 of 6)"
path: "/es2018-best-practices-1-of-6"
date: "2019-09-28"
coverImage: "../images/es2018.png"
excerpt: "Unlock the power of modern JavaScript."
tags: ["es2018 series", "javascript", "best practices"]
---

<center style="margin-bottom: 70px;">
Credit: <a href="https://css-tricks.com/wp-content/uploads/2018/12/es2018.png">css-tricks.com</a>
</center>

*This [series of articles](/tag/es2018-series/) will introduce you to ES2018 powerful features as well as how and why we should use them instead of the traditional JavaScript.*

## What is ES2018?
> *ECMAScript (ES) is a scripting-language specification standardized by Ecma International in ECMA-262 and ISO/IEC 16262. It was created to standardize JavaScript, so as to foster multiple independent implementations.* - [Wikipedia](https://en.wikipedia.org/wiki/ECMAScript)

In another word, ECMAScript is a standard that all implementations of JavaScript must satisfy. ES2018 is the version of that standard that was finalized in June 2018. ES2018 with features such as arrow function, spread operator, async/await, etc. is a much needed improvement to the traditionally confusing JavaScript.

## const
`const` is the keyword to declare **local** constant variables i.e. <span class="underline">a variable that cannot refer to a new memory address after declared</span>.

```js
const constantString = "This string cannot be modified.";
constantString = "Assigning new string to constant variable causes TypeError";
// Uncaught TypeError: Assignment to constant variable.

const httpRequest = {
  protocol: 'https',
  url: 'example.com',
  method: 'post',
  data: {
    username: 'admin',
   password: '12345678',
  },
};

// Object is a mutable data structure.
// Here, "httpRequest" is still refering to the same object
// even though its content is modified.
httpRequest.url = 'google.com'; // valid

// Array is a mutable data structure.
// Here, "fruits" is still refering to the same array
// even though its content is modified.
const fruits = ['banana', 'orange', 'apple'];
fruits.push('peach'); // valid
```

## let
`let` is the keyword to declare **local** variables.

```js
let modifiableString = 'will change later';
modifiableString = 'changing without errors';

const double = num => (num * 2);
let list = [0, 1, 2, 3, 4, 5];

list = list.map(double); // valid even though Array.map returns new array.
```

## Code Predictability
Code predictability refers to **the ability to predict the outcome of a piece of code while reading it**. The old way of declaring variables with `var` causes confusion and reduces code predictability by applying 2 things:
- Hoisting
- Function Scope

### Hoisting
> *Hoisting is JavaScript's default behavior of moving declarations to the top.*

```js
// What we write
console.log(foo);
var foo = 'string';

// What JavaScript sees
var foo = undefined;
console.log(foo);
foo = 'string';
```

When encountering variable `foo` in the above example, we expect an error because `foo` does not exist in our environment. Yet, JavaScript implicitly moves the declaration to the top of the scope. The code executes normally and `foo` is assigned a value that is not explicitly stated in the code. **Hoisting prevents us from predict the exact output of our code.**

Variables are not hoisted when declared with `const` and `let`. Trying to use a variable before it is declared will throw `ReferenceError`. Therefore, code predictability is maintained.

### Block Scope vs Function Scope
One important distinction between `const` & `let` and `var` is that `const` & `let`-declared variables are scoped by blocks while the ones declared with `var` are scoped by functions.

```js
const nums = [0, 1, 2, 3, 4, 5, 6];
const oddNums = [];

for (var num of nums) {
 if (num % 2 === 1) {
    oddNums.push(num);
  }
}

console.log(num); // 6
// oddNums = [1, 3, 5]
```

In this example, `num` can be accessed outside of the block scope created by `for...of`. This is because we declare `var num`. The variable `num` is attached to the closest function-scoped environment or global in this case.

If we were to declare `let num`, `num` is attached to the block scope created by the foor loop. Therefore, it cannot be accessed outside of the for loop. This "block scope" feature has 2 benefits:
- **Modularization**

  Variables are declared and destroyed on the same location. Not only this makes the data flow easier to follow but also avoids name collision and over-population of the global scope.
- **Name Reusability**

  We can reuse the same variable name for different block scopes without side effects.

## Conclusion
Using `const` and `let` should be the only way to declare variables if your interpreter understands ES2018. Personally, I have not written any line with `var` for a while and I do not intend to. Stick to the best practice to utilize the benefits of block scope and avoid unpredictability in your code.
