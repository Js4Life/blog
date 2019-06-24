---
title: "JavaScript — Observables Under The Hood"
description: "I like to think of Observable as a function that “throws” values. It can “throw” values in synchrony or asynchrony way. If you have interest on this values, you can register observer. When the…"
date: "2016-11-15T10:14:06.262Z"
categories: 
  - JavaScript
  - Rxjs
  - Angular2
  - Web Development
  - Nodejs

published: true
canonical_link: https://netbasal.com/javascript-observables-under-the-hood-2423f760584
redirect_from:
  - /javascript-observables-under-the-hood-2423f760584
---

![](./asset-1.jpeg)

#### The Observer pattern:

> The observer pattern is a software design pattern in which an object, called the subject, maintains a list of its dependents, called observers, and notifies them automatically of any state changes, usually by calling one of their methods. (Wikipedia)

#### The Observable:

I like to think of Observable as a function that “throws” values. It can “throw” values in synchrony or asynchrony way. If you have interest on this values, you can register observer.

#### The Observer:

The Observer is an object, with three functions.

1.  next() => Observable, please call this function when you have a new value for me.
2.  error() => Observable, please call this function when you have a new error for me.
3.  complete() => Observable, please call this function when you complete your job.

When the Observable ( i.e., function ) “throws” new value, error or completes, he will call the corresponding function on your observer.

#### Push vs. Pull:

If you are familiar with the _Iterator_ pattern, you know that in this case, you are in charge. When you want new value, you just call the next method to **pull** the value.

```
var it = makeIterator(['yo', 'ya']);
console.log(it.next().value); // 'yo'
```

> With Observable it’s like, don’t call us we call you.

The Observable is the boss. When he has a new value, he will **push** the value to you. Your job is just to “listen.”

#### Real World Metaphor — NewsLetters:

How the newsletter appears in your email? You are **subscribing** to the newsletter; when there is a new newsletter the manager just **pushes** it to your email.

Enough with the speeches, let’s start to get’s our hands dirty and understand how Observable works by creating our simple mini Rx.

We need to get this code to work:

<Embed src="https://gist.github.com/NetanelBasal/e983f0f2cc8b04bfe7506d2d619ebe25.js" aspectRatio={0.357} caption="" />

Let’s start.

<Embed src="https://gist.github.com/NetanelBasal/25aacbfb388d027ce85c566e688774ba.js" aspectRatio={0.357} caption="" />

We start by setting up the Observable class and save a reference to the function that will “throws” values. Now you can understand why Observables are **lazy**; we don’t invoke the function yet just saving a reference.

#### The subscribe function:

<Embed src="https://gist.github.com/NetanelBasal/f9c6a88edf246ab09e94b9932f3096d5.js" aspectRatio={0.357} caption="" />

Only when you call the subscribe method, you are invoking the function that will “throw” values with the observer.

Congratulations! You created your new Observable.

Let’s see how we can create the map method.

```
fakeAsyncData$.map(val => `New value ${val}`).subscribe({
   next(val) { console.log(val) } ,
   error(e) { console.log(e) } ,
   complete() { console.log(‘complete’) } 
});
```

<Embed src="https://gist.github.com/NetanelBasal/26853655929417fa779643b4a0cade56.js" aspectRatio={0.357} caption="" />

When we call map what is happening is the map method returns new Observable that subscribes to the source, in our case the fakeAsyncData$. When the source “throws” new value first it gets to the map method, and after applying the projection function on the value, the map Observable “throws” the value on us. ( remember we are subscribed to the map Observable )

Let’s see how easy is now to create the fromEvent method.

```
var button = document.getElementById(‘button’);

let clicks$ = Observable.fromEvent(button, 'click')
              .map(e => `${e.pageX}px`);

let unsubscribe = clicks$.subscribe({
  next(val) { console.log(val) } ,
  error(e) { console.log(e) } ,
  complete() { console.log('complete') } 
});
```

<Embed src="https://gist.github.com/NetanelBasal/1abb6172d856ca47f2b0bd0601eca539.js" aspectRatio={0.357} caption="" />

The fromEvent method just returns new Observable that will “throws” us the event object when the event happens. We don’t want memory leaks, so we return a function that will give us the ability to unsubscribe when we need.

```
setTimeout(() => unsubscribe(), 1000);
```

Let’s implement the fromArray method:

```
let array$ = Observable.fromArray([1,2,3]);

array$.subscribe({
 next(val) { console.log(val) } ,
 error(e) { console.log(e) } ,
 complete() { console.log(‘complete’) } 
});
```

<Embed src="https://gist.github.com/NetanelBasal/6c9270d880593fd929baa0e6691b1a8b.js" aspectRatio={0.357} caption="" />

We can see that Observables can be synchronous too.

#### Understanding mergeMap:

Let’s say we need to do this:

```
let promise = val => {
  return new Promise(resolve => {
    setTimeout(() => resolve(val), 3000);
 });
}

let data$ = Observable.fromArray([1,2,3]).map(val =>  Observable.fromPromise(promise(val)));
```

After this code runs we will get Observable, because the map function returns Observable. What we need is a way to merge this Observable into the stream. That’s why it’s called mergeMap; we are performing both a map operation and a merge operation at once.

Let’s create simple mergeMap method:

<Embed src="https://gist.github.com/NetanelBasal/b962ec16a9108cd424d0be062d4cc251.js" aspectRatio={0.357} caption="" />

I’m sure you can now understand alone what we are doing here. Now you merged the Observable into the stream.

```
let data$ = Observable.fromArray([1,2,3]).
            mergeMap(val => Observable.fromPromise(promise(val)));
```

#### Last tip:

If you only need to use the next function or you doesn’t like the object way you can do this:

<Embed src="https://gist.github.com/NetanelBasal/158aa7b0a3c989ca2ed0b5b0fa34260c.js" aspectRatio={0.357} caption="" />

Now you can write the code like this:

```
clicks$.subscribe(val => console.log(val));
```

#### The final code:

<Embed src="https://gist.github.com/NetanelBasal/9f579cfbef56f1fc0e47d12f0c00ad31.js" aspectRatio={0.357} caption="" />

_☞_ **_Please tap or click “︎_**❤” _to help to promote this piece to others._
