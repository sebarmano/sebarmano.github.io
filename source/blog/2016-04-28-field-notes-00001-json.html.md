---
title: Field Notes 1 - Javascript Objects vs JSON
date: 2016-04-28 3:24 UTC
tags: notes, json, javascript, objects
---

Today I've learned something about **Javascript Objects** and **JSON**. JSON
stands for _Javascript Object Notation_. It is widely used in APIs because as
its name mentions, it is compatible with Javascript, which is by far the most
used web client side language.

JSON looks a lot like a Javascript object.

However, despite being compatible, JSON has a minor (but important) difference
with a regular Object. Here are two examples, the first is a plain Javascript
object, the second a JSON one. Can you spot the difference?

```Javascript
// Javascript Object

var person = {
  name: "Sebastian Armano",
  twitter : "@sebarmano",
  city: "Jersey City, NJ"
};
```
```Javascript
// JSON

var person = {
  "name": "Sebastian Armano",
  "twitter": "@sebarmano",
  "city": "Jersey City, NJ"
};
```

Even though both look a lot alike, JSON keys _are strings_, while Javascript
ones are not. This is important for example to be able to understand if a
variable would be JSON compatible and could be read by any API consumer.

JSON is always compatible with Javascript, but that's not true the other way
around.
