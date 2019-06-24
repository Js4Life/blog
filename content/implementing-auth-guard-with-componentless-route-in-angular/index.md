---
title: "Implementing Auth Guard with Componentless Route in Angular"
description: "I see a lot of different implementations of Auth Guards around the web. \nThe implementation is quite simple. In most cases, you will find yourself doing something like this: As you see, we need to…"
date: "2017-05-15T15:55:17.818Z"
categories: 
  - JavaScript
  - Angular2
  - Angularjs
  - Web Development
  - Programming

published: true
canonical_link: https://netbasal.com/implementing-auth-guard-with-componentless-route-in-angular-b50a21f3bd77
redirect_from:
  - /implementing-auth-guard-with-componentless-route-in-angular-b50a21f3bd77
---

![](./asset-1.jpeg)

I see a lot of different implementations of Auth Guards around the web.   
The implementation is quite simple. In most cases, you will find yourself doing something like this:

<Embed src="https://gist.github.com/NetanelBasal/c8e2521b33ec1318bd84e943c76fb5c3.js" aspectRatio={0.357} caption="" />

And then you will add the guard to any protected route.

<Embed src="https://gist.github.com/NetanelBasal/151df0656068d3a11cb950faa2df4866.js" aspectRatio={0.357} caption="" />

As you see, we need to repeat the same line of code whenever there is a new protected route. Of course, it’s not the end of the world, but we can do better and not write redundant configuration.

Let’s use a `componentless` route as a parent. `Componentless` routes consume URL segments without instantiating components.

> Componentless routes are useful when the same configuration apply to all child routes

For example guards, resolvers, params, etc.,

Now let’s see how we can achieve the same thing with a `componentless` route.

<Embed src="https://gist.github.com/NetanelBasal/4b3ae9566bf525fcf0f416ec52c1ff46.js" aspectRatio={0.357} caption="" />

And we also need to change the method name in our guard.

<Embed src="https://gist.github.com/NetanelBasal/4a0a08f508fc0eebc4b4c8baa0b170d1.js" aspectRatio={0.357} caption="" />

Now every time you navigate to `/todos` or `/blog` route, the `AuthGuard` will run. 😎

You can also achieve the same thing in Angular version 4 with the following code:

<Embed src="https://gist.github.com/NetanelBasal/7d5d99b9549a5d201f1590e6b81b4bc7.js" aspectRatio={0.357} caption="" />

And that’s how it looks with lazy-load routes:

<Embed src="https://gist.github.com/NetanelBasal/4bfde80eb048adcc967c8a2c525b0daa.js" aspectRatio={0.357} caption="" />

### 🔥 **Last but Not Least, Have you Heard of Akita?**

Akita is a state management pattern that we’ve developed here in Datorama. It’s been successfully used in a big data production environment, and we’re continually adding features to it.

Akita encourages simplicity. It saves you the hassle of creating boilerplate code and offers powerful tools with a moderate learning curve, suitable for both experienced and inexperienced developers alike.

I highly recommend checking it out.

[**🚀 Introducing Akita: A New State Management Pattern for Angular Applications**  
_Every developer knows state management is difficult. Continuously keeping track of what has been updated, why, and…_netbasal.com](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8 "https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8")[](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8)

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Vue and JS!_
