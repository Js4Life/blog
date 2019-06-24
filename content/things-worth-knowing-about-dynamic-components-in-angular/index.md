---
title: "Things Worth Knowing About Dynamic Components in Angular"
description: "In this article, I would like to answer a question that was asked yesterday on Twitter by Sani Yusuf and also add more interesting points. When we dynamically create a component and insert it into…"
date: "2018-11-16T11:05:20.158Z"
categories: 
  - JavaScript
  - Angular
  - Web Development

published: true
canonical_link: https://netbasal.com/things-worth-knowing-about-dynamic-components-in-angular-166ce136b3eb
redirect_from:
  - /things-worth-knowing-about-dynamic-components-in-angular-166ce136b3eb
---

![](./asset-1.jpeg)

In this article, I would like to answer a question that was asked yesterday on Twitter by [Sani Yusuf](https://medium.com/@saniyusuf) and also add more interesting points.



When we dynamically create a component and insert it into the view by using a `ViewContainerRef`, Angular will invoke each one of the lifecycle hooks except for the `ngOnChanges()` hook.

The reason for that is the `ngOnChanges` hook isn’t called when inputs are set programmatically only by the view.

Here is an example for [dynamic](https://netbasal.com/dynamically-creating-components-with-angular-a7346f4a982d) component creation:

<Embed src="https://gist.github.com/NetanelBasal/d911e9b69c5f1738946880cb10aa5c9b.js" aspectRatio={0.357} caption="" />

So in this case, the answer is Yes, **Angular destroys the component when the parent has been destroyed.**

---

One more important thing to consider is that if we subscribe manually to a component’s `@Output()`, we need to clean it by ourselves otherwise we’ll have a [memory leak](https://netbasal.com/why-its-important-to-unsubscribe-from-rxjs-subscription-a7a6455d6a02). For example:

<Embed src="https://gist.github.com/NetanelBasal/1e0a38df7da041d5c3529dbb3bdf9e57.js" aspectRatio={0.357} caption="" />

---

Let’s change the `HelloComponent` and set its change detection to `onPush` :

<Embed src="https://gist.github.com/NetanelBasal/34b96a8e1ecfc91ee98f005e7c8d9322.js" aspectRatio={0.357} caption="" />

Now, let’s try to change the `name` input and call `detectChanges()` :

<Embed src="https://gist.github.com/NetanelBasal/a1c3199ccb71a20a1a9586527d4ad7a5.js" aspectRatio={0.357} caption="" />

Nothing is happening. But why? We’re running `detectChanges()` explicitly so it should work, right?

The reason is that we’re running `detectChanges()` on the [**host view**](https://blog.angularindepth.com/working-with-dom-in-angular-unexpected-consequences-and-optimization-techniques-682ac09f6866), not on the component itself and because we’re in `onPush` and setting the input [programmatically](https://netbasal.com/a-comprehensive-guide-to-angular-onpush-change-detection-strategy-5bac493074a4) from Angular perspective nothing has changed.

We have two solutions to this problem. We can inject the `CDRef` in our component and call it in the setter:

<Embed src="https://gist.github.com/NetanelBasal/8455790f7b16f6be90069d4abc28d8fc.js" aspectRatio={0.357} caption="" />

Or we can get a reference to the component injector:

<Embed src="https://gist.github.com/NetanelBasal/2d00c7b44739605672760f95d5c9cf82.js" aspectRatio={0.357} caption="" />

The above point also happens with non-dynamic components, but the essential point to take from here is that when we call `ref.changeDetectorRef.detectChanges()` or `ref.hostView.detectChanges()` we are invoking the CDR of the host view.

---

My friend [Max, Wizard of the Web](https://medium.com/@maxim.koretskyi) also add that if we attach the host view to the `ApplicationRef` (portals, etc.), we need to destroy it manually. For example:

<Embed src="https://gist.github.com/NetanelBasal/913edbf1ed494b12d90d193333c22906.js" aspectRatio={0.357} caption="" />

I will also add that in this case, unlike the first case, we need to call `detectChanges()` in order to invoke the `ngOnInit()`.

### 🔥 **Last but Not Least, Have you Heard of Akita?**

Akita is a state management pattern that we’ve developed here in Datorama. It’s been successfully used in a big data production environment, and we’re continually adding features to it.

Akita encourages simplicity. It saves you the hassle of creating boilerplate code and offers powerful tools with a moderate learning curve, suitable for both experienced and inexperienced developers alike.

I highly recommend checking it out.

[**🚀 Introducing Akita: A New State Management Pattern for Angular Applications**  
_Every developer knows state management is difficult. Continuously keeping track of what has been updated, why, and…_netbasal.com](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8 "https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8")[](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8)

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Akita and JS!_
