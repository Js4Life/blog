---
title: "Angular Quick Tip — How to Show a Placeholder in Select Control"
description: "Angular Quick Tip — How to Show a Placeholder in Select Control. I find it very strange, but two times people have asked the same question in SO and still have not found a satisfactory answer."
date: "2017-07-19T19:33:38.258Z"
categories: 
  - JavaScript
  - Angularjs
  - Angular2
  - Web Development
  - HTML

published: true
canonical_link: https://netbasal.com/angular-quick-tip-how-to-show-a-placeholder-in-select-control-bab688f98b98
redirect_from:
  - /angular-quick-tip-how-to-show-a-placeholder-in-select-control-bab688f98b98
---

![](./asset-1.png)

I find it very strange, but two times people have asked the same question in SO and still have not found a satisfactory answer.

[How to show placeholder (empty option) in select control in Angular 2?](https://stackoverflow.com/questions/38725365/how-to-show-placeholder-empty-option-in-select-control-in-angular-2)

[Add placeholder to select tag in angular](https://stackoverflow.com/questions/34183686/add-placeholder-to-select-tag-in-angular2)2

Let me introduce you to my solution.

#### Reactive Forms

```
// template
<div class="form-group">
  <select formControlName="category">
    <option [ngValue]="null">Select Category</option>
    <option *ngFor="let option of options" 
            [ngValue]="option">{{option.label}}</option>
  </select>
</div>

// component
options = [{ id: 1, label: 'Category One' }, { id: 2, label: 'Category Two' }];

form = new FormGroup({
  category: new FormControl(null, Validators.required)
});
```

#### **Forms Module**

```
// template
<form #f="ngForm">
  <select name="state" [ngModel]="state">
    <option [ngValue]="null">Choose a state</option>
    <option *ngFor="let state of states" [ngValue]="state">
      {{ state.name }}
    </option>
  </select>
</form>

//component
state = null;

states = [
  {name: 'Arizona', code: 'AZ'},
  {name: 'California', code: 'CA'},
  {name: 'Colorado', code: 'CO'}
];
```

That’s all.

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Vue and JS!_
