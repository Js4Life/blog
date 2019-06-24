---
title: "Show form errors on submit in Angular"
description: "If you are like me, you probably think that the best point to show to users the form errors is when the user submits the form. We are just exporting the ngForm directive to a local variable and uses…"
date: "2017-01-07T17:30:06.608Z"
categories: 
  - Angularjs
  - JavaScript
  - Angular2
  - Web Development
  - Programming

published: true
canonical_link: https://netbasal.com/show-form-errors-on-submit-in-angular-a6a10cd3e04b
redirect_from:
  - /show-form-errors-on-submit-in-angular-a6a10cd3e04b
---

![](./asset-1.png)

If you are like me, you probably think that the best point to show to users the form errors is when the user submits the form.

I think that is a bad user experience to do:

1.  Disable the submit button when the form is invalid — the user does not have a clue what’s wrong.
2.  Show the errors when the form is “touched” — this is very annoying, please don’t do that. Just let the user complete filing the form and then display the errors.

Let’s see how we can show the errors only after submit.

<Embed src="https://gist.github.com/NetanelBasal/8b33b142cfd6f43a015c786db1e2c368.js" aspectRatio={0.357} caption="" />

We are just exporting the `ngForm` directive to a local variable and uses it’s submitted property as an indication to know when the form is submitted, that’s all!

if you want to learn more about this syntax, you can read my [article](https://netbasal.com/angular-2-take-advantage-of-the-exportas-property-81374ce24d26#.f4oy7xam1) about the `exportAs` property.
