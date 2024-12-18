---
title: "The difference of Arrays vs Objects in Javascript"
datePublished: Sun Apr 14 2019 16:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cks3duiu40vhshqs11zqc5cik
slug: the-difference-of-arrays-vs-objects-in-javascript
canonical: https://highcenburg.tech.blog/2019/04/15/the-difference-of-arrays-vs-objects-in-javascript/
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682771623658/746b431a-4c5c-482d-93da-73b168c16256.jpeg
tags: javascript

---

Objects are, according to [Merriam-Webster](https://www.merriam-webster.com/dictionary/object) is something material that may be perceived by the senses. While Arrays, are, according to [Merriam-Webster](https://www.merriam-webster.com/dictionary/array) is a number of mathematical elements arranged in rows and columns.

In JavaScript, Objects are used to model real-world objects, giving them properties and behavior just like their real-world counterparts while Arrays, according to [w3schools.com](https://www.w3schools.com/js/js_arrays.asp) are used to store multiple values in a single variable.

This is an object :

```python
let dog {
    name: "LeBrown",
    numLegs: 4
};
```

While this is an array:

`var cars = ["Volkswagen", "Volvo", "Merc"];`

The difference of both are:

1.) Arrays are just objects but with some extra methods.

2.) There is nothing an object can do that an array can’t.

As a general rule of thumb, if you need to store a collection of properties with varying types, use an object. Otherwise, use an array.

Reference: https://frontendmayhem.com/javascript-arrays-objects/ https://www.w3schools.com/js/js\_arrays.asp https://learn.freecodecamp.org/javascript-algorithms-and-data-structures/object-oriented-programming/create-a-basic-javascript-object