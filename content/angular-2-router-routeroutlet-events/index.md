---
title: "Angular 2 Router — RouterOutlet Events"
description: "I want to share with you something that I discovered when going through the router source code. “Angular 2 Router — RouterOutlet Events” is published by Netanel Basal in Netanel Basal"
date: "2016-11-17T17:01:55.511Z"
categories: 
  - JavaScript
  - Angularjs
  - Angular2
  - Web Development

published: true
canonical_link: https://netbasal.com/angular-2-router-routeroutlet-events-8b0803d88082
redirect_from:
  - /angular-2-router-routeroutlet-events-8b0803d88082
---

![](./asset-1.png)

I want to share with you something that I discovered when going through the router source code.

The _router-outlet_ directive exposing two events:

1.  activate — emits any time a new component is instantiated.
2.  deactivate — emits when the component is destroyed.

For example:

```
<router-outlet
   (activate)='onActivate($event)'
   (deactivate)='onDeactivate($event)'>
</router-outlet>

export class AppComponent {

  onActivate(component) {
    // you have access to the component instance
  }

  onDeactivate(component) {
    // you have access to the component instance
  }

}
```

The only case that I can think of where it will be useful is if we need to add some global hook for all our routing components.

I am open to more ideas :)

_☞_ **_Please tap or click “︎_**❤” _to help to promote this piece to others._
