---
title: "🎩 Automagically Unsubscribe in Angular"
description: "As you probably know when you subscribe to an observable or event in JavaScript, you usually need to unsubscribe at a certain point to release memory in the system. Otherwise, you will have a memory…"
date: "2017-04-11T23:04:14.003Z"
categories: 
  - JavaScript
  - Angular2
  - Web Development
  - Typescript
  - Angularjs

published: true
canonical_link: https://netbasal.com/automagically-unsubscribe-in-angular-4487e9853a88
redirect_from:
  - /automagically-unsubscribe-in-angular-4487e9853a88
---

![](./asset-1.jpeg)

As you probably know when you subscribe to an observable or event in JavaScript, you _usually_ need to unsubscribe at a certain point to release memory in the system. Otherwise, you will have a memory [leak](https://appendto.com/2016/11/avoiding-memory-leaks-in-javascript/).

> A memory leak occurs when a section of memory that is no longer being used is still being occupied needlessly instead of being returned to the OS

In Angular components or directives, you will unsubscribe inside the `ngOnDestroy` lifecycle hook.

For example, if you have a component with three subscriptions:

<Embed src="https://gist.github.com/NetanelBasal/e2e1035d5a78a817d7e1acfb44aeac72.js" aspectRatio={0.357} caption="" />

You need to create the `ngOnDestroy` method and unsubscribe from each.

OK, that’s nice, but I want to **automate** the unsubscribe process. What if we could create a class `decorator` that will do the work for us? Maybe something like this:

<Embed src="https://gist.github.com/NetanelBasal/e2520a5d0476f8da25dd6c42e9d8b4a6.js" aspectRatio={0.357} caption="" />

Let’s create a class decorator named `AutoUnsubscribe` to clean our code.

In `[typescript](https://www.typescriptlang.org/docs/handbook/decorators.html)` or `[babel](https://babeljs.io/docs/plugins/transform-decorators/)`, a class decorator is just a function that takes one parameter, the `constructor` of the decorated class.

The class decorator is applied to the `constructor` of the class and can be used to observe, **modify**, or replace a class definition.

<Embed src="https://gist.github.com/NetanelBasal/eaf533bbc9c247d54a08fe5ca3739d22.js" aspectRatio={0.357} caption="" />

🤓 There are three simple steps here:

1.  Save a reference to the original `ngOnDestroy` function.
2.  Create our version of `ngOnDestroy`, loop over the `class` properties and invoke the `unsubscribe()` function if it exists.
3.  Invoke the original `ngOnDestroy` function if it exists.

That’s all, piece of 🍰.

### 😁 But…

But wait a minute, what if for some crazy reason you need to exclude subscriptions? for example you don’t want to unsubscribe from the `$two` subscription when the component is destroyed.

In this case we need to pass an argument to our decorator ( an array of excluded properties ), so we need to use a Decorator Factory:

> _A Decorator Factory is simply a function that returns the expression that will be called by the decorator at runtime._

<Embed src="https://gist.github.com/NetanelBasal/9f02ce7674e5e4a59415a4f6ba431775.js" aspectRatio={0.357} caption="" />

We are just checking that the `property` name is not in the `blacklist` array before invoking the `unsubscribe()` function.

Now we can use our decorator like this:

<Embed src="https://gist.github.com/NetanelBasal/76f864886a5e55ee4ab59f595caeaabc.js" aspectRatio={0.357} caption="" />

😃 Now we are done!

You can find the [decorator](https://github.com/NetanelBasal/ngx-auto-unsubscribe) here. If you have further improvements, please make a pull request.

---

If you want to be more declarative with the `[takeUntil](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/takeuntil.md)` operator, you can also check my [tiny](https://github.com/NetanelBasal/angular2-take-until-destroy) class decorator that will you give you the ability to do this:

<Embed src="https://gist.github.com/NetanelBasal/925c1e95abf473edd2a17d4c226a3801.js" aspectRatio={0.357} caption="" />

Do not be afraid to look at the source code, it’s just a few lines.

If you liked this article, check out my previous one — [Make your Code Cleaner with Decorators.](https://medium.com/front-end-hacking/javascript-make-your-code-cleaner-with-decorators-d34fc72af947)

### Conclusion:

You can leverage decorators in your apps and create powerful things with them. Decorators are not only for frameworks or libraries, so be creative and start using them. You can explore the different decorators [here](http://blog.wolksoftware.com/decorators-reflection-javascript-typescript).

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Vue and JS!_
