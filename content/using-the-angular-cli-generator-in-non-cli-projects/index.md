---
title: "Using the Angular CLI Generator in Non- CLI Projects"
description: "Today, I was curious whether I could get to the point where I could use the Angular’s CLI ability in order to generate boilerplate files in an environment which wasn’t created by the CLI. The reason…"
date: "2018-03-19T15:56:50.049Z"
categories: 
  - JavaScript
  - Web Development
  - Angular

published: true
canonical_link: https://netbasal.com/using-the-angular-cli-generator-in-non-cli-projects-b8a02c326c0c
redirect_from:
  - /using-the-angular-cli-generator-in-non-cli-projects-b8a02c326c0c
---

![Tired of Boilerplate](./asset-1.jpeg)

Today, I was curious whether I could get to the point where I could use the Angular’s CLI ability in order to generate boilerplate files in an environment which wasn’t created by the CLI.

The reason I was curious was that there are applications and libraries that are not built on top of the CLI and yet could benefit from its boilerplate generator ability.

After a few attempts, I finally reached the desired result. Let me share with you the process.

Go to any project and run the following command:

```
npm install @angular/cli --save-dev 
```

In the root directory of your project, create an `.angular-cli.json` file and paste in it the following code:

```
{
 "apps": [{
   "appRoot": ".",
   "root": "lib",
   "prefix": "your-prefix"
 }],
 "defaults": {
   "styleExt": "scss",
   "component": {
     "changeDetection": "OnPush"
   }
 }
}
```

Change the `root` property to where you want the CLI to generate the files, and you’re done.

![Live Demo](./asset-2.gif)

The beauty is that it also works with schematics. For example, let’s use @ngrx/schematics.

```
npm install @ngrx/schematics --save-dev
```

![Live Demo](./asset-3.gif)

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Vue and JS!_
