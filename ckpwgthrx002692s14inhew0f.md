## CSS ‘position’ property demystified


Have you ever happened to use rather play with position property? If the answer is YES, you must have understood the use of the word ‘play’ in the previous sentence.

You must have seen element(s) dancing in the window or playing hide-n-seek, as you change the values of position property from *relative *to *absolute*,absolute to *fixed *and so on, in dev-tools.

In order to enjoy the dance of these elements or play hide-n-seek with them, you need to know some of the basics of dancing or the rules of hide-n-seek.

Let’s dive into the details.

Before we begin, an important question is **WHEN **to use position property? The answer lies in the name of the property itself **position**. Every element you specify has its own default (*static*) position in the document. If you happen to position the element in a different way from its default position you must use *position *property.

We have got an answer to **WHEN **to use position. Now the next question is,**WHERE** to position the element? By where I mean, 45px from *top *or 45px towards *left *or 45px from *bottom *or 45px towards *right*. By now it is clear that-
> Only *position *property is not sufficient to *position *an element. The *top*, *right*, *bottom *and *left *properties specify the *position *of *positioned *elements.

Confused…! If these properties specify the position of elements then what does **position **property specify?

Let’s answer it this way. If someone says, I have placed (positioned) some object at a distance of say 45 units. Obviously, your immediate question will be 45 units from what, where? Which means knowing only the distance doesn’t help you need to know the reference with which the distance is measured. *position* property tells you rather decides the reference.

*position *answers the questions **HOW **the element is positioned w.r.t. the reference and **WHAT** is the reference? Is it relatively positioned or absolutely positioned or is it fixed w.r.t. reference.

Permissible values for *position *property:

* static

* relative

* fixed

* absolute

Below fiddle will make things more clear as you read.


* **static**

It gives the element its default position. *top*, *right*, *bottom *and *left *have no effect if element is statically positioned.

* **relative**

If the element is relatively positioned then reference is its default (static) position. *top, right, bottom *and *left* can then be used to position it w.r.t. its *static *position.
> Relatively positioned elements are in the normal flow of the document and they **do **take up space while placing other elements.

This would be more clear once we discuss *absolute *value.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1623666003932/IDJyVG4yk.png)

* **fixed**

If the element is positioned as *fixed*, then the reference is view-port or rather window. *top, right, bottom *and *left* can then be used to position it. Once the element is fixed it will not leave its position even if you scroll.

* **absolute**

If the element is absolutely positioned then reference is its nearest positioned ancestor element.
> Absolutely positioned elements are taken out of the normal flow of the document and they **do not** take up space while placing other elements.

This means if in below example, one more element say #7 is added after #6, it would be placed exactly below #5 and to the left of #6 and not after #6 because #6 is absolutely positioned and so it doesn’t take up space. Check out this [demo](http://jsfiddle.net/niranjan_borawake/vtRx5/1/embedded/result,html,css). If you just position the #6 element as static, all of them would be placed one below the other. Check out this [demo](http://jsfiddle.net/niranjan_borawake/vtRx5/2/embedded/result,html,css).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1623666005918/G3nBYAXDL.png)

Now, are you ready to dance and play with the elements ? If YES, go [dance and play](http://jsfiddle.net/niranjan_borawake/vtRx5/embedded/result,html,css).