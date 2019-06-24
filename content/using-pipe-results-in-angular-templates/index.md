---
title: "Using Pipe Results in Angular Templates"
description: "Let’s say you need to build a filter pipe which is responsible for filtering lists in your application. We create a pipe that takes an array and search value, and filters the result based on the…"
date: "2017-12-11T16:28:50.462Z"
categories: 
  - JavaScript
  - Angular
  - Web Development

published: true
canonical_link: https://netbasal.com/using-pipe-results-in-angular-templates-430683fa2213
redirect_from:
  - /using-pipe-results-in-angular-templates-430683fa2213
---

![](./asset-1.jpeg)

Let’s say you need to build a filter pipe which is responsible for filtering lists in your application.

A simple example would be:

<Embed src="https://gist.github.com/NetanelBasal/43d689709939baab9702c3182da76c1c.js" aspectRatio={0.357} caption="" />

We create a pipe that takes an array and search value, and filters the result based on the search value with a simple `indexOf` check.

Let’s use our pipe.

<Embed src="https://gist.github.com/NetanelBasal/822da398605def66176d74a95e2d001d.js" aspectRatio={0.357} caption="" />

Great, everything works just fine. But now our product guy wants to show the user a label with the number of visible items.

I have already seen developers who solved the problem by doing the following:

<Embed src="https://gist.github.com/NetanelBasal/afde3b15721b4cb47fb0d8bbdb2c37fd.js" aspectRatio={0.357} caption="" />

But please, don’t, we are not DRY, and the pipe runs twice.

From Angular version 4, we have a solution for this situation, and I do not see any reason why your application needs to be in a lower version than that.

We can assign the pipe result to a local variable with the help of the `as` keyword.

<Embed src="https://gist.github.com/NetanelBasal/9cf9cde3fba784a70ac2d13b23cb3e67.js" aspectRatio={0.357} caption="" />

If you try to run the code above, you will encounter an error:

> Cannot read property ‘length’ of undefined

The reason is that the local variable is scoped to the hosting HTML element and its children.

The solution for this is to create a surrounding tag that uses `ngIf`, which exposes the local variable to the scoped template.

<Embed src="https://gist.github.com/NetanelBasal/e22fc00dd03c329aea1c77679f0f574a.js" aspectRatio={0.357} caption="" />

Now we are good.

<Embed src="https://stackblitz.com/edit/angular-wxff2b?embed=1" aspectRatio={undefined} caption="Live demo from StackBlitz. Try editing the code yourself." />

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Vue and JS!_
