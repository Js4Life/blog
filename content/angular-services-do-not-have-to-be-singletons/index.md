---
title: "Angular Services do NOT have to be Singletons"
description: "As you probably know, when we add a service to a @NgModule() declaration, it will be a Singleton. Meaning, it will live as long as our application lives. For example: I see a lot of people whose…"
date: "2018-04-17T20:19:00.225Z"
categories: 
  - JavaScript
  - Angular
  - Web Development
  - Design Thinking

published: true
canonical_link: https://netbasal.com/angular-services-do-not-have-to-be-singletons-ffa879e62082
redirect_from:
  - /angular-services-do-not-have-to-be-singletons-ffa879e62082
---

![](./asset-1.jpeg)

As you probably know, when we add a service to a `@NgModule()` declaration, it will be a [Singleton](https://en.wikipedia.org/wiki/Singleton_pattern). Meaning, it will live as long as our application lives. For example:

```
export class AdminService {
  data = Array(10000).fill(dummy);
}

@NgModule({
  providers: [AdminService, AdminDataService]
})
```

I see a lot of people whose first instinct is to just put all their services in `@NgModule()` declarations without thinking about the consequences. The problem with that is:

> _You are not releasing memory_

Although you probably don’t need this memory anymore. Let’s stop for a second and think, do we really need this service to be an wide app singleton?

For example, in our application, we have an admin section where we need to display a big list of data which, of course, is stored in memory.

We don’t need this service to be a singleton. We don’t need a caching layer, and no one out of this tab needs data from it.

_The sole purpose of the service is to share data between child’s components and to provide a couple of helper’s methods._

So, we stick it in the **component** providers and make it a non-singleton service.

```
@Component({
  selector: 'admin-tab',
  providers: [AdminService, AdminDataService]
})
```

The benefit is that when Angular destroyed the component, Angular will also destroy the service and release the memory that is occupied by it.

### OnDestroy Hook

Many developers don’t know this, but non-singletons services also have the `ngOnDestroy()` lifecycle hook. You can use it for cleaning.

```
export class AdminService implements OnDestroy {

  ngOnDestroy() {
    // Clean subscriptions, intervals, etc
  }  
}
```

Also if we call `NgModuleRef.destroy()` or `PlatformRef.destroy()` then `ngOnDestroy` method of singleton providers will be also [executed](https://github.com/angular/angular/blob/674c3def319e2c444823319ae43394d46f3973b7/packages/core/src/view/ng_module.ts#L199-L204). Thanks to [Alexey Zuev](https://medium.com/@a.yurich.zuev) for the comment.

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Vue and JS!_

### 👂🏻 **Last but Not Least, Have you Heard of Akita?**

Akita is a state management pattern that we’ve developed here in Datorama. It’s been successfully used in a big data production environment for over seven months, and we’re continually adding features to it.

Akita encourages simplicity. It saves you the hassle of creating boilerplate code and offers powerful tools with a moderate learning curve, suitable for both experienced and inexperienced developers alike.

I highly recommend checking it out.

[**🚀 Introducing Akita: A New State Management Pattern for Angular Applications**  
_Every developer knows state management is difficult. Continuously keeping track of what has been updated, why, and…_netbasal.com](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8 "https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8")[](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8)
