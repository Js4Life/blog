---
title: "Angular Tests Made Easy With Spectator"
description: "Writing tests for our code is part of our daily routine. When working on large applications with many components, it can take time and effort. Although Angular provides us with great tools to deal…"
date: "2017-09-13T14:42:34.340Z"
categories: 
  - JavaScript
  - Angular
  - Angular2
  - Testing
  - Web Development

published: true
canonical_link: https://netbasal.com/angular-tests-made-easy-with-ngx-easy-test-7b8b75d8a47d
redirect_from:
  - /angular-tests-made-easy-with-ngx-easy-test-7b8b75d8a47d
---

![](./asset-1.jpeg)

Writing tests for our code is part of our daily routine. When working on large applications with many components, it can take time and effort.

Although Angular provides us with great tools to deal with these things, it still requires quite a lot of boilerplate work.

For this reason, I decided to create a library that will make it easier for us to write tests by cutting the boilerplate and add custom Jasmine matchers.

> Spectator helps you get rid of all the boilerplate grunt work, leaving you with readable, sleek and streamlined unit tests.

Let’s look at some examples.

The first step is to install the [library](https://github.com/NetanelBasal/spectator) through npm or yarn.

```
npm install @netbasal/spectator --save-dev
```

#### Building The Zippy Component

Let’s create a simple zippy component for testing.

<Embed src="https://gist.github.com/NetanelBasal/c83ff2683df400949ae0cd8b95fcf1bb.js" aspectRatio={0.357} caption="" />

#### Testing The Zippy Component

To test it we will use the `createHostComponentFactory()` method.

<Embed src="https://gist.github.com/NetanelBasal/e7b5522da2086fd3c7333645ba0813fe.js" aspectRatio={0.357} caption="" />

As you can see, this way removes most of the boilerplate and gives you a clear focus on the important things, and it’s also much more readable.

Now, let’s see **another** way to write tests without a host element.

#### Building The Button Component

Let’s create a simple button component for testing.

<Embed src="https://gist.github.com/NetanelBasal/3d5f5b3cd663f220d7d43dcfe905831d.js" aspectRatio={0.357} caption="" />

#### Testing The Button Component

To test it we will use the `createTestComponentFactory()` method.

<Embed src="https://gist.github.com/NetanelBasal/0c666a2ff8a20788c80e401f0efd4d91.js" aspectRatio={0.357} caption="" />

This is just a taste of the library’s capabilities. You can find the full documentation [here](https://netbasal.gitbooks.io/spectator/content/). If you have requests, comments or you want to contribute, I would love to hear.

More useful libraries from the creator: [ngx-auto-unsubscribe](https://github.com/NetanelBasal/ngx-auto-unsubscribe), [ngrx-generator](https://github.com/NetanelBasal/ngrx-generator), [ngx-take-until-destroy](https://github.com/NetanelBasal/ngx-take-until-destroy), [helpful-decorators](https://github.com/NetanelBasal/helpful-decorators).

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Vue and JS!_
