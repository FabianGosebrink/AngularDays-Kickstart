Labs for the angular workshop at the angular days 2018 from [Christian Liebel](https://twitter.com/christianliebel) and [Fabian Gosebrink](https://twitter.com/FabianGosebrink)

## Start

[Stackblitz](https://stackblitz.com/)

## Labs

### 1. Bindings

Start: https://stackblitz.com/fork/angular

<details><summary>Show Labs</summary>
	
#### Interpolation
In your freshly created project, open the file `src/app/app.component.html` and try the following bindings (one after another). You can completely remove the existing contents of this file.

1. `{{ 'hallo' }}`
2. `{{ 3 }}`
3. `{{ 17 + 4 }}`
4. `{{ '<div>Does this work?</div>' }}`
5. `{{ alert('boom') }}`

Which values do you see in the preview pane? Are there any error messages?

#### Interpolation II
Now, open the file `src/app/app.component.ts` and introduce a new field called `value` within the `AppComponent` class:

```ts
export class AppComponent {
  // …
  public value = "Hello";
}
```

Bind the value of this field to the template file, by adding the following interpolation to `src/app/app.component.html`.

```html
{{ value }}
```

Then, `Hello` should show up in the preview pane.

#### Property Binding

1. Declare a new field called `color` on your component instance and initialize it with a CSS color value (e.g., `hotpink`)
2. Create a new `div` element in the AppComponent’s HTML template (Hint: `<div></div>`)
3. Bind the value of the field to the background color of the `div` element (Hint—add the following attribute assignment to the `div` node: `[style.backgroundColor]="color"`)

The square brackets are not a typo! They might look odd, but it woll work.

#### Event Binding

1. Implement a new method `onClick` on the component instance that opens an alert box (Hint: `public onClick() { alert('Hello!'); }`)
2. Create a new `button` element in the AppComponent’s HTML template (Hint: `<button>Click me.</button>`)
3. Bind the click event of the button to the `onClick` method (Hint—add the following attribute assignment to the `button` node: `(click)="onClick()"`)
4. Implement a new method `onMouseMove` on the component instance that logs to the console (Hint: `console.log('Hello!')`)
5. Bind the `mousemove` event of the button to `onMouseMove`

Again, the brackets are not a typo. It will work out just fine.

</details>

<details><summary>Show Solution</summary>

https://stackblitz.com/edit/angular-5zuu2g

```js
export class AppComponent  {
  public value = "Hello";
  public color = "hotpink";

  public onClick(): void {
    alert('Hello!');
  }

  public onMouseMove(): void {
    console.log('Hello!');
  }
}
```

```html
{{ 'hallo' }} <br/>
{{ 3 }} <br/>
{{ 17 + 4 }} <br/>
{{ '<div>Does this work?</div>' }} <br/>

<hr/>

{{ value }}

<hr/>

<div [style.backgroundColor]="color">Test</div>


<button (click)="onClick()" (mousemove)="onMouseMove()">Click me.</button>
```

</details>

### 2. Bindings (Event with $event)

Start: https://stackblitz.com/edit/angular-5zuu2g

<details><summary>Show Labs</summary>
	
#### Event Binding (Advanced)
Adjust the implementations of `onClick()` and `onMouseMove()` to print the coordinates of the mouse (instead of printing `Hello!`)

Hints:
- `(click)="onClick($event)"`
- `public onClick(event: MouseEvent): void {}`

MouseEvent documentation: https://developer.mozilla.org/de/docs/Web/API/MouseEvent

</details>

<details><summary>Show Solution</summary>

https://stackblitz.com/edit/angular-zyc9xx

```js
export class AppComponent  {
  public value = "Hello";
  public color = "hotpink";

  public onClick(event: MouseEvent): void {
    alert(event.clientX);
  }

  public onMouseMove(event: MouseEvent): void {
    console.log(event.clientX);
  }
}
```

```html
<button (click)="onClick($event)" (mousemove)="onMouseMove($event)">Click me.</button>
```

</details>

### 3. Pipes

Start: https://stackblitz.com/edit/angular-zyc9xx

<details><summary>Show Labs</summary>
	
#### Interpolation

Adjust your value binding from lab #1 to be printed as lowercase (Hint: `{{ value | lowercase }}`).

Then, adjust it to be printed as UPPERCASE.

#### Built-in pipes

Add a new numeric field to your AppComponent (e.g., `public number = 3.14159;`). Bind this field to the template using the pipes:
- `percent`
- `currency`
- `number` (showing five decimal places)

Please use three interpolations (`{{ number | … }} {{ number | … }} {{ number | … }}`).

#### Create a new pipe

Right-click the `app` folder and select _Angular Generator_, then _Pipe_.

![image](https://user-images.githubusercontent.com/6698344/46677681-5fcbb300-cbe3-11e8-85a2-c7577374e7fc.png)

The pipe should be called `yell`. Open the generated file `yell.pipe.ts`.

Implement the yell pipe as follows:
- The yell pipe should suffix the bound value with three exclamation marks (e.g., `value + '!!!'` or `` `${value}!!!` ``).
- The developer can optionally pass an argument to override the suffix (`args` parameter).

| Interpolation                 | Value    |
| ----------------------------- | -------- |
| `{{ value \| yell }}`         | Hello!!! |
| `{{ value \| yell:'???' }}`   | Hello??? |

</details>

<details><summary>Show Solution</summary>

https://stackblitz.com/edit/angular-82f7cm

```js
export class AppComponent  {
  public value = "Hello";
  public number = 3.14159;
}
```

```js
@Pipe({
    name: 'yell',
})
export class YellPipe implements PipeTransform {
    transform(value: string, args: string): any {
        const suffix = args || '!!!';

        return `${value}${suffix}`;
    }
}
```

```html
{{ value | uppercase }}	<br/>

{{ number | percent }}	 <br/>
{{ number | currency }}	<br/>
{{ number | number:'0.5' }}	<br/>


{{ value | yell }}<br/>
{{ value | yell:'???' }}
```

</details>

### 4. Components

Start: https://stackblitz.com/edit/angular-82f7cm

<details><summary>Show Labs</summary>
	
#### Create a new component

Right-click the `app` folder and select _Angular Generator_, then _Component_.

![2018-10-09_18-17-34](https://user-images.githubusercontent.com/6698344/46683102-9f989780-cbef-11e8-8d0b-5455deb5cc37.png)

The new component should be named `todo`. Which files have been created? What’s the selector of the new component (`selector` property of `todo.component.ts`)?

#### Use the new component in your AppComponent’s template

Open the AppComponent’s template (i.e., HTML file) and use the new component there by adding an HTML element with the new component’s selector name (e.g., if the selector is `my-selector`, add `<my-selector></my-selector>` to the template).

If you like, you can duplicate this HTML element to see the idea of componentization in action.

</details>

<details><summary>Show Solution</summary>

https://stackblitz.com/edit/angular-jz9ivj

todo.component.ts

```js
@Component({
    selector: 'app-todo',
    templateUrl: './todo.component.html',
    styleUrls: ['./todo.component.css'],
})
export class TodoComponent implements OnInit {
    constructor() {}

    ngOnInit() {}
}
```

app.component.html

```html
<app-todo></app-todo>
```

</details>

### 5. Input/Output

Start: https://stackblitz.com/edit/angular-jz9ivj

<details><summary>Show Labs</summary>
	
#### Input

1. Extend your `TodoComponent` with an `@Input` field called `todo`.
2. Add a new `myTodo` field to the AppComponent and assign a todo object to it: `{ name: "Wash clothes", done: false, id: 3 }`
3. Pass the `myTodo` object to the `todo` component from the AppComponent’s template by using an input binding.
4. In the `TodoComponent`’s template, bind the value of the `todo` field to the UI using the `JSON` pipe.

#### Output

1. Extend your `TodoComponent` with an `@Output` field called `done`.
2. Add a `button` to your `TodoComponent` and an event binding for the `click` event of this button. When the button is clicked, emit the `done` event. Pass the current todo object as the event argument.
3. In the `AppComponent`’s template, bind to the `done` event using an event binding and log the finalized item to the console.

</details>

<details><summary>Show Solution</summary>

https://stackblitz.com/edit/angular-3bhmzs

todo.component.ts

```js
import { Input, Output, EventEmitter, OnInit } from '@angular/core';

@Component({
  selector: 'app-todo',
  templateUrl: './todo.component.html',
  styleUrls: ['./todo.component.css']
})
export class TodoComponent implements OnInit {

  @Input() todo: any;

  @Output() done = new EventEmitter<any>();

  constructor() { }

  ngOnInit() {
  }

  markTodoAsDone(){
    this.done.emit(this.todo);
  }

}
```

todo.component.html

```html
<p>
inside todo-component: <br/>
{{todo | json}}
</p>

<button (click)="markTodoAsDone()">mark as done</button>
```

app.component.html

```html
<app-todo [todo]="todoObject" (done)="catchDoneEvent($event)"></app-todo>
```

app.component.ts

```js
@Component({
  selector: 'my-app',
  templateUrl: './app.component.html',
  styleUrls: [ './app.component.css' ]
})
export class AppComponent  {
  public todoObject = { name: "Wash clothes", done: false, id: 3 }

  catchDoneEvent(todo: any) {
    console.log(todo)
  }
}
```

</details>

### 6. Directives

Start: https://stackblitz.com/edit/angular-3bhmzs

<details><summary>Show Labs</summary>
	
#### Create a color directive

Right-click the `app` folder and select _Angular Generator_, then _Directive_. Create a directive (e.g., named `color`) that takes a color as an input binding. The directive should set the color of the host element (using a host binding).

#### Create a click directive

Create another directive (e.g., named `click`) that adds a click handler to the elements where it’s placed on. Whenever the item is clicked, log a message to the console.

</details>

<details><summary>Show Solution</summary>

https://stackblitz.com/edit/angular-ar3wnk

todo.component.ts

```js
import { Input, Output, EventEmitter, OnInit } from '@angular/core';

@Component({
  selector: 'app-todo',
  templateUrl: './todo.component.html',
  styleUrls: ['./todo.component.css']
})
export class TodoComponent implements OnInit {

  @Input() todo: any;

  @Output() done = new EventEmitter<any>();

  colorToBind = "blue";

  constructor() { }

  ngOnInit() {
  }

  markTodoAsDone(){
    this.done.emit(this.todo);
  }
}
```

todo.component.html

```html
<p appClick appColor color="green">
  inside todo-component: <br/>
  {{todo | json}}
</p>
<p>
  <span appColor [color]="colorToBind">test to apply directive on</span>
</p>

<button (click)="markTodoAsDone()">mark as done</button>
```

color.directive.ts

```js
import { Directive, Input, HostBinding } from '@angular/core';

@Directive({
  selector: '[appColor]'
})
export class ColorDirective {

  @HostBinding('style.color')
  @Input() color: string;
}
```

click.directive.ts

```js
import { Directive, Input, HostListener } from '@angular/core';

@Directive({
    selector: '[appClick]',
})
export class ClickDirective {
    @HostListener('click', ['$event'])
    handleClick($event): void {
        console.log('a message');
    }

    constructor() {}
}
```

</details>

### 7. Dependency Injection/Services

Start: https://stackblitz.com/edit/angular-ar3wnk

<details><summary>Show Labs</summary>
	
#### Injecting ElementRef

In your AppComponent…
1. `import {ElementRef} from '@angular/core';`
2. Request an instance of `ElementRef` via constructor injection
3. Log the instance to the console
4. Inspect it
5. Is the instance provided by the root injector, a module or a component?

#### Injection Tokens

In your AppModule…
1. Define an `APP_NAME` injection token (string)
2. Provide it in the module providers and assign it a certain value
3. Consume it from the `AppModule`’s constructor
4. Print the name to the console

#### Create a new service

Right-click the `app` folder and select _Angular Generator_, then _Class_.

Create a new model class called `todo` and add the properties:
- `name` (string)
- `done` (boolean)
- `id` (number, optional)

Right-click the `app` folder and select _Angular Generator_, then _Service_.

In your TodoService, add the following methods:
- `create(todo: Todo): Todo`
- `get(todoId: number): Todo`
- `getAll(): Todo[]`
- `update(todo: Todo): void`
- `delete(todoId: number): void`

Add a very basic, synchronous implementation for getAll. Inject your TodoService into the AppComponent (don’t forget to update the imports on top). Log the list of todos to the console.

</details>

<details><summary>Show Solution</summary>

https://stackblitz.com/edit/angular-vjgnec

app.component.ts

```js
import { ElementRef } from '@angular/core';

@Component({
  selector: 'my-app',
  templateUrl: './app.component.html',
  styleUrls: [ './app.component.css' ]
})
export class AppComponent  {
  public todoObject = { name: "Wash clothes", done: false, id: 3 }

  constructor(private readonly elementRef: ElementRef,
  private readonly todoService: TodoService){
    console.log("elementRef from constructor", elementRef);

    console.log(todoService.getAll());
  }

  catchDoneEvent(todo: any) {
    console.log(todo);
  }

  logElementRef(){
    console.log("elementRef from console as property", this.elementRef);
  }
}
```

app.module.ts

```js
import { NgModule, InjectionToken, Inject } from '@angular/core';
// other imports

export const APP_NAME = new InjectionToken<string>('app-name');

@NgModule({
  imports:      [ BrowserModule, FormsModule ],
  declarations: [ AppComponent, 
                  HelloComponent, 
                  YellPipe, 
                  TodoComponent, 
                  ColorDirective, 
                  ClickDirective ],
  providers: [{ provide: APP_NAME, useValue: 'My cool app' }, TodoService],
  bootstrap:    [ AppComponent ]
})
export class AppModule {
  constructor(@Inject(APP_NAME) appName: string){
    console.log(appName);
  }
 }

```

todo.ts

```js
export class Todo {
  name: string;
  done: boolean;
  id?:number;
}
```

todo.service.ts

```js
@Injectable()
export class TodoService {

  private todos: Todo[] = [];

  constructor() { }

  create(todo: Todo) { }

  get(todoId: number)  { }

  getAll(): Todo[]  {
    return this.todos;
  }

  update(todo: Todo): void  { }

  delete(todoId: number): void  { }

}
```

</details>

### 8. Structural Directives

Start: https://stackblitz.com/edit/angular-vjgnec

<details><summary>Show Labs</summary>
	
#### *ngIf

In your AppComponent’s template, add the following snippet:

```html
<button (click)="toggle()">Toggle</button>
<div *ngIf="show">
  I’m visible!
</div>
```

On the component class, introduce a new `show` field and toggle it via a new `toggle()` method (Hint: `this.show = !this.show;`).

#### *ngFor

In the AppComponent, introduce a new field todos and assign the return value of todoService.getAll() to it.
Bind this field to the view using the `*ngFor` structural directive and an unordered list (`ul`) with one list item (`li`) for each todo:

```html
<ul>
  <li *ngFor="let todo of todos"></li>
</ul>
```

Next, iterate over your TodoComponent (app-todo) instead and pass the todo via the todo property binding. Adjust the template of TodoComponent to include:
- a checkbox (input) to show the “done” state
- a label to show the “name” text

```html
<label>
	<input type="checkbox" [checked]="todo.done">
	{{ todo.name }}
</label>
```

</details>

<details><summary>Show Solution</summary>

https://stackblitz.com/edit/angular-mznjjg

app.component.ts

```js
@Component({
  selector: 'my-app',
  templateUrl: './app.component.html',
  styleUrls: [ './app.component.css' ]
})
export class AppComponent  {

  private show = true;
  todos = [];

  constructor(private readonly elementRef: ElementRef,
  private readonly todoService: TodoService){
    console.log("elementRef from constructor", elementRef);

    this.todos = todoService.getAll();
  }

  catchDoneEvent(todo: any) {
    console.log(todo)
  }

  logElementRef(){
    console.log("elementRef from console as property", this.elementRef);
  }

  toggle() {
    this.show = !this.show;
  }
}
```

app.component.html

```html
<button (click)="toggle()">Toggle</button>	
<div *ngIf="show">	
	I am visible!	
</div>	
 <ul>	
  <li *ngFor="let todo of todos">{{todo.name}}</li>	
</ul>	
 <app-todo *ngFor="let todo of todos" [todo]="todo" (done)="catchDoneEvent($event)"></app-todo>	
```

todo.service.ts

```js
@Injectable()
export class TodoService {

  private todos: Todo[] = [];

  constructor() {
    this.todos.push({ name: "Wash clothes", done: false, id: 3 });
  }

  create(todo: Todo) {

  }

  get(todoId: number)  {}

  getAll(): Todo[]  {
    return this.todos;
  }

  update(todo: Todo): void  {}

  delete(todoId: number): void  {}

}
```

todo.component.ts

```js
import { Import, Output } from '@angular/core';

@Component({
  selector: 'app-todo',
  templateUrl: './todo.component.html',
  styleUrls: ['./todo.component.css']
})
export class TodoComponent implements OnInit {

  @Input() todo: any;

  @Output() done = new EventEmitter<any>();

  colorToBind = "blue";

  constructor() { }

  ngOnInit() {
  }

  markTodoAsDone(todo: Todo) {
    todo.done = !todo.done;
    this.done.emit(todo);
  }
}
```

todo.component.html

```html
<label>
  <input type="checkbox" [checked]="todo.done" (change)="markTodoAsDone(todo)">{{ todo.name }}
</label>
```

</details>

### 9. Observables

Start: https://stackblitz.com/edit/angular-mznjjg

<details><summary>Show Labs</summary>
	
#### Adjust service

Adjust your `TodoService` to now return Observables and upgrade the synchronous value in `getAll()` to an Observable (via `of()`).
- `create(todo: Todo): Observable<Todo>`
- `get(todoId: number): Observable<Todo>`
- `getAll(): Observable<Todo[]>`
- `update(todo: Todo): Observable<void>`
- `delete(todoId: number): Observable<void>`

#### Use HttpClient

In your AppModule, add HttpClientModule to the imports array

Add a constructor to TodoService and request an instance of HttpClient and use HTTP requests instead of returning synchronous data using the following URLs:

| Method | Action     | URL                                        |
| ------ | ---------- | ------------------------------------------ |
| GET    | get all    | https://tt-todos.azurewebsites.net/todos   |
| GET    | get single | https://tt-todos.azurewebsites.net/todos/1 |
| POST   | create     | https://tt-todos.azurewebsites.net/todos   |
| PUT    | update     | https://tt-todos.azurewebsites.net/todos/1 |
| DELETE | delete     | https://tt-todos.azurewebsites.net/todos/1 |

</details>

<details><summary>Show Solution</summary>

https://stackblitz.com/edit/angular-jfyble

app.module.ts

```js
import { HttpClientModule } from '@angular/common/http';

@NgModule({
    imports: [BrowserModule, FormsModule, HttpClientModule],
    declarations: [
        AppComponent,
        HelloComponent,
        YellPipe,
        TodoComponent,
        ColorDirective,
        ClickDirective,
    ],
    providers: [{ provide: APP_NAME, useValue: 'My cool app' }, TodoService],
    bootstrap: [AppComponent],
})
export class AppModule {
    constructor(@Inject(APP_NAME) appName: string) {
        console.log(appName);
    }
}
```

todo.service.ts

```js
@Injectable()
export class TodoService {

  private actionUrl = "https://tt-todos.azurewebsites.net/todos"

  constructor(private readonly httpClient: HttpClient) { }

  create(todo: Todo) {
    return this.httpClient.post<Todo>(this.actionUrl, todo);
  }

  get(todoId: number)  {
    return this.httpClient.get<Todo>(`${this.actionUrl}/${todoId}`);
  }

  getAll(): Observable<Todo[]>  {
    return this.httpClient.get<Todo[]>(this.actionUrl);
  }

  update(todo: Todo)  {
    return this.httpClient.put(`${this.actionUrl}/${todo.id}`, todo);
  }

  delete(todoId: number)  {
    return this.httpClient.delete(`${this.actionUrl}/${todoId}`);
  }
}
```

app.component.ts

```js
import { ElementRef } from '@angular/core';

@Component({
  selector: 'my-app',
  templateUrl: './app.component.html',
  styleUrls: [ './app.component.css' ]
})
export class AppComponent  {

  private show = true;
  todos = [];

  constructor(private readonly elementRef: ElementRef,
  private readonly todoService: TodoService){
    console.log("elementRef from constructor", elementRef);

    todoService.getAll().subscribe(todos => this.todos = todos);
  }

  catchDoneEvent(todo: any) {
    console.log(todo)
  }

  logElementRef(){
    console.log("elementRef from console as property", this.elementRef);
  }

  toggle() {
    this.show = !this.show;
  }
}
```

</details>

### 10. Async Pipe

Start: https://stackblitz.com/edit/angular-jfyble

<details><summary>Show Labs</summary>
	
#### Use Async Pipe

Use the `async` pipe instead of manually subscribing.

**Instead of:**
```ts
public todos: Todo[];
```

**Use:**
```ts
public todos$: Observable<Todo[]>;
```

**Instead of:**
```ts
todoService.getAll().subscribe(todos => this.todos = todos);
```

**Use:**
```ts
this.todos$ = todoService.getAll();
```

**Instead of:**
```ts
<app-todo *ngFor="let todo of todos" [todo]="todo">
</app-todo>
```

**Use:**
```ts
<app-todo *ngFor="let todo of todos$ | async" [todo]="todo">
</app-todo>
```
</details>

<details><summary>Show Solution</summary>

https://stackblitz.com/edit/angular-w7g8tc

app.component.ts

```js
import { ElementRef } from '@angular/core';

@Component({
  selector: 'my-app',
  templateUrl: './app.component.html',
  styleUrls: [ './app.component.css' ]
})
export class AppComponent implements OnInit {

  private show = true;
  todos$: Observable<Todo[]>;

  constructor(private readonly elementRef: ElementRef,
  private readonly todoService: TodoService){
    console.log("elementRef from constructor", elementRef);
  }

  ngOnInit() {
    this.todos$ = this.todoService.getAll();
  }

  catchDoneEvent(todo: any) {
    console.log(todo)
  }

  logElementRef(){
    console.log("elementRef from console as property", this.elementRef);
  }

  toggle() {
    this.show = !this.show;
  }
}
```

app.component.html

```html
<div *ngIf="todos$ | async as todos">
	You have {{ todos.length }} todos!
</div>

<ul>
	<li *ngFor="let todo of todos$ | async">
		{{ todo.name }}
	</li>
</ul>

<app-todo *ngFor="let todo of todos$ | async" [todo]="todo" (done)="catchDoneEvent($event)"></app-todo>
```

</details>

### 11. Routing

Start: https://stackblitz.com/edit/angular-w7g8tc

<details><summary>Show Labs</summary>
	
#### Generate components	
Add the following components:	
- TodoListComponent	
- TodoEditComponent	
- TodoCreateComponent	
- NotFoundComponent	

#### Define routes	
Define/assign the following routes:	
- todos	
- todos/:id	
- todos/new	
- **	

Redirect the default (empty) route to the todo list.	

#### Router outlet	
Add a `<router-outlet>` to your AppComponent:	
```html
<router-outlet></router-outlet>
```
Then try out different routes by typing them into the address bar.	
- Which parts of the page change?	
- Which parts stay the same?	

#### Router links	
In your AppComponent, define two links:	
- Home (/todos)	
- Create (/todos/new)

In TodoListComponent, request all todos and update the template:	
```html	
<ul>	
  <li *ngFor="let todo of todos$ | async"><a [routerLink]="todo.id">{{ todo.name }}</a></li>	
</ul>	
```	
#### Active router links	
In AppComponent, add routerLinkActive:	
```html	
<a routerLink="/todos" routerLinkActive="my-active">Home</a>	
```	
Or, if you prefer:	
```html	
<a routerLink="/todos" routerLinkActive="my-active" [routerLinkActiveOptions]="{ exact: true }">Home</a>	
```	
Add a CSS style for a.my-active	
#### Activated route	
 In TodoEditComponent, listen for changes of the ActivatedRoute and retrieve the record with the given ID from the TodoService and bind it to the view as follows:	
 ```	
{{ todo$ | async | json }}	
```	

</details>

<details><summary>Show Solution</summary>

https://stackblitz.com/edit/angular-dlrdvt

app.module.ts

```js
import { RouterModule, Routes } from '@angular/router';

const appRoutes: Routes = [
    { path: '', redirectTo: 'todos', pathMatch: 'full' },
    { path: 'todos', component: TodoListComponent },
    { path: 'todos/new', component: TodoCreateComponent },
    { path: 'todos/:id', component: TodoEditComponent },
    { path: '**', component: NotFoundComponent },
];

export const APP_NAME = new InjectionToken() < string > 'app-name';

@NgModule({
    imports: [
        BrowserModule,
        FormsModule,
        HttpClientModule,
        RouterModule.forRoot(appRoutes, { useHash: false }),
    ],
    declarations: [
        AppComponent,
        HelloComponent,
        YellPipe,
        TodoComponent,
        TodoEditComponent,
        TodoListComponent,
        TodoCreateComponent,
        NotFoundComponent,
        ColorDirective,
        ClickDirective,
    ],
    providers: [{ provide: APP_NAME, useValue: 'My cool app' }, TodoService],
    bootstrap: [AppComponent],
})
export class AppModule {
    constructor(@Inject(APP_NAME) appName: string) {
        console.log(appName);
    }
}
```

app.component.ts

```js
@Component({
    selector: 'my-app',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.css'],
})
export class AppComponent {}
```

app.component.html

```html
<a routerLink="/todos" routerLinkActive="my-active">Home</a> |
<a routerLink="/todos/new" routerLinkActive="my-active">Create</a>
<hr>
<br/>
<router-outlet></router-outlet>
```

todo.component.ts

```js
@Component({
  selector: 'app-todo',
  templateUrl: './todo.component.html',
  styleUrls: ['./todo.component.css']
})
export class TodoComponent implements OnInit {

  @Input() todo: any;

  @Output() done = new EventEmitter<any>();

  constructor() { }

  ngOnInit() {
  }

  markTodoAsDone(todo: Todo) {
    todo.done = !todo.done;
    this.done.emit(todo);
  }
}
```

todo.component.html

```html
<label >
  <input type="checkbox" [checked]="todo.done" (change)="markTodoAsDone(todo)">
  <a [routerLink]="todo.id">{{ todo.name }}</a>
</label>
```

todo-edit.component.ts

```js
@Component({
  selector: 'app-todo-edit',
  templateUrl: './todo-edit.component.html',
  styleUrls: ['./todo-edit.component.css']
})
export class TodoEditComponent implements OnInit {

  public todo$: Observable<Todo>;

  constructor(private readonly activatedRoute: ActivatedRoute,
              private readonly todoService: TodoService) { }

  ngOnInit() {
    this.todo$ = this.activatedRoute.params.pipe(
      pluck('id'),
      switchMap(id => this.todoService.get(+id))
    );
  }
}
```

todo-edit.component.html

```html
<p>
{{ todo$ | async | json }}
</p>
```

</details>

### 12. Template Forms

Start: https://stackblitz.com/edit/angular-dlrdvt

<details><summary>Show Labs</summary>
	
#### Add a form	
 In TodoEditComponent, update the template to contain the following form. It should have to fields: A text field for editing the name and a checkbox for setting the done state. Implement onSubmit and send the updated todo to the server.	

```html	
<form *ngIf="todo$ | async as todo" (ngSubmit)="onSubmit(todo)">	
	<!-- … -->	
	<button>Submit!</button>	
</form>	
```	

#### Validation	
 Now, add a required and minlength (5 characters) validation to the name field. Update the submit button to be disabled when the form is invalid:	
 ```html	
<form *ngIf="todo$ | async as todo" (ngSubmit)="onSubmit(todo)" #form="ngForm">	
	<!-- … -->	
	<button [disabled]="form.invalid">Submit!</button>	
</form>	
```	

</details>

<details><summary>Show Solution</summary>

https://stackblitz.com/edit/angular-4goufd

todo-edit.component.html

```html
<form *ngIf="todo$ | async as todo" (ngSubmit)="onSubmit(todo)" #form="ngForm">
  <input type="checkbox" [(ngModel)]="todo.done" name="done">
  <input type="text" [(ngModel)]="todo.name" name="name" required minlength="5">
  <button [disabled]="form.invalid">Submit!</button>
</form>
```

todo-edit.component.ts

```js
import { ActivatedRoute } from '@angular/router';

@Component({
  selector: 'app-todo-edit',
  templateUrl: './todo-edit.component.html',
  styleUrls: ['./todo-edit.component.css']
})
export class TodoEditComponent implements OnInit {

  public todo$: Observable<Todo>;

  constructor(private readonly activatedRoute: ActivatedRoute,
              private readonly todoService: TodoService) { }

  ngOnInit() {
    this.todo$ = this.activatedRoute.params.pipe(
      pluck('id'),
      switchMap(id => this.todoService.get(+id))
    );
  }

  onSubmit(todo: Todo) {
    this.todoService.update(todo).subscribe();
  }
}
```

</details>
