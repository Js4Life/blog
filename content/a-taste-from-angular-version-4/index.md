---
title: "A Taste from Angular Version 4"
description: "Let’s see two new features that are going to be in Angular version 4. ( unless something will change ) The ngIf directive now supports the else Input that takes a template reference to render when…"
date: "2017-01-17T06:40:44.327Z"
categories: 
  - Angularjs
  - JavaScript
  - Angular2
  - Web Development
  - Programming

published: true
canonical_link: https://netbasal.com/a-taste-from-angular-version-4-50be1c4f3550
redirect_from:
  - /a-taste-from-angular-version-4-50be1c4f3550
---

![](./asset-1.png)

Let’s see two new features that are going to be in Angular version 4. ( unless something will change )

### 💪 Super ngIf directive :

The `ngIf` directive now supports the `else` `Input` that takes a template reference to render when the condition is false.

<Embed src="https://gist.github.com/NetanelBasal/6f3b5faa3557a95b78f2a512f81e7fdb.js" aspectRatio={0.357} caption="" />

The second `Input` that the `ngIf` directive support is the `then` `Input`. This option becomes handy when you need to render a template when the condition is true or if you need to change the template reference at runtime.

<Embed src="https://gist.github.com/NetanelBasal/bc6e912667949e7ebe687e93d628ea86.js" aspectRatio={0.357} caption="" />

At first, it can be a little confusion, but you can read it like normal javascript:

```
if( condition ) {
  showThenTpl();
} else {
  showElseTpl();
}
```

The last add-on is that now you can store the result of the condition in a local variable.

For example:

<Embed src="https://gist.github.com/NetanelBasal/35182626c8f2189327eb3dd3d2486396.js" aspectRatio={0.357} caption="" />

When our `user` variable is null, Angular will render the `loading` template, and after 2 seconds Angular will render the `div` with the `ngIf` directive.

You also have access to the results of the condition locally in your template so it can repeatedly be bound in more efficient way.

There are several benefits to this approach:

1.  We are using the `async` pipe only once ( one subscription ).
2.  There is no need to use the safe-traversal-operator. ( the question mark )
3.  We can display an alternative template while waiting for the data.

Cool! 😎 You can play with the code [here](https://plnkr.co/edit/O1Oao7LBlBHKFgveFJPr?p=preview).

### 👏 NgComponentOutlet:

`NgComponentOutlet` is a new directive that provides a declarative approach for dynamic component creation.

If you already experienced with dynamic component creation in Angular version 2 you probably know that it takes some boilerplate and deeper understanding of things.

Let’s see how Angular version 4 make our life easier:

<Embed src="https://gist.github.com/NetanelBasal/9203ea55fb7a435ea7eda94b2dedb100.js" aspectRatio={0.357} caption="" />

We are using the `[ng-container](https://netbasal.com/getting-to-know-the-ng-container-directive-in-angular-a97b7a33c8ea#.809ic5qpc)` directive with the `NgComponentOutlet` that takes as `Input` a reference to a component. So easy!

Don’t forget to add your dynamic components to the `entryComponents` section of your module. You can read [here](https://angular.io/docs/ts/latest/cookbook/ngmodule-faq.html#!#q-when-entry-components) why you need to do this.

The `NgComponentOutlet` directive takes as `Inputs` additional advanced options (from the docs):

1.  `ngOutletInjector` — Optional custom Injector that will be used as a parent for the component. ( Defaults to the injector of the current view container )
2.  `ngOutletProviders` — Optional injectable objects that are visible to the  
    component.
3.  `ngOutletContent` — Optional list of projectable nodes to insert into the content. (ng-content)

<Embed src="https://gist.github.com/NetanelBasal/ad35ecb0a992e22c4a42c2c4397d0185.js" aspectRatio={0.357} caption="" />

You can play with the code [here](https://plnkr.co/edit/kwrjr2vIVb0Bfda4FUa0?p=preview).

### Last but not least -

1.  You don’t need to add the `novalidate` attribute to your forms by yourself anymore; Angular will add it for you by default.
2.  `NgTemplateOutlet` now compatible with \* syntax.

```
<ng-container *ngTemplateOutlet="templateRefExp; 
              context: contextExp">
</ng-container>
```![](./asset-2.png)

#### **_👉 Give me some ♥️ by clicking the heart below_**
