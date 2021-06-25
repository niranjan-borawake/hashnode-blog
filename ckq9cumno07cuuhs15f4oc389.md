## Functional Programming using JavaScript

Programming languages are classified in a variety of ways. One method for classifying languages based on their characteristics is the programming paradigm. Let's look at a couple of paradigms that are relevant to the current topic.

**Imperative **is where a programmer instructs a machine how to change its state. Procedural and Object-Oriented are both derived from imperative programming.

**Declarative **is where the programmer merely declares properties of the result and nothing about how to compute it. Functional programming (FP) falls under the declarative programming paradigm. In FP the desired result is **declared** as the value of series of **function** applications.

Let's look at the fundamental distinctions between procedural, object-oriented, and functional programming.


### Procedural Programming

can be thought of as assembly lines.
> Raw Input → Step 1 → Step 2 → … → Step n → Final Product.

* Each step changes (transforms/mutates) the raw material in one specific way.

* Each step depends on the previous step.

* An individual step in isolation is useless.

* We only get one specific product. If we need a slightly different product then we will need a different assembly line.

To put it in programming terms, the global mutable state (raw material changing at every step) creates interdependencies (between each step).
> Note: Global state is less of a problem but the mutable state is.

### Object-Oriented Programming

does the same thing that procedural does but with some modularisation. The global state is broken down into objects and only part of instructions can change part of the state. Albeit, interdependencies still exist.

### Functional Programming

takes a completely different approach. Here programs are constructed by applying and **composing** functions. Smaller functions can be used to compose larger functions. We try to bind everything in a **purely** mathematical function style. Using pure functions instead of procedures avoids a mutable state. The main focus is on *what to solve* in contrast to an imperative style where the main focus is *how to solve*.

```javascript
// Imperative
const list = [1, 2, 3, 4, 5];
const item = list[list.length - 1];
```

```javascript
// Declarative
const list = [1, 2, 4, 5, 6, 7, 9, 10, 11, 12, 15, 16, 18, 20];
const item = getLastItem(list);
```

With this fundamental understanding, let’s try to understand few basic concepts in FP.

### First Class Function

![firstClass.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1624444960820/jiAJhNG2N.jpeg)
*Photo by <a href="https://unsplash.com/@fraumuksch?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Jennifer Latuperisa-Andresen</a> on <a href="https://unsplash.com/@fraumuksch?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>*
  

A programming language is said to have first-class functions when a function in that language is treated like any other variable.

A function can be —

* assigned to a variable
* passed as an argument
* returned by another function

```javascript
// Assign a function to a variable
const foo = function() {
   console.log("foobar");
}
foo(); // Invokve using variable


// Pass a function as an argument
function sayHello() {
   return "Hello, ";
}
function greeting(helloMessage, name) {
  console.log(helloMessage() + name);
}
greeting(sayHello, "JavaScript!"); // Pass `sayHello` as an argument to `greeting` function


// Return a function
function sayHello() {
   return function() {
      console.log("Hello!");
   }
}
```

### Higher-Order Function


![hof.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1624444944583/NarKow9t3.jpeg)
*Photo by <a href="https://unsplash.com/@hooverpaul55?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Paul Teysen</a> on <a href="https://unsplash.com/@hooverpaul55?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>*
  
A higher-order function is a function that takes a function as an argument or returns a function or both.

```javascript
// Takes function as an argument - map
const numbers = [1, 2, 3];
const doubledNumbers = numbers.map(number => number * 2); // [2, 4, 6]

// Returns a function
function sayHello() {
   return function() {
      console.log("Hello!");
   }
}
```

### Immutability


![immutabe.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1624444919115/94TGXhYTI.jpeg)
*Photo by [Jeremy Pair](https://unsplash.com/@pairphoto?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/@pairphoto?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)*

* Immutable means unchangeable.
* Once initialized one should never change it again.
* No changing of elements in an array and no changing of properties in an object.
* If required one should create a new copy of an array or an object with a changed value.

```javascript
let person = {
    name: 'James McGill',
    profession: 'Lawyer',
    description: `World's best lawyer`,
};

person.name = 'Saul Goodman';

person.description = `World's greatest lawyer`;

// Instead should be achieved as -

const person = {
    name: 'James McGill',
    profession: 'Lawyer',
    description: `World's best lawyer`,
};

const updatedPerson = {
    ...person,
    name: 'Saul Goodman',
};

const greatestLawyer = {
    ...updatedPerson,
    description: `World's greatest lawyer`,
};
```

```javascript
// Get all evens

const numbers = [2, 4, 5, 6, 8, 10];
numbers.splice(2, 1);
console.log(numbers); // [2, 4, 6, 8, 10]

// Instead should be achieved as -

const numbers = [2, 4, 5, 6, 8, 10];
const evens = [...numbers.slice(0, 2), ...numbers.slice(3)];
console.log(evens); // [2, 4, 6, 8, 10]
```

### Pure Function


![pure.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1624444896727/MhJ8nc3ew.jpeg)
*Photo by [manu schwendener](https://unsplash.com/@manuschwendener?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/@manuschwendener?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)*

A pure function is a function that given the same input will always return the same output and does not have any observable side effects.

Side effects may include —

* changing a file system
* inserting a record in a DB
* querying DOM
* mutations
> Note: Pure functions are cacheable (memoization), portable, testable (no mocks required) and [referentially transparent](https://en.wikipedia.org/wiki/Referential_transparency).

```javascript
// Impure
const list = [1,2,3,4,5];

list.splice(0,3); // [1,2,3]

list.splice(0,3); // [4,5]

list.splice(0,3); // []

// Pure
const list = [1,2,3,4,5];

list.slice(0,3); // [1,2,3]

list.slice(0,3); // [1,2,3]

list.slice(0,3); // [1,2,3]

```

```javascript
const list = [1,2,3,4,5];

// impure
const addToList = (list, number) => {
  list.push(number);
  return list;
}

// pure
const addToList = (list, number) => {
  const newList = [...list];
  newList.push(number);
  return newList;
}

```

### Currying


![currying.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1624444873624/GjFrXjITz.jpeg)
*Photo by [David Clode](https://unsplash.com/@davidclode?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/collections/3789327/underwater?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)*

Currying is where you can call a function with fewer arguments than it expects. It then returns a function that takes the remaining arguments.

```javascript
const discount = (price, percent) => (price * percent);
discount(100, 0.1); // 10

// Currying

const discount = percent => price => price * percent;

const flat10Percent = discount(0.1);
const flat50Percent = discount(0.5);
const newYearDiscount = discount(0.7);

flat10Percent(100); // 10
flat50Percent(100); // 50
newYearDiscount(100); // 70

```

```javascript
function sum(a, b) {
  return a + b;
}

// Actual    : sum(1, 2);
// Expected  : sum(1)(2);

function curry(f) {
  return function(a) {
    return function(b) {
      return f(a, b);
    };
  };
}
let curriedSum = curry(sum);
curriedSum(1)(2); // 3

```

### Composition


![composition.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1624444853964/-ijYAADmO.jpeg)
*Photo by [Fran Jacquier](https://unsplash.com/@fran_?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/@fran_?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)*

Composition simply is to compose 2 functions to create a new function.

```javascript
f(x) = x * 2
g(y) = y + 1

g.f = g(f(x))

composition:    g(f(x)), for x = 2; Result = 5


f: x -> y
g:      y -> z
h: x    ->   z
__________________________
For x = 2:

f: 2 -> 4
g:      4 -> 5
h: 2    ->   5 


const f = x => x * 2;
const g = y => y + 1;

const compose = (g, f) => x => g(f(x));
const h = compose(g, f);
h(2); // 5 
```

### Pointfree


![pointfree.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1624444821716/F0rIWwe-3.jpeg)
*Photo by [Philipp Berndt](https://unsplash.com/@philberndt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/@philberndt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)*

Pointfree simply means we don’t explicitly specify what data is being passed to a function.

```javascript
f(x) = g(x)

// Point free
f = g

-------------------------------------

const numbers = [1, 2, 3, 4, 5];
const doubles = numbers.map(x => x * 2);

// Making it pointfree

const double = x => x * 2
const numbers = [1, 2, 3, 4, 5]
const doubles = numbers.map(double); // Point free

```
> Note: Please go through the [Thinking in Ramda](https://randycoulman.com/blog/categories/thinking-in-ramda/) series to understand the benefits of pointfree style.

IMO, instead of waiting until all of FP's difficult principles are grasped, one could start small by breaking large functions into smaller reusable ones and then composing them together.

Hope the tiny gists help you understand the concepts.


### References

* [https://github.com/MostlyAdequate/mostly-adequate-guide](https://github.com/MostlyAdequate/mostly-adequate-guide)

* [https://medium.com/javascript-scene/composing-software-the-book-f31c77fc3ddc](https://medium.com/javascript-scene/composing-software-the-book-f31c77fc3ddc)

* [https://ramdajs.com/](https://ramdajs.com/)

* [https://randycoulman.com/blog/categories/thinking-in-ramda/](https://randycoulman.com/blog/categories/thinking-in-ramda/)

*Cover Photo by <a href="https://unsplash.com/@estherrj?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Esther Jiao</a> on <a href="https://unsplash.com/s/photos/blocks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>*