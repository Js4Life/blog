---
title: "Create Modular Components with Angular Structural Directives"
description: "When creating components, particularly core components, our goal is to make them as modular and as flexible and grant the consumers a high level of control over what they can pass. For example, let’s…"
date: "2019-03-06T10:13:11.486Z"
categories: 
  - Angular
  - JavaScript
  - Web Development
  - Angularjs
  - Components

published: true
canonical_link: https://netbasal.com/create-modular-components-with-angular-structural-directives-1a5198d9ab7d
redirect_from:
  - /create-modular-components-with-angular-structural-directives-1a5198d9ab7d
---

![](./asset-1.png)

When creating components, particularly core components, our goal is to make them as modular and as flexible and grant the consumers a high level of control over what they can pass.

For example, let’s say we build a generic error component. What we want is to give our consumers the flexibly to create it by using one of three options:

-   They can choose to use the default text value
-   They can choose to use their own text which can be static or dynamic
-   They can choose to pass their own template

Let’s create the error component in question, and see how we can meet the above requirements:

<Embed src="https://gist.github.com/NetanelBasal/6309cea0f3e6459779364383abfe1e02.js" aspectRatio={0.357} caption="" />

We have an `error` input that accepts either a `string` or a reference to a `template`. We also define a default error message in case the consumer doesn’t pass anything.

Additionally, we have the `isTemplate` getter, which performs a simple check to let us know whether we need to render a custom template; It’s as simple as that.

Now we can use the component whichever way we need:

<Embed src="https://gist.github.com/NetanelBasal/8520e203174c459f836c6af3ba4c9b0d.js" aspectRatio={0.357} caption="" />

So far so good, but our inner code still isn’t reusable, and we need to repeat it for any component that requires this kind of functionality. A better approach would be to create a custom structural directive, that ensures our code is DRY and reusable.

We want to be able to write something like the following:

<Embed src="https://gist.github.com/NetanelBasal/9ef8b8d49f97edfb766d337e8a79988c.js" aspectRatio={0.357} caption="" />

Much cleaner. Let’s look at the directive’s implementation:

<Embed src="https://gist.github.com/NetanelBasal/b1e6743dc8d286de98c062b1cae8f1ad.js" aspectRatio={0.357} caption="" />

The directive is pretty straightforward; If the type of the `input` value is `TemplateRef` we render it; otherwise, we render the default template, which in our example is: `<p>{{error}}</p>` which can display the default error message or a custom one.

If you’re not familiar with Angular structural directives, I recommend the following resources:

[**The Power of Structural Directives in Angular**  
_What’s a Structural Directive?_netbasal.com](https://netbasal.com/the-power-of-structural-directives-in-angular-bfe4d8c44fb1 "https://netbasal.com/the-power-of-structural-directives-in-angular-bfe4d8c44fb1")[](https://netbasal.com/the-power-of-structural-directives-in-angular-bfe4d8c44fb1)

[**Understanding Angular Structural Directives**  
_In this article, we’ll create a carousel component with the use of custom structural directive. We’ll explore…_netbasal.com](https://netbasal.com/understanding-angular-structural-directives-659acd0f67e "https://netbasal.com/understanding-angular-structural-directives-659acd0f67e")[](https://netbasal.com/understanding-angular-structural-directives-659acd0f67e)

---

As you can see, the template option requires creating a local variable and passing it to the directive’s input. We can also add support for the following variation:

<Embed src="https://gist.github.com/NetanelBasal/d70ac71ea5772383f0a870aff3571743.js" aspectRatio={0.357} caption="" />

We do so by using the `ContentChild` decorator to obtain a reference to the template:

<Embed src="https://gist.github.com/NetanelBasal/8d29008da7cca476f71d264c573c9994.js" aspectRatio={0.357} caption="" />

Now we can truly support any method that fits our consumers’ needs.

### What About ng-content?

There are a few reasons why I prefer this way over using `ng-content`:

-   `ng-content` makes it harder to support a default content. Yes, you can find workarounds to do it, like checking the `childNodes.length` or by using the CSS :`empty` selector, but with `ng-template` it’s more elegant.
-   In some cases, `ng-content` can lead to unexpected side-effects, as the the host doesn’t have control over the content. For example, a lazy content (`*ngIf`) will not prevent Angular from creating the component instances. You can read more about it [here](https://medium.com/claritydesignsystem/ng-content-the-hidden-docs-96a29d70d11b).
-   While we didn’t use this feature in our example, `ng-template` is more powerful, as it gives us the option to pass a **context**.

[**Angular demo runner**  
_Online angular editor for building demo._ng-run.com](https://ng-run.com/edit/dyIrRsmviUFE8gPVF9FY "https://ng-run.com/edit/dyIrRsmviUFE8gPVF9FY")[](https://ng-run.com/edit/dyIrRsmviUFE8gPVF9FY)

### Testing Our Directive with Spectator

It’s important to test our directive, and ensure it works as expected for all the input variations.

Let’s test the directive with [Spectator](https://github.com/NetanelBasal/spectator). If you’re not familiar with Spectator, it helps you get rid of all the boilerplate grunt work, leaving you with readable, sleek and streamlined unit tests.

First, we need to create our two test components — the first uses our directive and the second consumes the first.

<Embed src="https://gist.github.com/NetanelBasal/5f9e96a447ed7e98ae74036954654ea5.js" aspectRatio={0.357} caption="" />

Now, we can use Spectator to test the various ways the directive can be utilised.

<Embed src="https://gist.github.com/NetanelBasal/b4e70ad13f4c6376c151343605cc8ca4.js" aspectRatio={0.357} caption="" />

<Embed src="https://stackblitz.com/edit/angular-spectator-templateorstring?embed=1" aspectRatio={undefined} caption="" />

If you want to learn more about Spectator you can read about it in this article by [Inbal Sinai](https://medium.com/@inbalsinai):

[**Spectator for Angular or: How I Learned to Stop Worrying and Love the Spec**  
_We’ve all been there: You’re writing your code, chugging along, when suddenly the mere thought of the amount of unit…_engineering.datorama.com](https://engineering.datorama.com/spectator-for-angular-or-how-i-learned-to-stop-worrying-and-love-the-spec-2aa8521c8488 "https://engineering.datorama.com/spectator-for-angular-or-how-i-learned-to-stop-worrying-and-love-the-spec-2aa8521c8488")[](https://engineering.datorama.com/spectator-for-angular-or-how-i-learned-to-stop-worrying-and-love-the-spec-2aa8521c8488)

### 🔥 **Last but Not Least, Have you Heard of Akita?**

Akita is a state management pattern that we’ve developed here in Datorama. It’s been successfully used in a big data production environment, and we’re continually adding features to it.

Akita encourages simplicity. It saves you the hassle of creating boilerplate code and offers powerful tools with a moderate learning curve, suitable for both experienced and inexperienced developers alike.

I highly recommend checking it out.

[**🚀 Introducing Akita: A New State Management Pattern for Angular Applications**  
_Every developer knows state management is difficult. Continuously keeping track of what has been updated, why, and…_netbasal.com](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8 "https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8")[](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8)

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Akita and JS!_
