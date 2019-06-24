---
title: "Getting To Know The Partial Type in TypeScript"
description: "I don’t usually bother to write about such small things, but I’ve come across many people who don’t know this handy feature, so I decided I should. The problem with the code above is that we must…"
date: "2017-10-23T11:21:39.155Z"
categories: 
  - Typescript
  - JavaScript
  - Angular
  - Web Development

published: true
canonical_link: https://netbasal.com/getting-to-know-the-partial-type-in-typescript-ecfcfbc87cb6
redirect_from:
  - /getting-to-know-the-partial-type-in-typescript-ecfcfbc87cb6
---

![](./asset-1.png)

I don’t usually bother to write about such small things, but I’ve come across many people who don’t know this handy feature, so I decided I should.

Let’s say we have a `UserModel` interface:

```
interface UserModel {
  email: string;
  password: string;
  address: string;
  phone: string;
}
```

And a `User` class with `update()` method:

```
class User {
  update( user: UserModel ) {
    // Update user
  }
}
```

The problem with the code above is that we must pass an object that implements the whole `UserModel` interface, otherwise typescript will be 😡.

But in our case, we want to be dynamic and not be committed to the entire interface, but still get IntelliSense.

TypeScript (v2.1) provides us with a solution precisely for these cases — The `Partial` interface. All we need to do is to change the code above to this:

```
class User {
  update( user: Partial<UserModel> ) {
    // Update user
  }
}
```

Now we can have the best of both worlds.

Another useful example would be if you have a component that takes configuration object as `Input()` and you want to have a default value.

```
type ComponentConfig = {
  optionOne: string;
  optionTwo: string;
  optionThree: string;
}

export class SomeComponent {

  private _defaultConfig: Partial<ComponentConfig> = {
    optionOne: '...'
  }

  @Input() config: ComponentConfig;
  
  ngOnInit() {
    const merged = { ...this._defaultConfig, ...this.config };
  }
}
```

Under the hood the `Partial` interface looks like this:

```
type Partial<T> = { [P in keyof T]?: T[P]; };
```

You can read more about the `keyof` feature [here](https://blog.mariusschulz.com/2017/01/06/typescript-2-1-keyof-and-lookup-types).

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Vue and JS!_
