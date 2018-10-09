Labs for the angular workshop at the angular days 2018 from [Christian Liebel](https://twitter.com/christianliebel?lang=en) and [Fabian Gosebrink](https://twitter.com/FabianGosebrink?lang=en)

## Start

[Stackblitz](https://stackblitz.com/)

## Labs

### 1. Bindings

Start: https://stackblitz.com/fork/angular

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

#### Solution

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

#### Event Binding (Advanced)
Adjust the implementations of `onClick()` and `onMouseMove()` to print the coordinates of the mouse (instead of printing `Hello!`)

Hints:
- `(click)="onClick($event)"`
- `public onClick(event: MouseEvent): void {}`

MouseEvent documentation: https://developer.mozilla.org/de/docs/Web/API/MouseEvent

#### Solution

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

#### Solution

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

#### Create a new component

Right-click the `app` folder and select _Angular Generator_, then _Component_.

![2018-10-09_18-17-34](https://user-images.githubusercontent.com/6698344/46683102-9f989780-cbef-11e8-8d0b-5455deb5cc37.png)

The new component should be named `todo`. Which files have been created? What’s the selector of the new component (`selector` property of `todo.component.ts`)?

#### Use the new component in your AppComponent’s template

Open the AppComponent’s template (i.e., HTML file) and use the new component there by adding an HTML element with the new component’s selector name (e.g., if the selector is `my-selector`, add `<my-selector></my-selector>` to the template).

If you like, you can duplicate this HTML element to see the idea of componentization in action.

#### Solution

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

#### Input

1. Extend your `TodoComponent` with an `@Input` field called `todo`.
2. Add a new `myTodo` field to the AppComponent and assign a todo object to it: `{ name: "Wash clothes", done: false, id: 3 }`
3. Pass the `myTodo` object to the `todo` component from the AppComponent’s template by using an input binding.
4. In the `TodoComponent`’s template, bind the value of the `todo` field to the UI using the `JSON` pipe.

#### Output

1. Extend your `TodoComponent` with an `@Output` field called `done`.
2. Add a `button` to your `TodoComponent` and an event binding for the `click` event of this button. When the button is clicked, emit the `done` event. Pass the current todo object as the event argument.
3. In the `AppComponent`’s template, bind to the `done` event using an event binding and log the finalized item to the console.

#### Solution

<details><summary>Show Solution</summary>

https://stackblitz.com/edit/angular-3bhmzs

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

<details><summary>Show Solution</summary>

https://stackblitz.com/edit/angular-ar3wnk

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
@Directive({
    selector: '[appColor]',
})
export class ColorDirective implements OnChanges {
    @Input()
    color: string;

    @HostBinding('style.color')
    hostColor = this.color;

    constructor(element: ElementRef) {
        this.hostColor = this.color;
    }

    ngOnChanges() {
        this.hostColor = this.color;
    }
}
```

click.directive.ts

```js
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

### 7. Dependency Injection

Start: https://stackblitz.com/edit/angular-ar3wnk

<details><summary>Show Solution</summary>

https://stackblitz.com/edit/angular-vjgnec

app.component.ts

```js
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
    console.log(todo)
  }

  logElementRef(){
    console.log("elementRef from console as property", this.elementRef);
  }
}
```

app.module.ts

```js
export const APP_NAME = new InjectionToken() < string > 'app-name';

@NgModule({
    imports: [BrowserModule, FormsModule],
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

<details><summary>Show Solution</summary>

https://stackblitz.com/edit/angular-jfyble

app.module.ts

```js
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

<details><summary>Show Solution</summary>

https://stackblitz.com/edit/angular-w7g8tc

app.component.ts

```js
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

<details><summary>Show Solution</summary>

https://stackblitz.com/edit/angular-dlrdvt

app.module.ts

```js
const appRoutes = [
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
