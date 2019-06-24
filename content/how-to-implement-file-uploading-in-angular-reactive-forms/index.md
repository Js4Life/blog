---
title: "How to Implement File Uploading in Angular Reactive Forms"
description: "In this article, I’ll walk you through the process of creating a reactive form that includes a file upload, along with the normal form fields. Along the way, we’ll create a custom form control for…"
date: "2019-03-19T06:55:25.061Z"
categories: 
  - JavaScript
  - Angular
  - Angular2
  - Web Development

published: true
canonical_link: https://netbasal.com/how-to-implement-file-uploading-in-angular-reactive-forms-89a3fffa1a03
redirect_from:
  - /how-to-implement-file-uploading-in-angular-reactive-forms-89a3fffa1a03
---

![](./asset-1.png)

In this article, I’ll walk you through the process of creating a reactive form that includes a file upload, along with the normal form fields. Along the way, we’ll create a custom form control for our file input, add validation and create custom RxJS operators.

Our demo will include a simple sign up form with a single text field, for the user’s email, and an upload file field, where we’ll require the user to upload a profile photo.

Here is an illustration of the final result:

![](./asset-2.gif)

Let’s get started.

### Creating the Form

First let’s create the sign-up form:

<Embed src="https://gist.github.com/NetanelBasal/0ce2232e06162b1e502fa0c623ae3677.js" aspectRatio={0.357} caption="" />

We’ve created a form group that contains the `email` and `image`, and defined both as required. In addition to that, for the image control, we added a custom validation for the file type. We’ll see the implementation of this validator later on.

### Creating the File Upload Component

Let’s continue with the file upload component:

<Embed src="https://gist.github.com/NetanelBasal/30e036ef74e274ca1529716d75a43138.js" aspectRatio={0.357} caption="" />

We registered a `change` event listener that emits the files that the user uploads. In our case we’re interested only in one file, so we’ll use the `item()` method, passing the first index to obtain a reference to it.

We also have a progress component that’s responsible for the loading progress UI (code omitted for the sake of brevity).

### Creating a Custom Form Control

At this point, we have a problem. Angular doesn’t come with a built-in value accessor for file input, so we’ll get the following error:

> Error: No value accessor for form control with name: ‘image’

What Angular means is that it doesn’t know how to connect our component to the form API. Let’s inform Angular on how to do that by creating a custom value accessor:

<Embed src="https://gist.github.com/NetanelBasal/87b6811039eb45e5286e8b3922da2af7.js" aspectRatio={0.357} caption="" />

I’m not going to get into the details of what actually happens here because I have an article dedicated to this topic:

[**Angular Custom Form Controls Made Easy**  
_Imagine that you need to implement an auto-expand textarea for one of your forms._netbasal.com](https://netbasal.com/angular-custom-form-controls-made-easy-4f963341c8e2 "https://netbasal.com/angular-custom-form-controls-made-easy-4f963341c8e2")[](https://netbasal.com/angular-custom-form-controls-made-easy-4f963341c8e2)

The important thing to note here is that upon file change we set the control value to be the chosen file, and that whenever we call `control.patchValue(null)` or `control.reset()` we’re clearing the file input.

Let’s see what we get by listening to the form’s `valueChanges` event:

![](./asset-3.gif)

Great, our `image` control contains the file data.

### Creating a Custom Validation

As we saw earlier, we want to create a custom validator that allows uploading only files with the `png` file extension:

<Embed src="https://gist.github.com/NetanelBasal/267f7bfb94e67c08c170a190553fdadf.js" aspectRatio={0.357} caption="" />

Whenever the user uploads a file, we return a `true` (invalid) value if its extension is not the same as the one we defined in the validator. Otherwise we return `null`, which indicates that the validation passed.

![](./asset-4.gif)

I’ve omitted the code that shows the errors for the sake of brevity. If you want to learn how you can magically show a control’s errors, read the following article:

[**Make Your Angular Form’s Error Messages Magically Appear**  
_In this article, we’re going to learn how to develop a generic method that displays validation errors in Angular’s…_netbasal.com](https://netbasal.com/make-your-angular-forms-error-messages-magically-appear-1e32350b7fa5 "https://netbasal.com/make-your-angular-forms-error-messages-magically-appear-1e32350b7fa5")[](https://netbasal.com/make-your-angular-forms-error-messages-magically-appear-1e32350b7fa5)

### Send it to the Server

We’ve reached the point where we have the information we need, so now it’s time to send it to our server. Let’s implement the `submit` method:

<Embed src="https://gist.github.com/NetanelBasal/be0e816fe4caa5fb9a01ba7f75099a52.js" aspectRatio={0.357} caption="" />

First, we need to use the `[FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData)` API in order to construct an object containing the selected value’s fields, so that they can be sent to the server via Ajax.

We create a pure and reusable function that takes an object, in our case the form value, and returns a `FormData` instance containing it:

<Embed src="https://gist.github.com/NetanelBasal/e4655cce5e8e43c92077f12b54256cd2.js" aspectRatio={0.357} caption="" />

Next, we want to get notified about the progress of the upload, so we need to set the `reportProgress` option to true and the `observe` option to `events`.

We then subscribe to our observable to initiate the request, and listen to the different event types over the course of the request lifecycle. Now we can react appropriately depending on the event type.

It’s worth noting that in real life we’d opt to use a service rather than using the HTTP service directly in the component.

### Room for Improvement

I want to take the code a step further and make it reusable, and also write it in a reactive way. Let’s create two custom operators to make that possible:

<Embed src="https://gist.github.com/NetanelBasal/f80e43f1f39602b4cb55f25b84783ac2.js" aspectRatio={0.357} caption="" />

Building your own operator is as simple as writing a function which takes a source `observable` as an input and returns an output stream. We can compose forms by seamlessly adding our custom operator to other operators.

Now we can modify the existing code as follows:

<Embed src="https://gist.github.com/NetanelBasal/a4602081fe3b4fa4089280dea8ae6a87.js" aspectRatio={0.357} caption="" />

Much better.

### The Server Endpoint

We’ll conclude by showing an example of our server endpoint. In this case, we’ll use Node and express, but the client-side code will work with any server-side implementation.

<Embed src="https://gist.github.com/NetanelBasal/5feeb86e2cc8428bf4a002e07939e289.js" aspectRatio={0.357} caption="" />

### 🔥 **Last but Not Least, Have you Heard of Akita?**

Akita is a state management pattern that we’ve developed here in Datorama. It’s been successfully used in a big data production environment, and we’re continually adding features to it.

Akita encourages simplicity. It saves you the hassle of creating boilerplate code and offers powerful tools with a moderate learning curve, suitable for both experienced and inexperienced developers alike.

I highly recommend checking it out.

[**🚀 Introducing Akita: A New State Management Pattern for Angular Applications**  
_Every developer knows state management is difficult. Continuously keeping track of what has been updated, why, and…_netbasal.com](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8 "https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8")[](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8)

[**😍 You Asked for It, We Listened, and Now it’s Here - Akita v3**  
_Akita 3.0 is finally here! We’ve listened to your feature requests and incorporated them in this brand new version of…_engineering.datorama.com](https://engineering.datorama.com/you-asked-for-it-we-listened-and-now-its-here-akita-v3-92740d0d72e4 "https://engineering.datorama.com/you-asked-for-it-we-listened-and-now-its-here-akita-v3-92740d0d72e4")[](https://engineering.datorama.com/you-asked-for-it-we-listened-and-now-its-here-akita-v3-92740d0d72e4)

[**Form Fatale: How Akita’s Form Manager Can Do Away with Complex Multistep Form Logic in Angular**  
_Akita’s Angular Form Manager_netbasal.com](https://netbasal.com/form-fatale-how-akitas-form-manager-can-do-away-with-complex-multistep-form-logic-in-angular-329a557cc68 "https://netbasal.com/form-fatale-how-akitas-form-manager-can-do-away-with-complex-multistep-form-logic-in-angular-329a557cc68")[](https://netbasal.com/form-fatale-how-akitas-form-manager-can-do-away-with-complex-multistep-form-logic-in-angular-329a557cc68)

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Akita and JS!_

[**NetanelBasal/ng-file-upload**  
_Contribute to NetanelBasal/ng-file-upload development by creating an account on GitHub._github.com](https://github.com/NetanelBasal/ng-file-upload "https://github.com/NetanelBasal/ng-file-upload")[](https://github.com/NetanelBasal/ng-file-upload)
