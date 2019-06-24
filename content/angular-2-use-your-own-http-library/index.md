---
title: "Angular 2 — Use Your own HTTP Library"
description: "Angular is flexible enough to let you use your desired HTTP library without worrying about change detection thanks to Zone js. In our root module, we are telling the injector, when you need the Http…"
date: "2016-11-28T15:41:09.184Z"
categories: 
  - JavaScript
  - Angularjs
  - Angular 2
  - Web Development

published: true
canonical_link: https://netbasal.com/angular-2-use-your-own-http-library-b45e51b3525e
redirect_from:
  - /angular-2-use-your-own-http-library-b45e51b3525e
---

![](./asset-1.png)

Angular is flexible enough to let you use your desired HTTP library without worrying about change detection thanks to Zone js.

So maybe for some reason:

1.  You don’t like the concept of observables.
2.  You need some abilities that the current API doesn’t support, and you want to add them without messing with the source code. ( for example HTTP interceptors ).
3.  You are just really really like other HTTP library.

You can use what ever you want.

My preferred library is [axios](https://github.com/mzabriskie/axios). Let’s see how easy is to integrate axios with Angular.

#### Install axios

```
npm install axios --save
```

#### Import and set the dependency injection:

<Embed src="https://gist.github.com/NetanelBasal/8efe80bc33c57a83f83205dd669a1f11.js" aspectRatio={0.357} caption="" />

In our root module, we are telling the injector, when you need the Http service return the axios object. ( library )

#### Use the new Http service:

<Embed src="https://gist.github.com/NetanelBasal/6d3ba0f18bd0c5f477fe6e46826cc239.js" aspectRatio={0.357} caption="" />

Our HTTP service is the **axios** library so we can use the axios API and thanks to Zone js our app continue to work as usual.

Let’s try to use the interceptors API:

<Embed src="https://gist.github.com/NetanelBasal/193db265aba2bef15709423aaac5ca17.js" aspectRatio={0.357} caption="" />

If you open the console, you will see the config object. Nice!

You can play with the code [here](https://plnkr.co/edit/zQGUIHBmPpLBHcVMVIEI?p=preview).

### Cons to this approach:

1\. You need to have your strategy to deal with testing because you are losing all the HTTP module API goodness.   
2\. You are using something that Angular doesn’t maintain.

That’s all!

_☞_ **_Please tap or click “︎_**❤” _to help to promote this piece to others._
