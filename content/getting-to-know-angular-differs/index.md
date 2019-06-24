---
title: "📢 Getting to Know Angular Differs"
description: "Angular differs are probably the least known APIs; These are highly optimized building blocks used by Angular itself throughout the framework (ngClass, ngStyle, ngFor, etc.). You certainly won’t use…"
date: "2018-05-24T14:46:45.561Z"
categories: 
  - JavaScript
  - Web Development
  - Angular
  - Angularjs

published: true
canonical_link: https://netbasal.com/getting-to-know-angular-differs-60cd68f4bd8f
redirect_from:
  - /getting-to-know-angular-differs-60cd68f4bd8f
---

![](./asset-1.jpeg)

Angular differs are probably the least known APIs; These are highly optimized building blocks used by Angular itself throughout the framework (ngClass, ngStyle, ngFor, etc.).

You certainly won’t use them on a daily basis, however, given the right circumstances, they can prove to be very useful. If you ever get to the point where you’re saying to yourself:

> It’s not enough for me to know that something has changed, I want to know what has changed.

Then you should know that Angular differs is the answer.

Angular offers two types of differs APIs, one is the `IterableDiffer` and the second is the `KeyValueDiffers`.

### The IterableDiffer API

`IterableDiffer` is an API that tracks changes made to an iterable over time and also exposes an API enabling you to react to these changes.

Let’s learn how we can use it by creating our own `ngClass` directive.

<Embed src="https://gist.github.com/NetanelBasal/b70391c1180a9ec9ceb2295f3e4ac11d.js" aspectRatio={0.357} caption="" />

The `find()` method checks whether Angular has a differ that supports the type of the given parameter. We can examine this process in the source code:

<Embed src="https://gist.github.com/NetanelBasal/1ccd188c1f537e37c44e77c7491dbb0d.js" aspectRatio={0.357} caption="" />

Once found, the `create()` method returns an instance of this differ, in this case `DefaultKeyValueDiffer` .

<Embed src="https://gist.github.com/NetanelBasal/0d719491bf9bef4a529dec15380c84bb.js" aspectRatio={0.357} caption="" />

Great, we have a differ. Now let’s see how we can use it. Our goal is to listen for any change in the `ngMyClass` input, and based on that adding or removing the class.

<Embed src="https://gist.github.com/NetanelBasal/a69e6f4c6cb1051e2e13da2e06c885d8.js" aspectRatio={0.357} caption="" />

We can check for changes by calling the `diff()` method with the new value.

When there aren’t any changes the return value will be null, otherwise the return value will be an object that exposes self-explanatory methods that we can use to react to these changes.

In our case, we want to know whether an item was added or removed so we can act accordingly.

<Embed src="https://stackblitz.com/edit/iterable-differs?embed=1" aspectRatio={undefined} caption="Live Demo" />

### The KeyValueDiffers API

The `KeyValueDiffers` api works in a similar matter, except it tracks the changes in an object rather than an iterable.

The same concepts mentioned in the first section also apply here. Let’s change the directive we created to get an object instead of an array:

<Embed src="https://gist.github.com/NetanelBasal/03a5bdd50414684cc8458d6df98905ac.js" aspectRatio={0.357} caption="" />

When using `KeyValueDiffers` the `find()` and `create()` method implementations are the following:

<Embed src="https://gist.github.com/NetanelBasal/82b11fe428eee6d3e3c68b86da90fcbf.js" aspectRatio={0.357} caption="" />

Now that we have a differ, let’s use it:

<Embed src="https://gist.github.com/NetanelBasal/a98a57a66811253fe6e0cce399a112db.js" aspectRatio={0.357} caption="" />

Same as before. When we don’t get null, we have handy self-explanatory methods that give us all what we need.

<Embed src="https://stackblitz.com/edit/keyvaluediffer?embed=1" aspectRatio={undefined} caption="" />

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Vue and JS!_

**Things to not miss**:

[**datorama/akita**  
_akita — 🚀 Simple and Effective State Management for Angular Applications_github.com](https://github.com/datorama/akita "https://github.com/datorama/akita")[](https://github.com/datorama/akita)

[**NetanelBasal/spectator**  
_spectator — 👻 Angular Tests Made Easy 🤓_github.com](https://github.com/NetanelBasal/spectator "https://github.com/NetanelBasal/spectator")[](https://github.com/NetanelBasal/spectator)
