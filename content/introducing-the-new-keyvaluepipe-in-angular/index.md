---
title: "Introducing the New KeyValuePipe in Angular"
description: "As you probably know, the ngFor directive doesn’t support iterations over objects or Maps. But there are many times when we need this kind of functionality. The KeyValuePipe was created to address…"
date: "2018-07-26T07:29:02.269Z"
categories: 
  - Angular2
  - Web Development
  - Angularjs
  - JavaScript

published: true
canonical_link: https://netbasal.com/introducing-the-new-keyvaluepipe-in-angular-435ac2205c72
redirect_from:
  - /introducing-the-new-keyvaluepipe-in-angular-435ac2205c72
---

![](./asset-1.jpeg)

Angular version **6.1** has been [released](https://blog.angular.io/angular-v6-1-now-available-typescript-2-9-scroll-positioning-and-more-9f1c03007bb6) and it’s shipped with a new useful pipe — `KeyValuePipe` .

As you probably know, the `ngFor` directive doesn’t support iterations over objects or Maps. But there are many times when we need this kind of functionality.

The `KeyValuePipe` was created to address this issue. It transforms an Object or a Map into an array of key-value pairs so we can use `ngFor` on it.

Let’s see how we can use it:

<Embed src="https://gist.github.com/NetanelBasal/5eb64567beb384218efba6ba22c65a19.js" aspectRatio={0.357} caption="" />

The result of the pipe’s conversion looks like this:

<Embed src="https://gist.github.com/NetanelBasal/c2267539bb92f41446f2cff82537454c.js" aspectRatio={0.357} caption="" />

Let’s see how we can use it with `Map`.

<Embed src="https://gist.github.com/NetanelBasal/26440c9807d781b7bdf1a269c9bb6974.js" aspectRatio={0.357} caption="" />

By default, the output array will be ordered by keys (Unicode point value). Here are examples of arrays that were created based on the default alphanumerical order:

<Embed src="https://gist.github.com/NetanelBasal/714cc35bce9225175bb2f3a6112bd371.js" aspectRatio={0.357} caption="alpha" />

<Embed src="https://gist.github.com/NetanelBasal/19c0cbc0b35588578ab444ebe0a8fe67.js" aspectRatio={0.357} caption="numerical" />

But you can always override that by passing a `compareFn` as the second input, if you need a custom ordering.

<Embed src="https://gist.github.com/NetanelBasal/eaafe551a0230da3e0ba0bf1ccef1079.js" aspectRatio={0.357} caption="" />

<Embed src="https://stackblitz.com/edit/keyvaluepipe?embed=1" aspectRatio={undefined} caption="" />

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Akita and JS!_

### **Things to not miss**:

[**🚀 Introducing Akita: A New State Management Pattern for Angular Applications**  
_Every developer knows state management is difficult. Continuously keeping track of what has been updated, why, and…_netbasal.com](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8 "https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8")[](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8)

[**NetanelBasal/spectator**  
_spectator - 👻 Angular Tests Made Easy 🤓_github.com](https://github.com/NetanelBasal/spectator "https://github.com/NetanelBasal/spectator")[](https://github.com/NetanelBasal/spectator)
