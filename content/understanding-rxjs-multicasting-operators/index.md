---
title: "Understanding RxJS Multicasting Operators"
description: "RxJS multicast operators, or their more known name, sharing operators are probably the most complicated topic to understand when get them…"
date: "2019-06-24T14:42:54.346Z"
categories: []
published: false
---

  

RxJS multicast operators, or their more known name, sharing operators are probably the most complicated topic to understand when get them. The key to really understand them, is to understand what’s the mechanism behind them and what’s the problem they solve.

  

  

### Unicast vs Multicast

In RxJS observables are cold, or unicast by default which means that each time we call `subscribe()` the subscription function invoked again. Let’s see an example:

When we subscribe to the `interval` observable we are invoking the `subscription` function, that executes the native JS `setInterval()` function and notifies the observer each time it’s invoked.

As a result, we are creating **two intervals** that are independent of each other.
