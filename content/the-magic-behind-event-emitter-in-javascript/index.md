---
title: "The Magic Behind Event Emitter in JavaScript"
description: "Understanding event emitter in javascript"
date: "2016-10-27T15:30:12.778Z"
categories: 
  - JavaScript
  - Nodejs
  - Web Development
  - Front End Development
  - Web Design

published: true
canonical_link: https://netbasal.com/javascript-the-magic-behind-event-emitter-cce3abcbcef9
redirect_from:
  - /javascript-the-magic-behind-event-emitter-cce3abcbcef9
---

![](./asset-1.jpeg)

### What is Event Emitter?

Event emitter as it sounds is just something that triggers an event to which anyone can listen.

Imagine the ability especially when working with asynchronous code to “scream” that some event happens in your code and let other parts of your system who cares about that to “hear” your screaming and register their intent.

There is a lot of different implementations for different purposes of the Event Emitter pattern, but the basic idea is to provide a framework for managing events and to be able subscribes to them.

Our goal here is to create our own Event Emitter to understand how the magic works. So let’s see how we can get the code below to work.

<Embed src="https://gist.github.com/NetanelBasal/8263d83d6a20e4c64dbf2bbd03f814b3.js" aspectRatio={0.357} caption="" />

Let’s start.

<Embed src="https://gist.github.com/NetanelBasal/6338ed621610902c4f93eee3a8ed6d36.js" aspectRatio={0.357} caption="" />

We start by creating the `EventEmitter` class and initialize the events property as an empty object. The purpose of the events property is to store our events name as the key and the value as an array of subscribers (i.e., functions).

### The Subscribe Function:

<Embed src="https://gist.github.com/NetanelBasal/0c9c7c2c30fdc8dfcbc2ffea3ffd0603.js" aspectRatio={0.357} caption="" />

The subscribe function takes the event name, in our example that will be `event:name-changed` and the function to call when someone emits ( or screaming ) the event.

One of the advantages of functions in Javascript is that functions are “first-class objects” so we can pass the function as a parameter to another function as we did in our subscribe method.

If no one already registers the event we need to do this at the first time by setting the key as the event name and initialize it with an empty array, then we push to this array the function that we want to execute when someone emits the event.

### The Emit Function:

<Embed src="https://gist.github.com/NetanelBasal/026e797ae3ba99c554c840cf16d4ebdb.js" aspectRatio={0.357} caption="" />

The emit function takes the event name that we want to “scream” and the data that we want to send with this event. If the event exists in our events map, we are looping over the functions that we register in the subscribe method and call them with the data.

That’s all it takes to make the code above work. But we still have one problem. We need a way to unregister those functions when we don’t need them anymore because if you don’t do this, you will have a **memory leak**.

Let’s solve this problem by returning an unsubscribe function from the subscribe method.

<Embed src="https://gist.github.com/NetanelBasal/5e063a2c499a4731d0562d584b78e2c6.js" aspectRatio={0.357} caption="" />

Because in Javascript functions are “first-class objects” you can return a function from a function. So now we can call the unsubscribe function like this:

<Embed src="https://gist.github.com/NetanelBasal/028863924457753652bdc33d3c5e35e6.js" aspectRatio={0.357} caption="" />

When we call the unsubscribe function, we are removing the function that we subscribed with the help of the filter method.

Bye Bye memory leak!

You can play with the code [here](https://plnkr.co/edit/TEM1eA9ahavEybuEKn6E?p=preview), That’s all.

### 💥 Last but Not Least, Have you Heard of Akita?

Akita is a state management pattern that we’ve developed here in Datorama. It’s been successfully used in a big data production environment, and we’re continually adding features to it.

Akita encourages simplicity. It saves you the hassle of creating boilerplate code and offers powerful tools with a moderate learning curve, suitable for both experienced and inexperienced developers alike.

I highly recommend checking it out.

[**🚀 Introducing Akita: A New State Management Pattern for Angular Applications**  
_Every developer knows state management is difficult. Continuously keeping track of what has been updated, why, and…_netbasal.com](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8 "https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8")[](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8)
