---
title: "Use RxJS to Modify App Behavior Based on Page Visibility"
description: "As developers, there are times when we want to provide real-time updates for specific features in our application. A typical example might be user notifications, alerts, or to verify that the current…"
date: "2019-04-30T08:01:43.159Z"
categories: 
  - JavaScript
  - Angular
  - Rxjs
  - Angular2
  - Web Development

published: true
canonical_link: https://netbasal.com/use-rxjs-to-modify-app-behavior-based-on-page-visibility-ce499c522be4
redirect_from:
  - /use-rxjs-to-modify-app-behavior-based-on-page-visibility-ce499c522be4
---

![](./asset-1.png)

### 🤓 A Brief Overview

As developers, there are times when we want to provide real-time updates for specific features in our application. A typical example might be user notifications, alerts, or to verify that the current user session hasn’t expired.

Unfortunately, not all servers provide push-based notifications such as web-sockets, so we’ll most likely create a polling system that goes and fetches the result from a REST endpoint once in a given cycle.

With tabbed browsing, there is a reasonable chance that any given webpage is in the background and thus not visible to the user. In such a case, when the user isn’t really engaging with the application, there’s no reason to keep polling and cause the server to perform redundant operations.

**We should always strive to interrupt the server as little as possible so that it is able serve other users more efficiently.** Let’s see how we can do this.

### 👁 Page Visibility API

The [Page Visibility API](https://developer.mozilla.org/en-US/docs/Web/API/Page_Visibility_API) lets your application know when a page is visible to the user. This basic information enables the creation of web pages that behave differently when they’re not being viewed.

Whenever the user minimizes the window or switches to another tab, the API emits a `visibilitychange` event to let listeners know the state of the page has changed. It does so also when the page becomes visible again. We can detect the event, and perform actions or alter the behavior of existing ones accordingly.

### 🦊 Use Case Example (Polling)

For our example, we’ll create a notification service that polls notifications from the server. (We’ll use Angular, but it will work with any other implementation)

<Embed src="https://gist.github.com/NetanelBasal/407cf26d57a9d6d2f869179b19c57ce7.js" aspectRatio={0.357} caption="" />

Let’s create the infinite poll operator with RxJS, combining the `timer` and `concatMap` operators. This is an elegant way to poll from the server, you can read more about why [here](https://medium.com/@inbalsinai/timers-for-asynchronous-polling-operations-the-good-the-bad-and-the-ugly-b446a018725a).

<Embed src="https://gist.github.com/NetanelBasal/56c940308ee738a360b7a3cc1112bca3.js" aspectRatio={0.357} caption="" />

Building your own operator is as simple as writing a function which takes a source observable as an input and returns an output stream.

In our case, we use the `timer` observable that takes a time period and an initial delay (which defaults to zero as we usually want to start the polling immediately) and concat the source observable. This will give us infinite polling functionality.

Next, we’ll create one last custom RxJS operator that leverages the Page Visibility API together with two built-in operators, in order to achieve the desired functionality:

<Embed src="https://gist.github.com/NetanelBasal/e293a2aec173dc9672082c30fff0e77c.js" aspectRatio={0.357} caption="" />

The `visibilitychange$` observable uses the `[fromEvent](https://rxjs-dev.firebaseapp.com/api/index/function/fromEvent)` observable, which adds a `visibilitychange` event listener to the document object. Since we want to create an event listener only once, we employ the `[shareReplay](https://rxjs-dev.firebaseapp.com/api/operators/shareReplay)` operator.

Then, we create two observables variations based on the `visibilitychange$` observable, one for when the page is visible and the another for when it’s hidden.

The final step is to chain the original source with two built-in operators that do all the work for us:

The `[takeUntil](https://rxjs-dev.firebaseapp.com/api/operators/takeUntil)` operator takes an observable. When it emits, it will automatically unsubscribe from the source observable. In our case, when the user switches tab `pageHidden$` will emit, and cause a termination of the notifications polling.

The `[repeatWhen](https://rxjs-dev.firebaseapp.com/api/operators/repeatWhen)` operator does the opposite; It takes a function which returns an observable. When this observable emits it resubscribes to the source observable. In our case, when the user returns to our application , it will re-subscribe to the notifications polling. It’s as simple as that 😀

<Embed src="https://gist.github.com/NetanelBasal/0d4fe5db08f3e0c6bf319cde089a0df2.js" aspectRatio={0.357} caption="" />

**A bit of advice:** consider adding a `[retry](https://rxjs-dev.firebaseapp.com/api/operators/retry)` functionality in case of error.

### 🚒 Hot Update

Starting from [RxJS v6.5](https://netbasal.com/whats-new-in-rxjs-v6-5-d0d74a6752ac), we can make this example even shorter and use the `partition` observable creation:

<Embed src="https://gist.github.com/NetanelBasal/03399bac08a089a4fb8b40f041642a01.js" aspectRatio={0.357} caption="" />

### **😍** 🚀 **Have You Tried Akita Yet?**

One of the leading state management libraries, Akita has been used in countless production environments. It’s constantly developing and improving.

Whether it’s entities arriving from the server or UI state data, Akita has custom-built stores, powerful tools, and tailor-made plugins, which help you manage the data and negate the need for massive amounts of boilerplate code. We/I highly recommend you try it out.

[**🚀 Introducing Akita: A New State Management Pattern for Angular Applications**  
_Every developer knows state management is difficult. Continuously keeping track of what has been updated, why, and…_netbasal.com](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8 "https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8")[](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8)

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Akita and JS!_
