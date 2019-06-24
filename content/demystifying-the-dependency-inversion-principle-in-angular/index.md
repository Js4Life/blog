---
title: "Demystifying the Dependency Inversion Principle in Angular"
description: "In this article, I’d like to talk about the letter D in the SOLID principles and how we can implement it in Angular. This D refers to the Dependency Inversion Principle If you are not familiar with…"
date: "2018-02-19T20:05:17.815Z"
categories: 
  - Software Development
  - Angular
  - Web Development
  - JavaScript

published: true
canonical_link: https://netbasal.com/demystifying-the-dependency-inversion-principle-in-angular-a2daca9b05ee
redirect_from:
  - /demystifying-the-dependency-inversion-principle-in-angular-a2daca9b05ee
---

![](./asset-1.jpeg)

In this article, I’d like to talk about the letter D in the [SOLI**D**](https://en.wikipedia.org/wiki/SOLID_%28object-oriented_design%29) principles and how we can implement it in Angular. This D refers to the Dependency Inversion Principle

If you are not familiar with the SOLID principles, I highly recommend you learn them. They will make your code designs more understandable, flexible and maintainable.

Let’s assume you need to implement web sockets in your application. Your first impulse might be to do the following:

<Embed src="https://gist.github.com/NetanelBasal/5efffa2720bb3096ef2643ac0e8b4bfe.js" aspectRatio={0.357} caption="" />

At first glance, this looks accurate. Yes, it will work, but there is still a problem with this thinking.

We’ve coupled our application to a specific implementation, in this case to [stomp](https://github.com/jmesnil/stomp-websocket), but what if, in the future, we want to change it and use a different implementation, for example, socket.io? To do that, we’ll have to change our entire codebase.

This is where the Dependency Inversion Principle comes to the rescue.

This principle’s primary suggestion is this:

> Depend on abstractions, not on concretions.

This entire principle is about **decoupling code**. Let’s think for a moment, do the components need to have knowledge about how we connect our web sockets? The answer is NO.

They just need to know there is some web socket connection they can work with.

Let’s fix our design by following the Dependency Inversion Principle.

<Embed src="https://gist.github.com/NetanelBasal/74a93eea9a47c1e1089bddd869283241.js" aspectRatio={0.357} caption="Dependency Inversion Principle" />

We’ve created an abstraction. We have our high-level class, the `SocketService`, which is unconcerned with the details. We also have the low-level class that _is_ concerned with details and specifics.

Now, thanks to the Angular Dependency Injection mechanism, if we want to swap the implementation for our entire application, we can do the following:

<Embed src="https://gist.github.com/NetanelBasal/f0f04ec5fe30ce01053d984a97a438c9.js" aspectRatio={0.357} caption="Socket IO Implementation" />

And we can do all that without changing anything in our codebase. That’s very powerful.

But it does not end here. Let’s assume, for some reason, one of our components needs to work with some micro service that works with NodeJS and has nothing to do with our primary server, which is written in Java.

We can easily use an appropriate implementation, for example:

<Embed src="https://gist.github.com/NetanelBasal/3eb04c921e8f805ab2abfdf9a15b75e5.js" aspectRatio={0.357} caption="Some component" />

It sounds like a rare thing, but do not stick to our specific example.

The last thing worth noting is that in other “real” OOP languages, like Java or PHP for example, this concept is achieved with Interface rather than Class, (You’ve probably heard the term “Coding to an Interface”)

In typescript, we can’t use Interface, because Interface doesn’t have a runtime representation. It’s available only during compile-time, so we must use a Class.

### Summary

We learned about the Dependency Inversion Principle, and we saw how not to tie our application to a specific implantation, but rather an abstraction. This resulted in a more maintainable codebase.

### 👂🏻 **Last but Not Least, Have you Heard of Akita?**

Akita is a state management pattern that we’ve developed here in Datorama. It’s been successfully used in a big data production environment for over seven months, and we’re continually adding features to it.

Akita encourages simplicity. It saves you the hassle of creating boilerplate code and offers powerful tools with a moderate learning curve, suitable for both experienced and inexperienced developers alike.

I highly recommend checking it out.

[**🚀 Introducing Akita: A New State Management Pattern for Angular Applications**  
_Every developer knows state management is difficult. Continuously keeping track of what has been updated, why, and…_netbasal.com](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8 "https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8")[](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8)

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Vue and JS!_
