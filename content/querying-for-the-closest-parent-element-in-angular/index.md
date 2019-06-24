---
title: "Querying For The Closest Parent Element in Angular"
description: "Imagine that you have a component with a nested structure. You’ve found a case where one of the children needs to run some functionality on one of its closest parents. Let’s say the widget-item…"
date: "2017-11-22T16:40:32.149Z"
categories: 
  - JavaScript
  - Angular
  - Web Development
  - Jquery

published: true
canonical_link: https://netbasal.com/querying-for-the-closest-parent-element-in-angular-b2554d60c47e
redirect_from:
  - /querying-for-the-closest-parent-element-in-angular-b2554d60c47e
---

![](./asset-1.jpeg)

Imagine that you have a component with a nested structure. You’ve found a case where one of the children needs to run some functionality on one of its closest parents.

To demonstrate this, let’s assume we have the following structure:

<Embed src="https://gist.github.com/NetanelBasal/af9e94686ea50c376934783461ddbf5e.js" aspectRatio={0.357} caption="" />

Let’s say the `widget-item` element needs to add a specific class to the closest parent, which matches the `widget-content` class selector when a specific condition is true.

Your initial instinct will probably be to use the method we all know from JQuery — the `closest()` method.

For example:

<Embed src="https://gist.github.com/NetanelBasal/95b93e2f1ed0d2d3498fcfecd06bd686.js" aspectRatio={0.357} caption="" />

The above code will work, but as professional Angular developers, we need to use the tools Angular provides us with to achieve the same result. Let’s see how we can do this.

First, we need to create a directive whose selector will match the required parent element. I like to call these ‘Query Directives’.

<Embed src="https://gist.github.com/NetanelBasal/059bc9a4e9540df1ac3276d1f9cbaa7d.js" aspectRatio={0.357} caption="" />

The next thing we need to do is to expose an API to add our custom class to the element.

<Embed src="https://gist.github.com/NetanelBasal/89cdcdab793ceee50f1b185718ad92ea.js" aspectRatio={0.357} caption="" />

Here, we are using the `HostBinding` decorator to add the `overflow` class to our host component.

Now, we can obtain a reference to our directive inside the `widgetItem` component, using the `Host` decorator.

> Specifies that an injector should retrieve a dependency from any injector until reaching the **host** element of the current component.

and call the `addOverflow()` method when necessary.

<Embed src="https://gist.github.com/NetanelBasal/ee21ff9803df080a3bb5ec6632816cc8.js" aspectRatio={0.357} caption="" />

<Embed src="https://stackblitz.com/edit/angular-peetss?embed=1" aspectRatio={undefined} caption="Live demo from StackBlitz. Try editing the code yourself." />

### 👂🏻 **Last but Not Least, Have you Heard of Akita?**

Akita is a state management pattern that we’ve developed here in Datorama. It’s been successfully used in a big data production environment for over seven months, and we’re continually adding features to it.

Akita encourages simplicity. It saves you the hassle of creating boilerplate code and offers powerful tools with a moderate learning curve, suitable for both experienced and inexperienced developers alike.

I highly recommend checking it out.

[**🚀 Introducing Akita: A New State Management Pattern for Angular Applications**  
_Every developer knows state management is difficult. Continuously keeping track of what has been updated, why, and…_netbasal.com](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8 "https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8")[](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8)

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Vue and JS!_
