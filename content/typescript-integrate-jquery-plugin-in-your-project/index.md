---
title: "Typescript — Integrate jQuery Plugin in your Project"
description: "In this article, we will see how to integrate the “bootstrap-daterangepicker” jQuery plugin, what issues you will face and how to fix them. This error occurs because jQuery does not come with his own…"
date: "2016-12-07T05:42:03.425Z"
categories: 
  - JavaScript
  - Typescript
  - Angular2

published: true
canonical_link: https://medium.com/@NetanelBasal/typescript-integrate-jquery-plugin-in-your-project-e28c6887d8dc
redirect_from:
  - /typescript-integrate-jquery-plugin-in-your-project-e28c6887d8dc
---

_By: Netanel Basal (Angular Expert) and Yaron Biton, misterBIT.co.il CTO_

![](./asset-1.png)

When you are working on your app, sometimes you need to use a jQuery plugin.

In this article, we will see how to integrate the “bootstrap-daterangepicker” jQuery plugin, what issues you will face and how to fix them.

**Note**: I’m using Angular 2, but this is relevant to any typescript project or any jQuery plugin.

Let’s start by installing jQuery:

```
npm install jquery --save
```

Next, import the library:

```
import * as $ from ‘jquery’;
```

You are going to see now typescript error:

```
Cannot find module ‘jquery’.
```

This error occurs because jQuery does not come with his own definition file, so typescript doesn’t know what is jQuery.

#### **The solution:**

```
npm install @types/jquery --save
```

From TypeScript version 2.0, packages are under the [@types](http://twitter.com/types "Twitter profile for @types") organization.  
They are published directly from DefinitelyTyped, and the typescript compiler will find them automatically.

Great, now typescript knows what jQuery is, and the error is gone.

Next, let’s install the _bootstrap-daterangepicker_ plugin:

```
npm install bootstrap-daterangepicker --save
```

Let’s try to use this plugin in our directive:

```
import * as $ from ‘jquery’;

@Directive({
 selector: “[daterangepicker]”
})
class DaterangepickerDirective {
 constructor(private el: ElementRef) {}

 ngAfterViewInit() {
   $(this.el.nativeElement).daterangepicker();
 }
}
```

Your next error will be:

```
daterangepicker is not a function
```

#### The solution:

We need to import the library. In my case, I have a vendor file where I am importing all my third party libraries.

```
import 'bootstrap-daterangepicker';
```

Ok, the error is gone, but now we are getting this error:

```
Property ‘daterangepicker’ does not exist on type ‘JQuery’
```

Again, this error occurs because typescript doesn’t know what is _daterangepicker_.

But in this case, we have two problems:

1.  The daterangepicker doesn’t have definition file.
2.  jQuery plugins are defined on the prototype of the jQuery object.

#### The solution:

After digging into the source of the jQuery definition file, I’ve found out that jQuery has an interface called jQuery that defines all its prototype functions. For example:

```
interface JQuery {
   addClass(className: string): JQuery;
   attr(attributeName: string, value: string|number): JQuery;
}
```

Great, we now need to add the declaration of our jQuery plugin under interface with the same name.  
We don’t want to mess with the jQuery source code, so we need to create our own type definition file. I’m using angular-cli, so I will add the code in a file called **typings.d.ts**.

```
interface JQuery {
   daterangepicker(options?: any, callback?: Function) : any;
}
```

I’m declaring the daterangepicker function (this is the actual function that the API expose) and setting all its arguments as any ( You can go further and use specific types, but in this case, I will keep it simple ).

Now when the typescript compiler sees our interface, it will merge it with the jQuery interface and the error is gone.

That’s all!

_☞_ **_Please tap or click “︎_**❤” _to help to promote this piece to others._
