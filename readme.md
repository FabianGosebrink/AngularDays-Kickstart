Labs for the angular workshop at the angular days 2018 from [Christian Liebel](https://twitter.com/christianliebel?lang=en) and [Fabian Gosebrink](https://twitter.com/FabianGosebrink?lang=en)

## Start

[Stackblitz](https://stackblitz.com/)

## Labs

### 1. Bindings (only Input)

Start: https://stackblitz.com/

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

### 5. Input / Output

Start: https://stackblitz.com/edit/angular-jz9ivj

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
    { path: 'todos/:id', component: TodoEditComponent },
    { path: 'todos/new', component: TodoCreateComponent },
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
