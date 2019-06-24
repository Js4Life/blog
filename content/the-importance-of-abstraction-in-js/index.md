---
title: "The Importance Of Abstraction in JS"
description: "JavaScript is no longer what it used to be. The Web has evolved in an extraordinary way. Yes, you know what I’m talking about. You sense it too. The bigger your application, the more difficult it is…"
date: "2017-12-19T04:23:51.325Z"
categories: 
  - JavaScript
  - Web Development
  - Design Process
  - Design Patterns
  - Nodejs

published: true
canonical_link: https://netbasal.com/the-importance-of-abstraction-in-js-ea27e07e996
redirect_from:
  - /the-importance-of-abstraction-in-js-ea27e07e996
---

![](./asset-1.jpeg)

JavaScript is no longer what it used to be. The Web has evolved in an extraordinary way. Yes, you know what I’m talking about. You sense it too.

Every day new libraries are published, new ideas are introduced, and new API’s come into the world.

If you’re like me, and I believe you are, you always want your application to be ahead of its time.

The bigger your application, the more difficult it is to migrate to newer stuff. How can we always stay one step ahead? How can we minimize the difficulty of this transition?

Let me equip you with one important tip — abstraction.

> Abstraction is a process whereby you hide implementation details from the developer and instead only provide the functionality

Put simply, the developer will have the information on what the object does, not how it does it.

Let’s examine two examples from our world.

### #Example One — Lodash

For this example, we will take the library that most of us use — [Lodash](https://lodash.com/).

If you are using Lodash, I assume all your code is smeared with imports like the following:

<Embed src="https://gist.github.com/NetanelBasal/3d39fead79a488d769cfc87fb29ee21f.js" aspectRatio={0.357} caption="Lodash usage" />

The problem with the above code is that when we use it, **we tie our application to Lodash**. But what if, at some point, we want to change Lodash with something like [Ramda](http://ramdajs.com/), or even better, with the native JS API’s? This won’t be an easy task.

To overcome the problem, we can create an abstraction.

<Embed src="https://gist.github.com/NetanelBasal/2f6c6224c8dfc9acc4ac905b1e9da511.js" aspectRatio={0.357} caption="Array utils" />

With this abstraction, we gain three important things:

1.  We have one place with our single source of truth.
2.  We can check for the support of native API’s and use them.
3.  We can swap the implementation without breaking anything.

<Embed src="https://gist.github.com/NetanelBasal/f561ea80d9e5489460c6f8f79da3fdd7.js" aspectRatio={0.357} caption="Array utils with Ramda" />

### #Example Two— HTTP

Every application leverages a third-party HTTP library to deal with XHR requests. Let’s say you’re working with the [axios](https://github.com/axios/axios) library and are using it like this:

<Embed src="https://gist.github.com/NetanelBasal/3d886e66eafdd29a774ccdbc13566483.js" aspectRatio={0.357} caption="Todos service — axios" />

Again, **our application is tied to a specific implementation**. What if we want to use a different library or swap and use the native fetch API? What if, even worse, a new version of axios is released with breaking changes in the API? we won’t be covered.

To overcome this problem, we can create an abstraction that will encapsulate the real implementation.

<Embed src="https://gist.github.com/NetanelBasal/6b494894600c4b4e4c4e3d386cf1f9aa.js" aspectRatio={0.357} caption="HTTP service" />

With this abstraction, we gain three important things:

1.  The implementation is in one place, allowing us to easily address breaking changes.
2.  We can swap a library without breaking anything.
3.  We can add things like logging, or global HTTP headers in one place. (if the library doesn’t support these)

<Embed src="https://gist.github.com/NetanelBasal/76c568bd2f3f5110cbd27ca8237facb4.js" aspectRatio={0.357} caption="Add logging" />

### Summary

In this article we saw the problems we face when we tie our application to a specific implementation, and we learned how to solve these issues with abstractions. You do not have create abstractions for everything. Use your judgment to decide it’s necessary in your particular case.

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Vue and JS!_
