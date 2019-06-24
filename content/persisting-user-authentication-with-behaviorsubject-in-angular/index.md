---
title: "Persisting user authentication with BehaviorSubject in Angular"
description: "In this article, I will show you how to manage user authentication status with the help of the BehaviorSubject. The BehaviorSubject represents a value that changes over time, like the user…"
date: "2016-12-14T13:25:39.891Z"
categories: 
  - JavaScript
  - Angularjs
  - Angular2
  - Web Development
  - Programming

published: true
canonical_link: https://netbasal.com/angular-2-persist-your-login-status-with-behaviorsubject-45da9ec43243
redirect_from:
  - /angular-2-persist-your-login-status-with-behaviorsubject-45da9ec43243
---

![](./asset-1.jpeg)

In this article, I will show you how to manage user authentication status with the help of the BehaviorSubject.

#### Why BehaviorSubject?

The BehaviorSubject represents a value that changes over time, like the user authentication status for example. But the real power of the BehaviorSubject, in this case, is that every subscriber will always get the initial or the last value that the subject emits.

From the docs:

> Observers can subscribe to the subject to receive the last (or initial) value and all subsequent notifications.

Let’s start coding, and I hope you will understand better the concept and why we need the BehaviorSubject in this case.

First, let’s create the AuthService:

<Embed src="https://gist.github.com/NetanelBasal/44b541be9206466c4ca2dfc0f3783f3e.js" aspectRatio={0.357} caption="" />

We are creating new BehaviorSubject and setting the initial value to be the results from our _hasToken_ function.   
The _hasToken_ function is only checking if the user has a token in his local storage and returns the results as a boolean.

<Embed src="https://gist.github.com/NetanelBasal/c7f8444c72c9a0949b45968881b24475.js" aspectRatio={0.357} caption="" />

We are saving the token to local storage and sending the next status to our subscribers. ( In real life this will be after HTTP call )

<Embed src="https://gist.github.com/NetanelBasal/39c4bfaeb8767456e9c7f2415d840225.js" aspectRatio={0.357} caption="" />

We are removing the token from local storage and sending the next status to our subscribers.

<Embed src="https://gist.github.com/NetanelBasal/649909e45644aac27c0e69625a8d82bc.js" aspectRatio={0.357} caption="" />

A subject in Rx is both Observable and Observer. In this case, we only care about the Observable part, letting other parts of our app the ability to subscribe to our Observable.

We can return only the Observable part of our subject with the help of the _asObservable_ function.

Now we can use the AuthService, and because we are working with Observables, we can use the async pipe.

<Embed src="https://gist.github.com/NetanelBasal/595b4760778e71c53b8c9ed8a6ea1db2.js" aspectRatio={0.357} caption="" />

Now imagine you have a component that listens to the _isLoggedIn_ Observable after we already call the next method, with simple Observable or Subject the component will not get any data.

That’s why we need the BehaviorSubject because now it does not matter when you register the subscriber, he will get the last or initial value, and that’s what we want.

The complete AuthService:

<Embed src="https://gist.github.com/NetanelBasal/7b2594b9cbd20345f8056febe9909f55.js" aspectRatio={0.357} caption="" />

#### last note:

You should consider using the [share](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/share.md) operator in this case because we want to avoid from creating multiple subscriptions. (see @[Mark Pieszak](https://medium.com/@MarkPieszak?source=responses---------0-31--------) comment)

That’s all!

You can join to my Angular group on [Linkedin](https://www.linkedin.com/groups/8434339) to get the latest resources about Angular.

_☞_ **_Please tap or click “︎_**❤” _to help to promote this piece to others._
