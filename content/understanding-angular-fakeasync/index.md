---
title: "Understanding Angular fakeAsync"
description: "As the name suggests fakeAsync gives you the ability to test asynchronous code in a synchronous way. Under the hood, it uses a dedicated…"
date: "2019-06-24T14:47:06.632Z"
categories: []
published: false
---

  

As the name suggests fakeAsync gives you the ability to test asynchronous code in a synchronous way. Under the hood, it uses a dedicated test zone that intercepts the asynchronous browser APIs like setTimeout, setInterval, etc., and [monkey patches](https://www.audero.it/blog/2016/12/05/monkey-patching-javascript/) them.

What it means, that it doesn’t invoke the real method implementation, it basically save the method internally and exposes an API let you decide when to invoke them.

### Testing Timers

fakeAsync supports the setImmediate, setInterval and setTimeout APIs. Let’s see two examples of how we can test a component that uses these APIs.

#### setTimeout

Excuse me for not giving real-world examples. The goal is to focus on `fakeAsync` and not to blow the template with a boilerplate. Ok, so back to our example, we can see that we wrap the assertion method with the fakeAsync function so that any code inside this function will run in a “fake zone”. 

The next thing we use is the tick() method. Because we work with timers, we need a way to control time. The tick() method that can be used only inside fakeAsync zone gives us the power to simulates the asynchronous passage of time.

The tick() method takes as a parameter the number of milliseconds we want to run forward. In our example, we have a setTimout of 100 milliseconds, so we need to call tick(100) to invoke the setTimeout callback. After the callback runs, we need to call detectChanges and make our assertion.

#### setInterval

When we work with setInterval the process is the same. We need to call the tick() method passing the number of milliseconds we want to skip. Pay attention that you can’t test infinite setInterval (from the obvious reasons)

the calls to tick() are cumulative within a test.
