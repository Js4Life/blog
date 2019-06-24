---
title: "When to Unsubscribe in Angular"
description: "As you probably know when you subscribe to an observable or event in JavaScript, you usually need to unsubscribe at a certain point to release memory in the system. Otherwise, you will have a memory…"
date: "2017-04-26T18:42:12.873Z"
categories: 
  - JavaScript
  - Rxjs
  - Angularjs
  - Angular
  - Web Development

published: true
canonical_link: https://netbasal.com/when-to-unsubscribe-in-angular-d61c6b21bad3
redirect_from:
  - /when-to-unsubscribe-in-angular-d61c6b21bad3
---

![](./asset-1.jpeg)

As you probably know when you subscribe to an observable or event in JavaScript, you _usually_ need to unsubscribe at a certain point to release memory in the system. Otherwise, you will have a memory [leak](https://appendto.com/2016/11/avoiding-memory-leaks-in-javascript/).

Let’s see the most common cases that you will need to unsubscribe inside the `ngOnDestroy` lifecycle hook.

#### Forms —

<Embed src="https://gist.github.com/NetanelBasal/183d7391cbf7aac37de1cd37ff12e85f.js" aspectRatio={0.357} caption="" />

This also applies to any form control.

#### The Router —

<Embed src="https://gist.github.com/NetanelBasal/42583587f290f9ed4d72587791bcdacf.js" aspectRatio={0.357} caption="" />

According to the official documentation, Angular should unsubscribe for you, but apparently, there is a [bug](https://github.com/angular/angular/issues/16261).

#### Renderer Service —

<Embed src="https://gist.github.com/NetanelBasal/a2ec868617f38230750716716f90f6e1.js" aspectRatio={0.357} caption="" />

#### Infinite Observables —

When you have an **infinite** sequence, you should unsubscribe (unless you have a special case), for example when using the `interval()` or the `fromEvent()` observables.

<Embed src="https://gist.github.com/NetanelBasal/f2de112be1f1e758b62705a5e654a15b.js" aspectRatio={0.357} caption="" />

#### Redux Store —

<Embed src="https://gist.github.com/NetanelBasal/3303b51745c982601f883849a623e232.js" aspectRatio={0.357} caption="" />

[ngrx/store](https://github.com/ngrx/store) and [redux-angular](https://github.com/angular-redux/store) `select` method returns an observable. Therefore they have to be cleaned.

### Don’t Unsubscribe

#### [Async](https://angular.io/docs/ts/latest/api/common/index/AsyncPipe-pipe.html) pipe —

<Embed src="https://gist.github.com/NetanelBasal/94108b87f415e507082dd354fd5db093.js" aspectRatio={0.357} caption="" />

When the component gets destroyed, the `async` pipe unsubscribes automatically to avoid potential memory leaks.

#### @HostListener —

<Embed src="https://gist.github.com/NetanelBasal/8b6b6b908875b20bee6c659da68b8e76.js" aspectRatio={0.357} caption="" />

#### Finite Observable —

When you have a **finite** sequence, **usually** you don’t need to unsubscribe, for example when using the `HTTP` service or the `timer` observable.

<Embed src="https://gist.github.com/NetanelBasal/55276f8948a1724075ee9109fc1dfb1a.js" aspectRatio={0.357} caption="" />

### Final tip —

You should be more declarative and try as little as possible to call the `unsubscribe` method. You can read more about the subject in this article — [RxJS: Don’t Unsubscribe](https://medium.com/@benlesh/rxjs-dont-unsubscribe-6753ed4fda87).

For example:

<Embed src="https://gist.github.com/NetanelBasal/96665f08a18302d3a8939ad345a1f16c.js" aspectRatio={0.357} caption="" />

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Vue and JS!_
