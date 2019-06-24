---
title: "Angular 2 — Making your component auth-aware"
description: "When dealing with cross cutting concerns, AOP (Aspect Oriented Programming) is your best friend. In Angular 2, this is achievable by writing your own Directive. I’m working on my Angular 2 project…"
date: "2016-10-20T15:17:49.293Z"
categories: 
  - JavaScript
  - Angularjs
  - Angular2
  - Web Development

published: true
canonical_link: https://netbasal.com/angular-2-making-your-component-auth-aware-982054048c8a
redirect_from:
  - /angular-2-making-your-component-auth-aware-982054048c8a
---

_By: Netanel Basal (Angular Expert) and Yaron Biton, misterBIT.co.il CTO_

![](./asset-1.png)

#### **Aspect Oriented Programming in Angular 2**

When dealing with cross cutting concerns, AOP (Aspect Oriented Programming) is your best friend. In Angular 2, this is achievable by writing your own Directive.

#### **Background and scope**

I’m working on my Angular 2 project with `ng2redux` which is based on the `Redux` library.   
After implementing the SignIn/Signout logic with all the `Redux` good stuff like actions, reducers, and one-way direction flow, I came to the part that I needed to show or hide templates based on the user session status.

#### **The Naïve\[rbose\] approach**

My first approach was in every component to subscribe to the session observable from my store and render what I need based on the status.

So for example in my navigation component:

<Embed src="https://gist.github.com/NetanelBasal/43694cdc7f7b7277c44cd37953335172.js" aspectRatio={0.357} caption="" />

And then continue following this approach in **all** my components that needed this kind of functionality.

### **It Feels Smelly.**

1.  Not DRY; a cross cutting concern is handled in every component.
2.  Creates many observables.

### The solution:

Directive for managing component behavior triggered by login/logout events.

When dealing with cross cutting concerns, AOP (Aspect Oriented Programming) is your best friend. In Angular 2, this is achievable by writing your own Directive. In this case, we are talking about a Structural Directiv_e_ (such as \*ngIf):

> “ A Structural directive changes the DOM layout by adding and removing DOM elements. ”

Let me show you the final code and then we can go over the details:

<Embed src="https://gist.github.com/NetanelBasal/763e02c4fff7e645afb7a64b327cbbbf.js" aspectRatio={0.357} caption="" />

#### **ShowIfLoggedInDirective Explained**

We need access to the template (`TemplateRef`) and something that can render its contents (`ViewContainerRef`), so we inject both into our component.

**Note:** If you are asking yourself from where this `templateRef` coming from, the asterisk in the directive is only syntactic sugar for this code for example:

<Embed src="https://gist.github.com/NetanelBasal/79aea7ecb206b998690d242353a3b7c6.js" aspectRatio={0.357} caption="" />

So the `templateRef` is just the template tag. Piece-of-mind has been successfully restored.

There is this bit where we subscribe to the `isLoggedIn` value from the `ng2redux` store, (run this function whenever the `isLoggedIn` status change) this is something that is not for this article to cover. Do check out how RxJS and Redux are best buddies to create flux based architectures in Angular.

### **Victorious!**

Now we can use this directive everywhere we want, this `auth-aware` aspect is handled in a good DRY way:

<Embed src="https://gist.github.com/NetanelBasal/4ef4c720463602b3fda083e02f1c1578.js" aspectRatio={0.357} caption="" />

#### Cheers!

![](./asset-2.png)

_☞_ **_Please tap or click “︎_**❤” _to help to promote this piece to others._
