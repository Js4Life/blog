---
title: "Angular 2 — Explore The Renderer Service"
description: "The Renderer class is a built-in service that provides an abstraction for UI rendering manipulations. For example, you need to set the focus on an input element, so you may be tempted to do something…"
date: "2016-10-26T10:06:06.729Z"
categories: 
  - JavaScript
  - Angular2
  - Web Development
  - Angularjs
  - Front End Development

published: true
canonical_link: https://netbasal.com/angular-2-explore-the-renderer-service-e43ef673b26c
redirect_from:
  - /angular-2-explore-the-renderer-service-e43ef673b26c
---

_By: Netanel Basal (Angular Expert) and Yaron Biton, misterBIT.co.il CTO_

![](./asset-1.jpeg)

The Renderer class is a built-in service that provides an abstraction for UI rendering manipulations.

For example, you need to set the focus on an input element, so you may be tempted to do something like:

<Embed src="https://gist.github.com/NetanelBasal/830b03602328b7508b72c5911a841ded.js" aspectRatio={0.357} caption="" />

This works fine. We are grabbing the input element with the help of the ViewChild decorator and then access the native DOM element and call the _focus()_ method on the input.

The problem with this approach is that when we access the native element directly we are giving up on Angular’s DOM abstraction and miss out on the opportunity to be able to execute also in none-DOM environments such as: native mobile, native desktop, web worker or server side rendering.

Remember that Angular is a platform, and the browser is just one option for where we can render our app.

So what you do is to give this **responsibility** to the Renderer class.

First let’s create the ExploreRendererDirective.

<Embed src="https://gist.github.com/NetanelBasal/37078dde544e7221118b41815cb8c105.js" aspectRatio={0.357} caption="" />

The code is straightforward; we are injecting the Renderer service and the ElementRef service to get access to the host element.

```
<div exploreRenderer></div>
```

In this case the host element is the **div**.

Next, Let’s explore the Renderer class methods. ( all the methods are self explanatory so I won’t go over and explain them )

-   **Renderer.createElement( parentElement: any, name: string )**

```
let inputElement = this.renderer.createElement(this.nativeElement, “input”);
```

-   **Renderer.setElementAttribute( renderElement: any, attributeName: string, attributeValue: string)**

```
this.renderer.setElementAttribute(inputElement, “value”, “Hello from renderer”);
```

-   **Renderer.invokeElementMethod( renderElement: any, methodName: string, args?: \[\] )**

```
this.renderer.invokeElementMethod(inputElement, “focus”, []);
```

-   **Renderer.createText( parentElement: any, value: string )**

```
let buttonElement = this.renderer.createElement(this.nativeElement, “button”);
this.renderer.createText(buttonElement, “Click me!”);
```

-   **Renderer.setElementProperty( renderElement: any, propertyName: string, propertyValue: string )**

```
this.renderer.setElementProperty(buttonElement, “disabled”, true);
```

-   **Renderer.listen( parentElement: any, name: string, callback: Function )**

```
this.renderer.listen(buttonElement, “click”, ( event ) => console.log(event));
```

-   **Renderer.setElementClass( renderElement: any, className: string, isAdd: boolean )**

```
this.renderer.setElementClass(buttonElement, "btn-large", true);
```

-   **Renderer.setElementStyle( renderElement: any, styleName: string, styleValue: string )**

```
this.renderer.setElementStyle(buttonElement, “backgroundColor”, “yellow”);
```

-   **Renderer.listenGlobal (target: string, name: string, callback: Function )**

```
// The target could be one of three: window, document, body
this.renderer.listenGlobal("body", "click", () => console.log("Global event"));
```

-   **Renderer.selectRootElement(selectorOrNode: string|any)**

```
// This is equivalent to document.querySelector("input")
let inputElement = this.renderer.selectRootElement("input");
```

-   **Renderer.projectNodes( parentElement: any, nodes: any\[\] )**

```
const pEleOne = this.renderer.createElement(this.nativeElement, "p");
const pEleTwo = this.renderer.createElement(this.nativeElement, "p");
this.renderer.createText(pEleOne, "Element one");
this.renderer.createText(pEleTwo, "Element two");
this.renderer.projectNodes(this.nativeElement, [pEleOne, pEleTwo]);
```

-   **Renderer.attachViewAfter( node:any, viewRootNodes: any\[ \])**

```
this.renderer.attachViewAfter(inputElement, [pEleOne, pEleTwo]);
```

-   **Renderer.detachView( viewRootNodes: any\[\] )**

```
this.renderer.detachView([pEleTwo]);
```

-   **Renderer.animate(element: any, startingStyles: AnimationStyles, keyframes: AnimationKeyframe\[\], duration: number, delay: number, easing: string) : AnimationPlayer**

```
const startingStyles : AnimationStyles = {
  styles: [{}]
}

const keyframes : AnimationKeyframe[] = [
  {
    offset: 0,
    styles: {
      styles: [{
        transform: 'translateX(0px)',
      }]
    }
  },
  {
    offset: 1,
    styles: {
      styles: [{
        transform: 'translateX(100px)',
      }]
    }
  }]

const animation: AnimationPlayer = this.renderer.animate(buttonElement, startingStyles, keyframes, 3000, 1000, "ease");
animation.play();
animation.onDone(() => console.log('Animation complete'));
```

**TIP**: You can also use the Renderer service to bypass Angular’s templating and make custom UI changes that cannot be expressed declaratively. For example — you need to set a property or an attribute whose name is not statically known.

That’s all.

You can play with the code [here](https://plnkr.co/edit/8hhUkYQsJTudNuM6wzob?p=preview).

_☞_ **_Please tap or click “︎_**❤” _to help to promote this piece to others._
