---
title: "Angular Components: Tips and Tricks"
description: "One of my latest assignments was to develop a custom drop-down component which supports both single and multiple selections. Let’s begin by preparing the ground with two key components — we need a…"
date: "2018-06-20T13:31:51.521Z"
categories: 
  - JavaScript
  - Web Development
  - Angular
  - Angularjs

published: true
canonical_link: https://netbasal.com/angular-components-tips-and-tricks-d9e725871d58
redirect_from:
  - /angular-components-tips-and-tricks-d9e725871d58
---

![](./asset-1.jpeg)

One of my latest assignments was to develop a custom drop-down component which supports both single and multiple selections.

As always, I will share some tips with you in the hope that you will learn new things.

Let’s begin by preparing the ground with two key components — we need a `dato-select` and `dato-option` components.

<Embed src="https://gist.github.com/NetanelBasal/448f3d1698c917a317d1f93d48ce8c13.js" aspectRatio={0.357} caption="dato-select" />

### #Tip 1

Since it’s a drop-down that supports multiple selection, I need to add a checkmark that indicates whether an option is selected.

One way to achieve this is by adding a `type` input() and according to its value show and hide the markup.

<Embed src="https://gist.github.com/NetanelBasal/9ad5e9291910c1af281b849c24d4ee19.js" aspectRatio={0.357} caption="" />

Yeah, it will work, but the disadvantage of this approach is that we add one more check per option to Angular, so in a dropdown with 10,000 options we added 10,000 more checks, and this is only one `ngIf`; usually it will be accompanied with `ngClass`, `ngStyle`, etc.

In my case performance is very important, so I decided to split the `multi-option` to a different component that inherits from the `single-option` to avoid redundant checks.

Also, there are still parts of the markup that both have in common, so I’ll use template strings to keep my code DRY.

<Embed src="https://gist.github.com/NetanelBasal/30e15af538c1c4caf59d9cf7461f2b8d.js" aspectRatio={0.357} caption="" />

### #Tip 2

The previous change brought with it a new problem. Let’s say I want to use the `multi-select` dropdown:

<Embed src="https://gist.github.com/NetanelBasal/966508b06cfcc5abef3d16cb4852f72a.js" aspectRatio={0.357} caption="" />

This will not work as the `ContentChildren` decorator is looking for `DatoOptionComponent`. If we want a reference to the `option-multi` components we need to add a different `ContentChildren` query.

<Embed src="https://gist.github.com/NetanelBasal/3bdcdb17018399f0e35cf05a33cc01ba.js" aspectRatio={0.357} caption="" />

But now we have to maintain both. Luckily we can solve this more elegantly with the help of Angular DI.

<Embed src="https://gist.github.com/NetanelBasal/3608931ee75d8dbd660c1177226dd924.js" aspectRatio={0.357} caption="" />

We can tell Angular — when you need `DatoOptionComponent` use `DatoOptionMultiComponent`.

### #Tip 3

I was not satisfied yet. I wanted to use the same selector — `dato-option` — for both single and multiple drop-down components. So I changed the `multi-option` selector to be the same.

<Embed src="https://gist.github.com/NetanelBasal/780d606d306c2e9ab599882d625ca2a6.js" aspectRatio={0.357} caption="" />

But now I have a problem. Angular doesn’t allow duplicate selectors and throws an error:

> More than one component matched on this element.  
> Make sure that only one component’s selector can match a given element.

The way I solved it is by leveraging the fact that Angular supports the `:not` CSS selector. (along with other useful [selectors](https://angular.io/api/core/Directive#selector))

<Embed src="https://gist.github.com/NetanelBasal/9ce5c81a40e16a74d6628c3a09fee66f.js" aspectRatio={0.357} caption="" />

Now we can stick with the same selector and add attribute when we need `multi-option`. This feels more appropriate, rather than introducing a brand new selector.

### #Tip 4

The drop-down comes with a search box that is performing an internal search by default. We also need the option to perform a server-side search.

One way to achieve this is to create both an `input()` and `output()`:

<Embed src="https://gist.github.com/NetanelBasal/4970d43a8ee895a02431d5f290064d97.js" aspectRatio={0.357} caption="" />

But it feels like input is redundant and could be avoided; We can make do with knowing if someone is listening to the `search` event and based on that deciding whether to activate the internal search or not.

<Embed src="https://gist.github.com/NetanelBasal/5865e3210479db88dbe3f66d36257d25.js" aspectRatio={0.357} caption="" />

Every event emitter maintains a list of its observers. If we have at least one observer, it means that someone is subscribed.

### 😍 **Have You Tried Akita Yet?**

One of the leading state management libraries, Akita has been used in countless production environments. It’s constantly developing and improving.

Whether it’s entities arriving from the server or UI state data, Akita has custom-built stores, powerful tools, and tailor-made plugins, which help you manage the data and negate the need for massive amounts of boilerplate code. We/I highly recommend you try it out.

[**🚀 Introducing Akita: A New State Management Pattern for Angular Applications**  
_Every developer knows state management is difficult. Continuously keeping track of what has been updated, why, and…_netbasal.com](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8 "https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8")[](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8)

<Embed src="https://stackblitz.com/edit/angular-g8pixk?embed=1" aspectRatio={undefined} caption="" />

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Vue and JS!_
