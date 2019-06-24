---
title: "Create and Test Decorators in JavaScript"
description: "I remember the first time I saw this sexy symbol. Yes, you know what I’m talking about. The at sign (@), above or before class, property or a method — a Decorator. Decorators are a mechanism for…"
date: "2017-10-02T16:06:18.242Z"
categories: 
  - JavaScript
  - Typescript
  - Web Development
  - Angular2
  - React

published: true
canonical_link: https://netbasal.com/create-and-test-decorators-in-javascript-85e8d5cf879c
redirect_from:
  - /create-and-test-decorators-in-javascript-85e8d5cf879c
---

![](./asset-1.jpeg)

I remember the first time I saw this sexy symbol. Yes, you know what I’m talking about. The at sign (@), above or before class, property or a method — a **Decorator**.

Decorators are a mechanism for observing, modifying, or replacing classes, methods or properties in a declarative fashion.

Decorators are a [proposed](https://tc39.github.io/proposal-decorators/) standard in ECMAScript2016. In Typescript, we can enable them by setting the “experimentalDecorators” compiler flag, or with _babel_ by installing the [babel-plugin-transform-decorators](https://babeljs.io/docs/plugins/transform-decorators/) plugin.

Creating decorators is actually quite easy. Let’s explore the various decorators.

### Class Decorators

A Class decorator is a function that takes the constructor of the class as the only parameter. If the class decorator returns a value, it will replace the class declaration with the provided constructor function, i.e., override the constructor.

Let’s see an example of how to override the constructor.

<Embed src="https://gist.github.com/NetanelBasal/0523a7278939555808db10a5c3e96889.js" aspectRatio={0.357} caption="" />

We are returning a brand new Class that extends the original constructor function, in our case, the `HelloComponent`.

Let’s see an example of how to decorate the existing constructor. Let’s say we work with React and we want to know when React calls the `render` method.

<Embed src="https://gist.github.com/NetanelBasal/f222ae90d043a46831d7f32d8b4426ce.js" aspectRatio={0.357} caption="" />

We start by saving a reference to the original method, create a new one, call what we need and return the original method, letting React do its magic.

You can find a real-life example in [this](https://github.com/NetanelBasal/ngx-auto-unsubscribe) repository.

### Method Decorators

A Method decorator is a function that takes three parameters.

#### The target:

Either the constructor function of the class for a static member, or the prototype of the class for an instance member.

#### The key:

The method name.

#### The descriptor:

The Property [Descriptor](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor) for the method.

Let’s see an example of how to decorate an existing method with the `setTimeout` API. But before we can proceed, if you remember, the `setTimeout` function takes a number of milliseconds to wait before executing the code, so we need to learn how to pass options to a decorator by using a Decorator Factory.

> _A Decorator Factory is simply a function that returns the expression that will be called by the decorator at runtime._

Or in plain English, function that returns function.

<Embed src="https://gist.github.com/NetanelBasal/89eeaced3448ebaecfc559e6cc1e105d.js" aspectRatio={0.357} caption="" />

We can get a reference to the original method from the `descriptor` value property.  
Then, we will override the original value and create a new function that wraps the original with `setTimeout`.

Remember that you can also use a method decorator with `static` methods, for example:

<Embed src="https://gist.github.com/NetanelBasal/2448b7c00134ba172a463126f30d1366.js" aspectRatio={0.357} caption="" />

The only difference in this case is you will get the constructor function of the class and not the `prototype` of the class.

You can find a real-life examples in [this](https://github.com/NetanelBasal/helpful-decorators) repository.

### Property Decorators

A Property Decorator is declared just before a property declaration. Same as the others, it’s a function that takes two parameters.

#### The target:

Either the constructor function of the class for a static member, or the prototype of the class for an instance member.

#### The key:

The property name.

Let’s see an example of how to decorate a property.

<Embed src="https://gist.github.com/NetanelBasal/7162edad7fc9687a1fab19ba759fbec1.js" aspectRatio={0.357} caption="" />

The `logProperty` decorator above redefines the decorated property on the object. We can define a new property to the constructor’s `prototype` by using the `Object.defineProperty()` method. Here, we’re using `get` to return the value and log it. Secondly, we’re using `set` to directly write a value to the internal property and log it.

We can also use the new [Reflect](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect) API instead of the `defineProperty()` method.

<Embed src="https://gist.github.com/NetanelBasal/49d080a9cc48fcbfed9e3a09b11b758c.js" aspectRatio={0.357} caption="" />

Now we can use our decorator like this:

<Embed src="https://gist.github.com/NetanelBasal/2516a52a19ba5b66a74abeb467c4d764.js" aspectRatio={0.357} caption="" />

You can find a real-life example in [this](https://github.com/gugamm/localstorage-decorator) repository.

### Parameter Decorators

I’m going to skip this explanation because it’s the least common and it usually comes in conjunction with a method decorator. You can find a detailed example in the official [documentation](https://www.typescriptlang.org/docs/handbook/decorators.html).

### Composing Decorators

When multiple decorators apply to a single declaration, their evaluation is similar to function [composition](https://en.wikipedia.org/wiki/Function_composition) in mathematics. For example, the following code:

<Embed src="https://gist.github.com/NetanelBasal/70a8d8d8ddc3fdebef934c58d343c911.js" aspectRatio={0.357} caption="" />

is equivalent to `decoratorTwo(decoratorOne(someMethod))`.

---

### Testing Decorators

I’m going to use [jest](https://facebook.github.io/jest/) for testing, but you can use whatever test library you like. We have two ways to test decorators.

1.  Use them in your tests like in your code.

<Embed src="https://gist.github.com/NetanelBasal/cbf60d5b4ba14954642616a64c7d3e70.js" aspectRatio={0.357} caption="" />

2\. Since decorators are just functions we can test them like any other function.

<Embed src="https://gist.github.com/NetanelBasal/bd61893fa922b5d6ee50f99849ec459d.js" aspectRatio={0.357} caption="" />

Let’s close the article by testing the `@timeout` decorator.

<Embed src="https://gist.github.com/NetanelBasal/a53ff3515f73de063805c7485f5a5866.js" aspectRatio={0.357} caption="" />

We are passing the necessary parameters manually to the `timeout` decorator and leveraging the `[useFakeTimers](http://facebook.github.io/jest/docs/en/timer-mocks.html#docsNav)` feature from jest to check if our `setTimeout` function was called.

### Wrapping Up

You can leverage decorators in your apps and create powerful things with them. Decorators are not only for frameworks or libraries, they are worth the time to learn as they can make your code more extensible and even more readable. Decorators also promote code reuse. Give them a try sometime soon 🙏

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Akita and JS!_

### 👂🏻 **Last but Not Least, Have you Heard of Akita?**

Akita is a state management pattern that we’ve developed here in Datorama. It’s been successfully used in a big data production environment for over seven months, and we’re continually adding features to it.

Akita encourages simplicity. It saves you the hassle of creating boilerplate code and offers powerful tools with a moderate learning curve, suitable for both experienced and inexperienced developers alike.

I highly recommend checking it out.

[**🚀 Introducing Akita: A New State Management Pattern for Angular Applications**  
_Every developer knows state management is difficult. Continuously keeping track of what has been updated, why, and…_netbasal.com](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8 "https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8")[](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8)

### Wrapping Up

You can leverage decorators in your apps and create powerful things with them. Decorators are not only for frameworks or libraries, they are worth the time to learn as they can make your code more extensible and even more readable. Decorators also promote code reuse. Give them a try sometime soon 🙏

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Vue and JS!_
