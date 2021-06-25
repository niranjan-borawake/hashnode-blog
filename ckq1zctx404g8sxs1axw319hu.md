## When not to use Array.prototype.map()

It's nearly impossible to find a JavaScript developer who hasn't used `[].map()`. But I have unfortunately come across many, during interviews and PR reviews,  with different levels of experience, misusing it.

Let's get a better understanding of it so we don't misuse it.

- What is `[].map()`?
- Why to use `[].map()`?
- When (not) / How to use `[].map()`?

### What is `[].map()`? 

Simply put, it maps a given thing in one form to another. A photocopying machine copying a colour image into the black and white image is the best analogy.

![arrayMap.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1623938921480/ejntyjuhh.png)

Points to consider -

1. The original photo remains intact.
2. A new photo is received.
3. The image on the original and the new copy is same (except for colours).

Similarly, when using `map` - 

1. The original array remains intact.
2. A new array is returned.
3. The original and returned array do have things in common or at least they are bound by some mapping logic.

### Why to use `[].map()`?


- It's intuitive.
- It's declarative.
- It allows chaining.
- It's always good to use the right tools for the right job in the right way.

### When (not) / How to use `[].map()`?

#### Use `map` when - 

- mapping an array into a new array, with some specific mapping logic.
- expecting a new array to be returned (and actually use it in next steps).

``` javascript
const actors = [{
  name: 'Bryan Cranston',
  aka: 'Walter White',
  birthYear: 1956,
}, {
  name: 'Aaron Paul',
  aka: 'Jesse Pinkman',
  birthYear: 1979,
}, {
  name: 'Bob Odenkirk',
  aka: 'Saul Goodman',
  birthYear: 1962,
}];

// Get alternative name and age for all actors

const akaAndAgeForActors = actors.map(({
  aka,
  birthYear
}) => ({
  aka,
  age: new Date().getFullYear() - birthYear,
}));
```
#### Do not use `map` when - 
- the returned array is not used in next steps.
- not returning anything from the `callbackFunction` or `mapperFunction` passed to `map`.
> Do NOT use `map` to iterate over an array.

Have seen this pattern multiple times and should be avoided.

``` javascript 
const series = [{
  name: 'Breaking Bad',
  isWatched: true
}, {
  name: 'Better Call Saul',
  isWatched: true,
}, {
  name: 'Ozark',
  isWatched: true,
}, {
  name: 'Peaky Blinders',
  isWatched: false,
}];

// Get names of all watched series

const watchedSeries = [];

series.map(({
  name,
  isWatched
}) => {
  if (isWatched) {
    watchedSeries.push({
      name,
    })
  }
});
```

Instead use  [forEach](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)  or  [for...of](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of). 

For more information have a look at MDN Docs [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map).



Next time using `[].map`, remember - 
> It's pointless to use a photocopy machine if the returned duplicate will be discarded.


*Cover Photo by <a href="https://unsplash.com/@travelergeek?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">T R A V E L E R G E E K</a> on <a href="https://unsplash.com/s/photos/hammer-and-nail?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>*
  


 



