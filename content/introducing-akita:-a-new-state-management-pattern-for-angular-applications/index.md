---
title: "🚀 Introducing Akita: A New State Management Pattern for Angular Applications"
description: "Every developer knows state management is difficult. Continuously keeping track of what has been updated, why, and when, can become a nightmare, especially in large applications. In the Angular…"
date: "2018-06-12T08:19:47.046Z"
categories: 
  - JavaScript
  - Angular
  - Web Development
  - Angularjs
  - Rxjs

published: true
canonical_link: https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8
redirect_from:
  - /introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8
---

![Akita](./asset-1.jpeg)

Every developer knows state management is difficult_._ Continuously keeping track of what has been updated, why, and when, can become a nightmare, especially in large applications.

In the Angular world, the most popular solution is ngrx/store, which was inspired by the famous Redux model. Personally, I’m a big fan of Redux and worked a lot with ngrx; However, both as a developer and as a consultant, I’ve also come across the need for a different solution.

### 👶 Birth of [Akita](https://github.com/datorama/akita)

Here in [Datorama](https://datorama.com/), we experimented with several tools, including mobx and ngrx, and decided that they weren’t a good fit. That’s why I’ve decided to create different state management, one that suits our needs better. This can also serve as a solution for developers who don’t feel connected to the Redux concepts or find them hard to grasp.

Akita has been used by Datorama for half a year now, and we’ve decided that now, it is mature enough to be published as open source. Almost all our code base has been migrated to Akita from both mobx and ngrx. The team is satisfied, having found Akita both simple and effective, and we’ve seen improvements both in delivery time and in performance.

It’s worth mentioning that Datorama is an enterprise company with complicated state management, a lot of complex, interconnecting entities, filters, etc.

### **🤔 What is **[**Akita**](https://netbasal.gitbook.io/akita/)**?**

[Akita](https://netbasal.gitbook.io/akita/) is a state management pattern, built on top of RxJS, which takes the idea of multiple data stores from Flux and the immutable updates from Redux, along with the concept of streaming data, to create the **Observable Data Stores model**.

Akita encourages simplicity. It saves you the hassle of creating boilerplate code and offers powerful tools with a moderate learning curve, suitable for both experienced and inexperienced developers alike.

Akita is based on object-oriented design principles instead of functional programming, so developers with OOP experience should feel right at home. Its opinionated structure provides your team with a fixed pattern that cannot be deviated from.

![Akita Architecture](./asset-2.png)

### 👁 High-level Principles

1\. The Store is a single object which contains the store state and serves as the “single source of truth.”

2\. The only way to change the state is by calling `setState()` or one of the update methods based on it.

3\. A component should NOT get the data from the store directly but instead, use a Query.

4\. Asynchronous logic and update calls should be encapsulated in services and data services.

Let’s start by learning Akita’s core concepts. Although Akita supports both a regular store and an entities store, we’ll focus on the entities store feature because, for the most part, it will be the one you’ll require in your applications.

### 🤓 Core Concepts

#### The Model

The model is a representation of an entity. Let’s take for example a todo:

<Embed src="https://gist.github.com/NetanelBasal/947ca2bce62de5a4383533c0dbd12d3f.js" aspectRatio={0.357} caption="todo.model" />

Akita’s recommended creating both a type and a factory function in charge of building the entity.

#### Entity Store

The Store is a single object which contains the store state and serves as the “single source of truth.”

You can think of an entity store as a table in a database, where each table represents a flat collection of entities.

Akita’s `EntityStore` simplifies the process, giving you everything you need to manage it.

Let’s see how we can use it to create a `todos` table, i.e., an `EntityStore` managing a `Todo` object:

<Embed src="https://gist.github.com/NetanelBasal/7e0b7e9c3d73702abaa84c197670df94.js" aspectRatio={0.357} caption="todos.store.ts" />

First, we need to define the store’s interface. In our case, we can make do with extending the `EntityState` from Akita, providing it with the `Todo` Entity type.

If you are curious, `EntityState` has the following signature:

<Embed src="https://gist.github.com/NetanelBasal/17324511578c752dcb0c68ee74118298.js" aspectRatio={0.357} caption="entity-state.ts" />

By using this model, you’ll receive from Akita a lot of built-in functionality, such as crud operations on entities, error management, etc.

For example, here’s how we can add a new todo:

<Embed src="https://gist.github.com/NetanelBasal/0001d117ccf08715473ab6ef8fdae1cf.js" aspectRatio={0.357} caption="todos-page.component.ts" />

Or an example of how we can update a todo:

<Embed src="https://gist.github.com/NetanelBasal/a32701ea385e038d8e54a0c99d85326a.js" aspectRatio={0.357} caption="todos-page.component.ts" />

For the full API check out the [docs](https://netbasal.gitbook.io/akita/entity-store/entity-store).

#### Entity Query

A Query is a class offering functionality responsible for querying the store.

You can think of the query as being similar to database queries. Its constructor function receives as parameters its own store and possibly other query classes.

**Queries can talk to other queries, join entities from different stores, etc.**

Let’s see how we can use it to create a `todos` query:

<Embed src="https://gist.github.com/NetanelBasal/82c358df7cd2b568099baf6c6d7c8909.js" aspectRatio={0.357} caption="" />

Here, you’ll receive from Akita a lot of built-in functionality, including methods such `selectAll()`, `selectEntity()`, `selectMany()`, etc.

For example, here’s how we can _reactively_ query the list of todos:

<Embed src="https://gist.github.com/NetanelBasal/502dea8989b6d2543ec97871a4c033a7.js" aspectRatio={0.357} caption="todos-page.component.ts" />

Or an example of how we can query the first todo’s complete status:

<Embed src="https://gist.github.com/NetanelBasal/ec49eb57d440d40da70f2a751a75256c.js" aspectRatio={0.357} caption="query-entity.component.ts" />

Again, for the full API check out the [docs](https://netbasal.gitbook.io/akita/entity-store/entity-query/api).

Here is a complete working todos application:

<Embed src="https://stackblitz.com/edit/akita-todos-app?embed=1" aspectRatio={undefined} caption="" />

The [next](https://engineering.datorama.com/building-a-shopping-cart-in-angular-using-akita-c41f6a6f7255) post will feature a shopping cart example using Akita, which will demonstrate Akita’s real-world functionality.
