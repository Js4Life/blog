---
title: "Using 3rd Party Library Inside Angular2"
description: "When you’re working on your project, very often you will find yourself in need to use a non-angular library, such as a tools library or a plugin. In this article, I will show you how to integrate…"
date: "2016-11-14T06:22:20.950Z"
categories: 
  - JavaScript
  - Typescript
  - Angularjs
  - Angular2
  - Programming

published: true
canonical_link: https://netbasal.com/using-3rd-party-library-inside-angular2-c06b7675d3a
redirect_from:
  - /using-3rd-party-library-inside-angular2-c06b7675d3a
---

![](./asset-1.jpeg)

_By: Netanel Basal (Angular Expert) and Yaron Biton, misterBIT.co.il CTO_

When you’re working on your project, very often you will find yourself in need to use a non-angular library, such as a tools library or a plugin.

In this article, I will show you how to integrate third-party library named _shave_ with Angular 2 by creating custom directive.

Shave is a zero dependency javascript plugin that truncates multi-line text to fit within an HTML element based on a set max-height. You can take a look at it [here](https://github.com/dollarshaveclub/shave).

#### Step one — Install the library:

```
npm install shave --save
```

#### Step two— Declare the directive:

<Embed src="https://gist.github.com/NetanelBasal/4322bab4fa099d43c55392a304445db9.js" aspectRatio={0.357} caption="" />

Assuming you work with Typescript you will get this error:

```
cannot find module shave
```

#### Why do I get this error?

You are getting this error because the _shave_ library doesn’t have declaration file and Typescript doesn’t know what _shave_ is.

We need to “teach” typescript what _shave_ is. I’m working with angular-cli, so all the customs declarations will be in a file named **typings.d.ts**.

After looking at _shave_ documentation I came up with this:

<Embed src="https://gist.github.com/NetanelBasal/f9caa2b7e9d731a0956a1cf9973e0f3a.js" aspectRatio={0.357} caption="" />

We are declaring the shave module and exporting the shave function as the default. This will resolve the Typescript error, great!

You can learn more about typescript declarations [here](https://www.typescriptlang.org/docs/handbook/declaration-files/deep-dive.html). I’m encouraged you to submit your typings to the DefinielyTyped repository so other developers can benefit from your work.

#### Step three — Implement the directive:

<Embed src="https://gist.github.com/NetanelBasal/c1835bb31ebe5b2f00dbad50c28197ed.js" aspectRatio={0.357} caption="" />

The first required param in the shave function is the DOM element. We can get a reference to the host element by injecting the ElementRef.

#### Our Inputs:

1.  shaveMaxHeight — (default: 100px) allows the user to set the max height.
2.  shave — a way to set the extra options.

#### Why we need the setters?

We define the setters because we need a way to re-run our code every time the directive Inputs changes ( like a $watch in Angular 1 ).  
In our case every time the shave options or the max height Inputs will change, we need to re-run the runShave function to update the DOM.

Another option here would be to use the **ngOnChanges** life cycle hook. If you have multiple Inputs to track, this method may be preferred. (In our case I am showing you also the setters approach )

#### Important Note:

You can implement **ngOnChanges** to be notified of changes if:

1.  Your Input is JavaScript primitive type.
2.  Your Input reference changes (using some kind of immutability strategy).

If the model reference doesn’t change, but some property of the Input model changes, you may implement the **ngDoCheck** life cycle hook to manually construct your change detection logic.

That’s all.

_☞_ **_Please tap or click “︎_**❤” _to help to promote this piece to others._
