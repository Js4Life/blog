---
title: "Testing Observables in Angular"
description: "In this article, I’d like to talk about a misconception I’ve read in other articles about writing tests for observables in Angular. Other articles around the web suggest that, in order to test the…"
date: "2018-04-04T07:23:01.670Z"
categories: 
  - JavaScript
  - Angular
  - Testing
  - Web Development

published: true
canonical_link: https://netbasal.com/testing-observables-in-angular-a2dbbfaf5329
redirect_from:
  - /testing-observables-in-angular-a2dbbfaf5329
---

![](./asset-1.jpeg)

In this article, I’d like to talk about a misconception I’ve read in other articles about writing tests for observables in Angular.

Let’s examine this basic example we’re all familiar with.

<Embed src="https://gist.github.com/NetanelBasal/01b75fad9e4e4adaa5d5aa229dfbfb91.js" aspectRatio={0.357} caption="todos component spec" />

We have data service that uses the Angular HTTP library to return cold observable.

Other articles around the web suggest that, in order to test the above component, we can create a stub service that returns an `of()` observable.

<Embed src="https://gist.github.com/NetanelBasal/8fb1b1006b452fb2e13f8384bc532ca3.js" aspectRatio={0.357} caption="todos component spec" />

You run the code above. The test passes, and you are 👯 👯 👯 👯 👯

> That’s what I call cheating 🙈

By default, the `of()` observable is synchronous, so you’re basically making asynchronous code synchronous.

Let’s demonstrate this with a small add-on to our code.

<Embed src="https://gist.github.com/NetanelBasal/c8f80c76caa22514d9eeb0a730fa8d8d.js" aspectRatio={0.357} caption="todos component" />

We added a loading element that should be visible when the request begins but hidden when the subscription function is called. (i.e., the request finished successfully)

Let’s test it.

<Embed src="https://gist.github.com/NetanelBasal/01276f66cb37e33c4218cd99e39e8a68.js" aspectRatio={0.357} caption="todos component spec" />

As you likely imagined, the above test will never pass. You will get the error:

> Expected null not to be null.

When we run `detectChanges()`, the `ngOnInit()` hook will run and execute the subscription function synchronously, causing the `isLoading` property to always be false. As a result, the test always fails.

You probably remember the old days, when we wrote tests in AngularJS and stub promises like this:

```
spyOn(todosService,'get').and.returnValue(deferred.promise);
```

The code above is completely valid — unlike observables, promises are always asynchronous.

Let me show you three ways to correct the above.

### Using defer()

Those who’d rather stick to promises can return a `defer()` observable that will return a promise with the data.

<Embed src="https://gist.github.com/NetanelBasal/ebe0a97658999106d39ee531a173f857.js" aspectRatio={0.357} caption="todos component spec" />

We use `defer()` to create a block that is only executed when the resulting observable is subscribed to.

### Using Schedulers

Schedulers influence the timing of task execution. You can change the default schedulers of some operators by passing in an extra scheduler argument.

<Embed src="https://gist.github.com/NetanelBasal/4ffc7e3cfd3c545584f15dc38c2ab994.js" aspectRatio={0.357} caption="todos component spec" />

The `async` scheduler schedules tasks asynchronously by setting them on the JavaScript event loop queue. This scheduler is best used to delay tasks in time or schedule tasks at repeating intervals.

You can read more about schedulers [here](https://blog.strongbrew.io/what-are-schedulers-in-rxjs/).

### Using jasmine-marbles

RxJS marble testing is an excellent way to test both simple and complex observable scenarios.

Marble testing uses a similar marble language to specify your tests’ observable streams and expectations.

<Embed src="https://gist.github.com/NetanelBasal/cb47b8f6aa1f910bf94384a333124c33.js" aspectRatio={0.357} caption="todos component spec" />

This test defines a _cold_ observable that waits two frames (`--`), emits a value (`x`), and completes (`|`). In the second argument, you can map the value marker (`x`) to the emitted value (`todos`).

Here are a few more resources to learn about marble testing:

1.  The official [docs](https://github.com/ReactiveX/rxjs/blob/master/doc/writing-marble-tests.md).
2.  [Marble testing Observable Introductio](https://medium.com/@bencabanes/marble-testing-observable-introduction-1f5ad39231c)n.
3.  [RxJS Marble Testing: RTFM](https://blog.angularindepth.com/rxjs-marble-testing-rtfm-a9a6cd3db758)

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Vue and JS!_

### 👂🏻 Last but Not Least, Have you Heard of Akita?

Akita is a state management pattern that we’ve developed here in Datorama. It’s been successfully used in a big data production environment for over seven months, and we’re continually adding features to it.

Akita encourages simplicity. It saves you the hassle of creating boilerplate code and offers powerful tools with a moderate learning curve, suitable for both experienced and inexperienced developers alike.

I highly recommend checking it out.

[**🚀 Introducing Akita: A New State Management Pattern for Angular Applications**  
_Every developer knows state management is difficult. Continuously keeping track of what has been updated, why, and…_netbasal.com](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8 "https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8")[](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8)
