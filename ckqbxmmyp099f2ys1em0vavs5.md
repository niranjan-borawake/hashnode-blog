## Do You Really Know JavaScript’s ‘this’ Keyword?

How many times in your professional career have you felt that you knew something, but when you needed to explain it to someone or came across an interesting blog post about it, you calmly reminded yourself, "No, I didn't know it."

If you're a JavaScript developer, there's a good chance this has happened to you with the `this` keyword.

Because as they say —
> There are only 2 things that are hard in JavaScript — `this` and everything else.

If you are new to JavaScript and landed here to understand how the `this` keyword works, then this is not the right place. Please head to the MDN docs [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this).

Okay. You're still reading this. So you believe you are an experienced JavaScript developer who understands `this`.

Would you mind going through a very quick exercise to prove it?

![Prove you understand this](https://cdn.hashnode.com/res/hashnode/image/upload/v1624600509531/qr2QkGWJY.png)

Here’s the [fiddle](https://jsfiddle.net/niranjan_borawake/8wyj1506/), go and prove it.

Don’t be in a hurry.


![Some things take time](https://cdn.hashnode.com/res/hashnode/image/upload/v1624600585562/TqBwFwRqF.jpeg)


*Photo by <a href="https://unsplash.com/@duanemendes?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Duane Mendes</a> on <a href="https://unsplash.com/@duanemendes?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>*

Did you really try the exercise? If not, please do so.

How was the exercise?



1. 
Easy, I solved it in a minute. I was right, I did know `this`. *Great!*
1. 
Difficult, I took some time but was able to solve it. *Interesting*.
1. 
Could not solve it. Sorry, I still don’t know how `this` works. *So, you were wrong.*

Please note that we kept the exercise simple to focus only on `this` and did not consider arguments passed in the `printMessage` function.

If you fall under the #1 category, then you might want to try passing a few values to `printMessage` , use them, and then try to make it work again.

Albeit, here is a very simple [solution](https://jsfiddle.net/niranjan_borawake/c40238fh/) to the exercise.

I hope it was worth the exercise. For a full-proof solution, have a look at `bind` polyfill [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_objects/Function/bind#polyfill).

As per [Kent C. Dodds](https://twitter.com/kentcdodds)’s recent tweet —

%[https://twitter.com/kentcdodds/status/1388561641382420480?s=20]


If `this` is a problem for you, could it be possible to eliminate the complexity of `this` ?

In my opinion, yes. Switch to functional programming. Here’s [my take](https://blog.niranjanborawake.in/functional-programming-using-javascript) on the very basic concepts of functional programming.


*Photo by <a href="https://unsplash.com/@srz?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">sydney Rae</a> on <a href="https://unsplash.com/@srz?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>*