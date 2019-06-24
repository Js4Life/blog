---
title: "Querying a Normalized State with RxJS in Angular"
description: "First, we need to create a dedicated selector for every part of the state so that we can compose them later. The result and articles depend on each other ( if we add new article we need to update…"
date: "2017-05-27T21:05:41.099Z"
categories: 
  - JavaScript
  - Angular2
  - Rxjs
  - Web Development
  - Programming

published: true
canonical_link: https://netbasal.com/querying-a-normalized-state-with-rxjs-in-angular-71ecd7ca25b4
redirect_from:
  - /querying-a-normalized-state-with-rxjs-in-angular-71ecd7ca25b4
---

![](./asset-1.jpeg)

This post assumes that you at least have some working knowledge of ngrx/store and RxJS.

In this article, we are going to learn how we can query normalized state in our redux store.

Let’s say we have a blog and our normalized state look like this:

<Embed src="https://gist.github.com/NetanelBasal/6a8a09f57e17a7a9fcb3ff970073886c.js" aspectRatio={0.357} caption="" />

First let’s create our smart component.

<Embed src="https://gist.github.com/NetanelBasal/8c49f2b0d2411ce78b9ca9543e55ea69.js" aspectRatio={0.357} caption="" />

I’m going to abstract the `queries` to a dedicated service for three reasons:

1.  I do not like the idea of injecting the `Store` into my component. The component does not need to know how he gets the data. ( For the same reason that you do not inject the `HTTP` service. )
2.  I want the ability to reuse my `queries` in any other parts of my application.
3.  I want to keep my component clean.

Now let’s create the `ArticlesQuery` .

<Embed src="https://gist.github.com/NetanelBasal/9abf5e5e3d932c7732ec07beb98fd57a.js" aspectRatio={0.357} caption="" />

First, we need to create a dedicated selector for every part of the state so that we can compose them later.

Now let’s say we have a place in the app that needs only the articles. (`aside` area for example)

<Embed src="https://gist.github.com/NetanelBasal/361ee8e13e8e231466c2ee96862e482e.js" aspectRatio={0.357} caption="" />

The `result` and `articles` depend on each other ( if we add new article we need to update both the `result` and the `articles` ), so our best choice is to use the `[zip](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/zip.md)` operator.

> After all observables emit, emit values as an array

We can also use the second parameter — `resultSelector`. A function which takes the inputs at the specified index and combines them together.

`combineLatest` will also work but I prefer `zip` in this case.

**_update:_** `**_combineLatest_**` **_is a better option._**

<Embed src="https://gist.github.com/NetanelBasal/c1af9506865698ebf87710bb740df0a7.js" aspectRatio={0.357} caption="" />

Now let’s say we need the `articles` with their `comments`.

<Embed src="https://gist.github.com/NetanelBasal/36bd25166644848ea3b02900418519ba.js" aspectRatio={0.357} caption="" />

Do you see where I’m going with this? We can compose `queries` to get the necessary data.

Now let’s say we need the `articles` with their `comments` and the `authors`.

<Embed src="https://gist.github.com/NetanelBasal/8ba81e3a920b8e8eea8a2c2641be8425.js" aspectRatio={0.357} caption="" />

Now let’s add a `selectedArticle` key to our state.

<Embed src="https://gist.github.com/NetanelBasal/0539573f83d5f460b23402d6633cfc25.js" aspectRatio={0.357} caption="" />

If we want to display the selected article we can use the `[withLatestFrom](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/withlatestfrom.md)` operator.

<Embed src="https://gist.github.com/NetanelBasal/adfdca3f858ecddd0b74c8e04bcc1738.js" aspectRatio={0.357} caption="" />

> Also provide the last value from another observable

When the `selectedArticle` changes our observable emits new value with the latest `articles` and our component will re-render.

If we need only the `article` without any other data we can do this:

<Embed src="https://gist.github.com/NetanelBasal/35c1eaad4cf5851559309201a7ee873e.js" aspectRatio={0.357} caption="" />

#### Angular Router

The Router `params` are an observable so for example, if we store the selected id in the URL we can query the article like this:

<Embed src="https://gist.github.com/NetanelBasal/22d4889a454eedae6a42944ee37e9884.js" aspectRatio={0.357} caption="" />

#### Form Control

The `valueChanges` property is an observable, so for example if we need to implement search we can do this like this:

<Embed src="https://gist.github.com/NetanelBasal/fa4d8e51cd2884fa7ebe81ad879c7020.js" aspectRatio={0.357} caption="" />

Let’s say you need to get the favorite articles of a user, and this information is stored in a `user` reducer.

<Embed src="https://gist.github.com/NetanelBasal/274692beff63e071854733645ce284e3.js" aspectRatio={0.357} caption="" />

### You don’t need [reselect](https://github.com/reactjs/reselect)

The `select()` method will give you an observable that calls `[distinctUntilChanged](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/distinctuntilchanged.md)()` internally, meaning they will only fire when the state actually **changes**. ( it means when there is a new reference ).

### Summary

In the end, you have to remember that everything is a **stream**. You can use various observables/operators to compose data from your state or any other source.

`[combineLatest](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/combinelatest.md)` , `[withLatestFrom](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/withlatestfrom.md)` , `[zip](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/zip.md)`, `[concat](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/concat.md)` , `[forkJoin](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/forkjoin.md)` ,`[merge](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/merge.md)` and more…

### 🔥 **Last but Not Least, Have you Heard of Akita?**

Akita is a state management pattern that we’ve developed here in Datorama. It’s been successfully used in a big data production environment for over seven months, and we’re continually adding features to it.

Akita encourages simplicity. It saves you the hassle of creating boilerplate code and offers powerful tools with a moderate learning curve, suitable for both experienced and inexperienced developers alike.

I highly recommend checking it out.

[**🚀 Introducing Akita: A New State Management Pattern for Angular Applications**  
_Every developer knows state management is difficult. Continuously keeping track of what has been updated, why, and…_netbasal.com](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8 "https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8")[](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8)

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Vue and JS!_
