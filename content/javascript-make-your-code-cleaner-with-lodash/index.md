---
title: "JavaScript — Make Your Code Cleaner With Lodash"
description: "I want to share with you a tip for cleaner code when you need to validate your code with a bunch of if statements. How many times did you get an annoying exception because you forgot to check one…"
date: "2017-03-27T04:32:40.407Z"
categories: 
  - JavaScript
  - Web Development
  - Programming

published: true
canonical_link: https://netbasal.com/javascript-make-your-code-cleaner-with-lodash-e1292663d2de
redirect_from:
  - /javascript-make-your-code-cleaner-with-lodash-e1292663d2de
---

![](./asset-1.jpeg)

I want to share with you a tip for cleaner code when you need to validate your code with a bunch of `if` statements.

How many times you wrote something like this:

```
let person = {};
let street = “Default message when street is undefined”;

if( person && person.address && person.address.street ) {
 street = person.address.street;
}
```

How many times did you get an annoying exception because you forgot to check one step in your validation process?

And this is only a simple object I’m sure you wrote a lot more than that.

Lodash has a [util](https://lodash.com/docs/4.17.4#get) function that helps with this chaos.

#### `[get](https://lodash.com/docs/4.17.4#get)( object, path, [defaultValue] )`

> Gets the value at path of object. If the resolved value is undefined, the defaultValue is returned in its place

So now we can change our code to this:

```
import get from ‘lodash/get’;

let street = get(person, “address.street”, “Default message when street is undefined”);
```

We cleaner!

This is also works with arrays, for example:

```
let street = get(person, “addresses[0].street”, “Default message when street is undefined”);
```

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Vue and JS!_

![](./asset-2.png)
