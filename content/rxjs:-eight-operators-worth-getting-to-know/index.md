---
title: "RxJS: Eight Operators Worth Getting to Know"
description: "Splits the source observable into two, one with values that satisfy a predicate, and another with values that don’t satisfy the predicate. Akita is a state management pattern that we’ve developed…"
date: "2018-07-03T11:55:03.926Z"
categories: 
  - JavaScript
  - Rxjs
  - Web Development
  - Angular

published: true
canonical_link: https://netbasal.com/rxjs-eight-operators-worth-getting-to-know-2b6c18e601d
redirect_from:
  - /rxjs-eight-operators-worth-getting-to-know-2b6c18e601d
---

![](./asset-1.jpeg)

### \# mapTo

Ignores the source value and emit the provided constant when the source emits.

<Embed src="https://gist.github.com/NetanelBasal/11eae3ce110efffeacdd229d62605f1b.js" aspectRatio={0.357} caption="" />

### \# finalize

Call the provided function on error or complete.

<Embed src="https://gist.github.com/NetanelBasal/742b69fef77fb8b6df8fa506b00afed7.js" aspectRatio={0.357} caption="" />

### \# switchMapTo

Like `switchMap`, for when you don’t need to use the source value.

<Embed src="https://gist.github.com/NetanelBasal/1eeef6fceb168f2bd4770c98ccd2c16b.js" aspectRatio={0.357} caption="" />

There are also variations for `mergeMapTo` and `concatMapTo` .

### \# pluck

Maps each source value (an object) to its specified nested property.

<Embed src="https://gist.github.com/NetanelBasal/ad06f1198f386a4abc165d735d8060d4.js" aspectRatio={0.357} caption="" />

### \# startWith

Emit given value(s) first.

<Embed src="https://gist.github.com/NetanelBasal/3bd6f78ad38bcec4555f13801237f040.js" aspectRatio={0.357} caption="" />

### \# toArray

Creates an array from observable sequence. (upon completion)

<Embed src="https://gist.github.com/NetanelBasal/36d423dd5901ddf97d676ec2e3e278b5.js" aspectRatio={0.357} caption="" />

### \# partition

Splits the source observable into two, one with values that satisfy a predicate, and another with values that don’t satisfy the predicate.

<Embed src="https://gist.github.com/NetanelBasal/4e3b64ae43e5d28519faeecd41f9bea3.js" aspectRatio={0.357} caption="" />

### \# empty

Just emits `complete`, and nothing else.

<Embed src="https://gist.github.com/NetanelBasal/34001e9d49c9adb4fbbb8cda9f2477de.js" aspectRatio={0.357} caption="" />

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Vue and JS!_

### 👂🏻 Last but Not Least, Have you Heard of Akita?

Akita is a state management pattern that we’ve developed here in Datorama. It’s been successfully used in a big data production environment for over seven months, and we’re continually adding features to it.

Akita encourages simplicity. It saves you the hassle of creating boilerplate code and offers powerful tools with a moderate learning curve, suitable for both experienced and inexperienced developers alike.

I highly recommend checking it out.

[**🚀 Introducing Akita: A New State Management Pattern for Angular Applications**  
_Every developer knows state management is difficult. Continuously keeping track of what has been updated, why, and…_netbasal.com](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8 "https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8")[](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8)
