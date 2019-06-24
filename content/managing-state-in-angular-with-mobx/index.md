---
title: "💪 Managing State in Angular with Mobx"
description: "Every developer knows state management is difficult. Continually keeping track of what has been updated, why and when, can become a nightmare, especially in large applications. In the Angular world…"
date: "2017-10-30T16:21:35.837Z"
categories: 
  - JavaScript
  - Angular
  - Web Development
  - Redux

published: true
canonical_link: https://netbasal.com/managing-state-in-angular-with-mobx-51191803e14f
redirect_from:
  - /managing-state-in-angular-with-mobx-51191803e14f
---

![State Management](./asset-1.jpeg)

**Recently we released a new state management pattern. You should check it out.**

[**🚀 Introducing Akita: A New State Management Pattern for Angular Applications**  
_Every developer knows state management is difficult. Continuously keeping track of what has been updated, why, and…_netbasal.com](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8 "https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8")[](https://netbasal.com/introducing-akita-a-new-state-management-pattern-for-angular-applications-f2f0fab5a8)

Every developer knows state management is _difficult._ Continually keeping track of what has been updated, why and when, can become a nightmare, especially in large applications.

In the Angular world, several solutions can make state management less painful, complicated, and brittle.

Two of the most popular solutions are ngrx/store, which were inspired by the famous Redux and Observable Data Services.

Personally, I’m a big fan of Redux and think it’s worth **every** keystroke of boilerplate. But unfortunately, some don’t agree or use applications that are less suitable for Redux.

That’s why I’ve decided to explain how Mobx can be used to manage our application state. My idea is to combine the two worlds of Redux and Mobx while maintaining the smart and dumb (AKA stateless and stateful ) components architecture.

We’ll take Redux’s immutability, ngrx’s power of Rx, and Mobx’s state management abilities. This combination allows us to use the `async` pipe combined with the `OnPush` Change strategy to gain better performance.

Before we begin, this post assumes you have at least some working knowledge in Mobx and Angular.

For simplicity, we’ll create the traditional todos application. Let the fun begin.

### The Stores

I want to keep the [single](https://en.wikipedia.org/wiki/Single_responsibility_principle) responsibility principle, so I will create a store for both the `filter` and the `todos` (you can merge them if you prefer).

Let’s create the todos filter store.

```
import { Injectable } from '@angular/core';
import { action, observable} from 'mobx';

export type TodosFilter = 'SHOW_ALL' | 'SHOW_COMPLETED' | 'SHOW_ACTIVE';

@Injectable()
export class TodosFilterStore {
  @observable filter = 'SHOW_ALL';

  @action setFilter(filter: TodosFilter) {
    this.filter = filter;
  }

}
```

And the todos store.

```
export class Todo {
  completed = false;
  title : string;

  constructor( { title, completed = false } ) {
    this.completed = completed;
    this.title = title;
  }
}

@Injectable()
export class TodosStore {

  @observable todos: Todo[] = [new Todo({ title: 'Learn Mobx' })];

  constructor( private _todosFilter: TodosFilterStore ) {}
  
  @action addTodo( { title } : Partial<Todo> ) {
    this.todos = [...this.todos, new Todo({ title })]
  }

  @computed get filteredTodos() {
    switch( this._todosFilter.filter ) {
      case 'SHOW_ALL':
        return this.todos;
      case 'SHOW_COMPLETED':
        return this.todos.filter(t => t.completed);
      case 'SHOW_ACTIVE':
        return this.todos.filter(t => !t.completed);
    }
  }

}
```

If you are familiar with Mobx, the code above should seem straightforward.

Notably, it is a good practice to always use the `@action` decorator. This helps us to retain the “_Do not modify the state directly_” concept that we know from Redux. From Mobx docs:

> In strict mode, it is not allowed to change any state outside of an `[action](https://github.com/mobxjs/mobx/blob/gh-pages/docs/refguide/action.md)`.

### RxJS [Bridge](https://github.com/NetanelBasal/ngx-mobx)

One of the benefits of RxJS is the ability to convert every data source into an RxJS Observable. In our case, we will use the `computed` function from Mobx to listen to state changes and pass it forward to our subscribers.

```
import { Observable } from 'rxjs/Observable';
import { computed } from 'mobx';

export function fromMobx<T>( expression: () => T ) : Observable<T> {

  return new Observable(observer => {
    const computedValue = computed(expression);

    const disposer = computedValue.observe(changes => {
      observer.next(changes.newValue);
    }, true);

    return () => {
      disposer && disposer();
    }
  });
}
```

In Rx, `computed` is kind of like a `BehaviorSubject` combined with `distinctUntilChanged()`.

Every time there is a change ( a reference change ) in the _expression_, the callback will fire, which in turn pushes the new value to our subscribers. Now, we have a bridge between Mobx and Rx.

### Todo Component

Let’s create a `todo` component that takes the todo as `Input()` and emits an event when checked. Note that here we are leveraging the `**onPush**` change detection strategy.

```
@Component({
  selector: 'app-todo',
  template: `
    <input type="checkbox" 
           (change)="complete.emit(todo)" 
           [checked]="todo.completed">
    {{todo.title}}
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class TodoComponent {
  @Input() todo: Todo;
  @Output() complete = new EventEmitter();
}
```

### Todos Component

Let’s create a `todos` component that takes the list of todos as `Input()` and emits an event when checked. Note that we are leveraging the `**onPush**` change detection strategy.

```
@Component({
  selector: 'app-todos',
  template: `
    <ul>
      <li *ngFor="let todo of todos">
        <app-todo [todo]="todo" 
                  (complete)="complete.emit($event)">
        </app-todo>
      </li>
    </ul>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class TodosComponent {
  @Input() todos: Todo[] = [];
  @Output() complete = new EventEmitter();
}
```

### Todos Page Component

Now let’s put things together.

```
@Component({
  selector: 'app-todos-page',
  template: `
   <button (click)="addTodo()">Add todo</button> 
   <app-todos [todos]="todos | async"   
              (complete)="complete($event)">
    </app-todos>
  `
})
export class TodosPageComponent {
  todos : Observable<Todo[]>;

  constructor( private _todosStore: TodosStore ) {
  }
  
  ngOnInit() {
    this.todos = fromMobx(() => this._todosStore.filteredTodos);
  }

  addTodo() {
    this._todosStore.addTodo({ title: `Todo ${makeid()}` });
  }

}
```

If you have worked with ngrx/store, you should feel right at home. The `todos` property is an Rx observable and will only fire when there is a change in the `filteredTodos` property from our store.

The `filteredTodos` is a `computed` value that triggers a change if there is a **pure** change in the `filter` or the `todos` properties from our store.

And of course, because now it’s an Rx stream, we can access all of Rx’s benefits: `combineLatest()`, `take()`, etc..

That’s all. I will leave you to play with the full example.

<Embed src="https://stackblitz.com/edit/angular-mobx-netanel?embed=1" aspectRatio={undefined} caption="" />

I hope you liked this proof of concept and I hope I’ll see interest around it.

If you have any observations or comments, I’d love to hear.

_Follow me on_ [_Medium_](https://medium.com/@NetanelBasal/) _or_ [_Twitter_](https://twitter.com/NetanelBasal) _to read more about Angular, Vue and JS!_
