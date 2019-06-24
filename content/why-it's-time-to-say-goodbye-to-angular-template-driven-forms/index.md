---
title: "Why It’s Time to Say Goodbye to Angular Template-Driven Forms"
description: "Angular gave us the powerful tool that is reactive forms. As a result, I’ve opted to stop using template-driven forms altogether. Here’s why: As our template grows it becomes harder to understand the…"
date: "2018-05-15T15:24:19.493Z"
categories: 
  - JavaScript
  - Angular
  - Web Development
  - Forms
  - Angularjs

published: true
canonical_link: https://netbasal.com/why-its-time-to-say-goodbye-to-angular-template-driven-forms-350c11d004b
redirect_from:
  - /why-its-time-to-say-goodbye-to-angular-template-driven-forms-350c11d004b
---

![](./asset-1.jpeg)

Angular gave us the powerful tool that is reactive forms. As a result, I’ve opted to stop using template-driven forms altogether. Here’s why:

### Readability

As our template grows it becomes harder to understand the component’s form structure. For example, let’s take a basic login form:

<Embed src="https://gist.github.com/NetanelBasal/92fd081a01e3ca480f4d7714f0771180.js" aspectRatio={0.357} caption="login.component.html" />

If we use template-driven forms, we’ll have to make an effort to understand the form structure. Let’s compare this with the same form built using reactive forms:

<Embed src="https://gist.github.com/NetanelBasal/228828c3a25d4e261b2868a637d5f31b.js" aspectRatio={0.357} caption="" />

With this approach we can see immediately the structure of our form in a very clean way, without rooting around the template file.

### Visibility

With template-driven forms getting a reference to the form or its controls is more verbose.

<Embed src="https://gist.github.com/NetanelBasal/a516c67098277c33b9113d2d695c39f8.js" aspectRatio={0.357} caption="" />

We need to use template variables to get access to the form value or `ViewChild()` to get access to the form instance in our component.

Let’s compare this with the reactive approach:

<Embed src="https://gist.github.com/NetanelBasal/c16bcfd9ef60253604ae2e9ef0302e57.js" aspectRatio={0.357} caption="" />

We can access the form in our component at any point and we don’t need to use template variables to get its values. This makes it easier to pass form controls as `inputs()` to other components.

### Validators

If we want to use custom validators with template-driven forms, we must create a custom directive in addition to the validation functionality. For example:

<Embed src="https://gist.github.com/NetanelBasal/5ed8d79a2c38bcfec9e6ece138c8dd3c.js" aspectRatio={0.357} caption="Custom validator" />

Whereas with reactive forms we can use the function directly:

<Embed src="https://gist.github.com/NetanelBasal/57a70aa084f5ea50be87c810246280d9.js" aspectRatio={0.357} caption="Custom validator" />

Another important thing that you should pay attention to is that unlike reactive forms, **template-driven forms are asynchronous.**

One of the side effects of this is that if you have a validator that depends on another control, you should add a condition to ensure that control exists. For example:

<Embed src="https://gist.github.com/NetanelBasal/d5f7bbd7605183bfebec9599dc15f780.js" aspectRatio={0.357} caption="" />

Whereas with reactive forms you can omit that condition.

### It’s Just JavaScript

I like to compare reactive forms to what is commonly said in the world of React — you have the power of JS. Reactive forms are pure JS, which makes them the winners when it comes to reusability and dynamic use.

### Two way binding

Template-driven forms expose a two-way binding option which breaks the one-way data flow and immutability principles and makes your code bug-prone and hard to reason about.

### Testability

When using template-driven forms, we must create a component in order to test our forms and use one of the async testing helpers that Angular provides. When using reactive forms, this is not required, simplifying the testing process.

### Why NOT use both?

There are four reasons I can think of:

1.  Steep learning curve.
2.  We’ll need to load both, so our bundle will get slightly bigger.
3.  We can’t predict which strategy the developer will choose so it becomes harder to go over pull requests.
4.  In my opinion it’s always good to stick to a single paradigm, unless there is a good reason not to.

I want to be fair and give you a different point of view from [Ward Bell](https://medium.com/@wardbell), where he claims the exact opposite. You can read his opinion in the following comment:

[**Wow. You know I love your articles. We are almost always on the same wave length.**  
_Not this time. This article “wrong” on every point. I hardly know how to begin. I’ll just vent and see what you say._medium.com](https://medium.com/@wardbell/wow-you-know-i-love-your-articles-we-are-almost-always-on-the-same-wave-length-11e0d53f7da3 "https://medium.com/@wardbell/wow-you-know-i-love-your-articles-we-are-almost-always-on-the-same-wave-length-11e0d53f7da3")[](https://medium.com/@wardbell/wow-you-know-i-love-your-articles-we-are-almost-always-on-the-same-wave-length-11e0d53f7da3)

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Vue and JS!_

### **Things to not miss**:

[**🚀 Introducing Akita: A New State Management Pattern for Angular Applications**  
_Every developer knows state management is difficult. Continuously keeping track of what has been updated, why, and…_netbasal.com](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8 "https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8")[](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8)

[**datorama/akita**  
_akita - 🚀 Simple and Effective State Management for Angular Applications_github.com](https://github.com/datorama/akita "https://github.com/datorama/akita")[](https://github.com/datorama/akita)

[**NetanelBasal/spectator**  
_spectator - 👻 Angular Tests Made Easy 🤓_github.com](https://github.com/NetanelBasal/spectator "https://github.com/NetanelBasal/spectator")[](https://github.com/NetanelBasal/spectator)
