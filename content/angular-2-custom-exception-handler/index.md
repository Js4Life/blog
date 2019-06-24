---
title: "Angular 2 — Custom Exception Handler"
description: "When we are developing our app, we are in control, and we can see the errors and handle them appropriately. But this is not the case in production, even if you have tested your app. \nIn production…"
date: "2016-11-08T13:07:13.475Z"
categories: 
  - Angular2
  - Angularjs
  - JavaScript
  - Programming
  - Front End Development

published: true
canonical_link: https://netbasal.com/angular-2-custom-exception-handler-1bcbc45c3230
redirect_from:
  - /angular-2-custom-exception-handler-1bcbc45c3230
---

![](./asset-1.jpeg)

When we are developing our app, we are in control, and we can see the errors and handle them appropriately.

But this is not the case in production, even if you have tested your app.   
In production, you can’t see what your user is seeing and experiencing and you not capable of knowing about errors unless you find a way to be notified about the exceptions.

Angular 2 has his own way to catch and handle errors. The default implementation of **ErrorHandler** prints errors messages to the console. You can intercept the default error handling and write a custom exception handler that replaces this default as appropriate for your app.

In our case, in production, we want to send the errors to our server. Let’s see how we can do it.

We need to create our own ErrorHandler class that implements the default ErrorHandler. The only method that required is the **handleError** method that takes the error as an argument.

<Embed src="https://gist.github.com/NetanelBasal/a9bc8e4c4370d81678f6a464d81fbe81.js" aspectRatio={0.357} caption="" />

Next, we need to tell our injector to use our implementation instead of the default.

<Embed src="https://gist.github.com/NetanelBasal/73c440efcb3381e63a1befc967e5b078.js" aspectRatio={0.357} caption="" />

That’s all. If you don’t want to ignore the Angular default handler, you can extend the **ErrorHandler** class and also call the default handler like this:

<Embed src="https://gist.github.com/NetanelBasal/81144976e19fa0b52aca9c85f777f072.js" aspectRatio={0.357} caption="" />

You can play with the code [here](https://plnkr.co/edit/AWfOb8phC4XGgsPbpsIv?p=preview).

_☞_ **_Please tap or click “︎_**❤” _to help to promote this piece to others._
