---
title: "Exploring The New Meta Service in Angular Version 4"
description: "In Angular version 4 we have a new Meta service. “Exploring The New Meta Service in Angular Version 4” is published by Netanel Basal in Netanel Basal"
date: "2017-04-05T05:08:53.025Z"
categories: 
  - JavaScript
  - Angularjs
  - Angular2
  - Web Development
  - Html5

published: true
canonical_link: https://netbasal.com/exploring-the-new-meta-service-in-angular-version-4-b5ba2403d3e6
redirect_from:
  - /exploring-the-new-meta-service-in-angular-version-4-b5ba2403d3e6
---

![](./asset-1.jpeg)

In Angular version 4 we have a new Meta service.

> A service that can be used to get and add [meta](https://www.sitepoint.com/meta-tags-html-basics-best-practices/) tags.

Let’s explore the API.

<Embed src="https://gist.github.com/NetanelBasal/fd8f1567311c47525eba8824944eb408.js" aspectRatio={0.357} caption="" />

#### addTag (tag, forceCreation) : HTMLMetaElement

<Embed src="https://gist.github.com/NetanelBasal/e602e755cf000f7c214cd6b54e073ef5.js" aspectRatio={0.357} caption="" />

#### addTags (tags\[\], forceCreation) **:** HTMLMetaElement\[\]

<Embed src="https://gist.github.com/NetanelBasal/582aabcc5c524f372dc191814a9640a8.js" aspectRatio={0.357} caption="" />

#### getTag(attrSelector) : HTMLMetaElement

<Embed src="https://gist.github.com/NetanelBasal/c855fa4c9074e3cd38e30acd7054b581.js" aspectRatio={0.357} caption="" />

#### getTags(attrSelector) : HTMLMetaElement\[\]

<Embed src="https://gist.github.com/NetanelBasal/c3721bc32d9c4f3b9ed481d7cfcd33da.js" aspectRatio={0.357} caption="" />

#### updateTag(tag, selector**?**)**:** HTMLMetaElement

<Embed src="https://gist.github.com/NetanelBasal/8798186b14738b8245d4beef8e87977a.js" aspectRatio={0.357} caption="" />

#### removeTag(attrSelector)

<Embed src="https://gist.github.com/NetanelBasal/a0a32d964fdd984b7293509852b17647.js" aspectRatio={0.357} caption="" />

You can play with the p[lunker](https://plnkr.co/edit/8QzhQJ6Zna8OmlOwwev8?p=preview).

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Vue and JS!_

![](./asset-2.png)
