---
title: "👷 Building a Shopping Cart in Angular Using Akita"
description: "This is the second part of the introduction to Akita (the first part can be found here). In this part, we’ll create from scratch a working shopping cart by following the principles we learned in the…"
date: "2018-06-12T08:20:31.670Z"
categories: 
  - JavaScript
  - Web Development
  - Angular
  - Angularjs
  - Rxjs

published: true
canonical_link: https://engineering.datorama.com/building-a-shopping-cart-in-angular-using-akita-c41f6a6f7255
redirect_from:
  - /building-a-shopping-cart-in-angular-using-akita-c41f6a6f7255
---

![Shopping Cart](./asset-1.jpeg)

This is the second part of the introduction to [Akita](https://github.com/datorama/akita) (the [first](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8) part can be found here). In this part, we’ll create from scratch a working shopping cart by following the principles we learned in the previous article in order to connect the dots.

![Live Demo](./asset-2.gif)

### The Products

First, we need to display a list of products that we’ll get from the backend API:

<Embed src="https://gist.github.com/NetanelBasal/e8d10d06d305eee6fff3fba5395ca33e.js" aspectRatio={0.357} caption="" />

You already should be familiar with the above code. In Akita, everything starts with defining the entity type, a store, and a query.

Note that in this case, we aren’t creating a factory function, because the state is read-only, meaning we don’t need to create new products and we don’t need to modify the object coming from the server.

Akita recommends that asynchronous logic and update calls should be encapsulated in services, so let’s continue by creating a service which is responsible for fetching the products from the server:

<Embed src="https://gist.github.com/NetanelBasal/096e6afeac296e20033abaeaae50f044.js" aspectRatio={0.357} caption="" />

Let’s stop for a second to explain what’s going on in the service.

When using entities store, it’s initial state is `pristine`, and when you call `store.set()`, Akita changes it to false. Calling `remove()` sets it to true again.

This can be used to determine whether the data is present in the store, to save on additional server requests.

Returning to our example, we only initialize a server request if the store’s state is pristine, otherwise returning an observable that `next()` once and complete.

Now, let’s move on to the components.

Akita encourages the model of smart and dumb (aka stateless and stateful ) components’ architecture. This combination allows us to use the `async` pipe, combined with the `OnPush` change strategy to gain better performance.

### Presentational Components

Presentational components describe **how things look**. Typically, they will receive data via inputs() and will communicate via outputs(), aka events.

Let’s create a product component in charge of the presentation of a product:

<Embed src="https://gist.github.com/NetanelBasal/bb8e95dce56ff7f4089a2f537bb012f0.js" aspectRatio={0.357} caption="product.component.ts" />

The product component receives the product as input() and communicates with its parent via outputs.

### Container Components

Container components are concerned with **how things work**. They provide the data and behavior to presentational or other container components.

Let’s create a products component in charge of displaying a list of products filtered by a search term:

<Embed src="https://gist.github.com/NetanelBasal/c768018f087b0e95f00a6ac8f809ada1.js" aspectRatio={0.357} caption="" />

We start by calling the service we created earlier, to fetch the products from the server and update the store.

The `selectLoading()` is a query method from Akita that provides the value of the `loading` key from the store reactively.

The initial value of the `loading` state is set to `true` and is switched to `false` when you call `store.set()`(you can always change it by using the API).

We want to filter the products based on the search value, so we’re listening to the control input, leveraging Akita’s filterBy feature to return products matching the search term.

**Tip:** We recommend placing the logic for underlying queries inside the query class so it can be more readable and reusable:

<Embed src="https://gist.github.com/NetanelBasal/ae6a370546f21483c7a53da06293eaf6.js" aspectRatio={0.357} caption="cart.query.ts" />

That’s it for the products page, let’s move on to create the cart.

### The Cart

As always, we start by creating Akita’s building blocks:

<Embed src="https://gist.github.com/NetanelBasal/ef8927624ee90d5d3f1f9353978fca88.js" aspectRatio={0.357} caption="cart-akita" />

The observant among you may have noticed two new code samples.

First, we have a factory function that knows how to create new cart items.

Second, there’s the `idKey` attribute in the `CartStore` constructor. By default, Akita takes the id key from the entity `id` field, but in our case, we’re telling Akita — the id key is `productId`.

#### Add a Product to the Cart

When a user clicks the add button, we want to add a new cart item to the store. Here is how we can do this:

<Embed src="https://gist.github.com/NetanelBasal/99a4c5dcf9fcf5a87ecef78ae8751463.js" aspectRatio={0.357} caption="cart.service.ts" />

Using the product id, we check that the product doesn’t exist, and add it; Otherwise, we update the quantity.

Here is the code for updating a cart item:

<Embed src="https://gist.github.com/NetanelBasal/4fd78629eb6dc156706e65eaab26b6ee.js" aspectRatio={0.357} caption="cart.query.ts" />

We are using the `update()` method from Akita, passing the `productId` and a callback function that returns a new immutable updated item.

For brevity, I will skip the parts dealing with subtracting/removing an item from the cart. Let’s move on to the exciting part, the cart list.

#### Displaying the Cart Items

We need to show the list of cart items and the total amount, but we also need some information from the product, like the title and the price. Therefore we need to **join** the cartStore with the productsStore.

In Akita, Queries can talk to other queries, join entities from different stores, etc. Here is how we join them:

<Embed src="https://gist.github.com/NetanelBasal/41e3739a3da5e273ef66e0aa17a23920.js" aspectRatio={0.357} caption="" />

We’re using the `combineLatest()` observable to get both the list of cart items and the products. Then we are mapping over them, merging a cart item with the corresponding product based on the `productId`.

We also want to calculate the cart total without executing the mapping function again, so we’re leveraging one of the `share()` operators from Rx.

Now, we can display the cart items:

<Embed src="https://gist.github.com/NetanelBasal/f8695de229b099e3f280b0089bfcb229.js" aspectRatio={0.357} caption="cart.component.ts" />

You can find the live example [here](http://akita.surge.sh/).

### Summary

We’ve seen here how the various core concepts of Akita work together to give us an easy to manage shopping cart. This is only a small taste of Akita; It has many more additional features, such as support for active state, transactions, web workers, etc.

I encourage you to explore the API by reading the docs and the source code of the demo application which contains a todos page, a shopping cart, and login management.
