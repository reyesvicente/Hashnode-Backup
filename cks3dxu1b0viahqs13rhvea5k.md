---
title: "How does this keyword in JavaScript work?"
datePublished: Mon Apr 15 2019 16:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cks3dxu1b0viahqs13rhvea5k
slug: how-does-this-keyword-in-javascript-work
canonical: https://highcenburg.tech.blog/2019/04/16/how-does-this-keyword-in-javascript-work/
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682771571974/0297f688-63af-4dd2-ae99-148a94097c79.jpeg
tags: javascript

---

The JavaScript this keyword refers to the object it belongs to. It has different values depending on where it is used. In a method, this refers to the **owner object**. Alone, **this** refers to the **global object**. In a function, this refers to the global object. In a function, in strict mode, **this** is **undefined**. In an event, **this** refers to the **element** that received the event. Methods like **call()**, and **apply()** can refer to **this** to any object.

```python
let dog = {
  name: "Spot",
  numLegs: 4,
  sayLegs: function() {return "This dog has " + this.numLegs + " legs.";}
};

console.log(dog.sayLegs());
// Outputs "This dog has 4 legs."
```

Reference: https://learn.freecodecamp.org/javascript-algorithms-and-data-structures/object-oriented-programming/make-code-more-reusable-with-the-this-keyword https://www.w3schools.com/js/js\_this.asp