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
