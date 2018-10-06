Labs for the angular workshop at the angular days 2018 from [Christian Liebel](https://twitter.com/christianliebel?lang=en) and [Fabian Gosebrink](https://twitter.com/FabianGosebrink?lang=en)

## Labs:

### Bindings (only Input)

https://stackblitz.com/edit/angular-5zuu2g

<details><summary>Show Solution</summary>

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

### Bindings (Event with $event)

https://stackblitz.com/edit/angular-zyc9xx

<details><summary>Show Solution</summary>

```js
public onClick(event: MouseEvent): void {
  alert(event.clientX);
}
public onMouseMove(event: MouseEvent): void {
  console.log(event.clientX);
}
```

```html
<button (click)="onClick($event)" (mousemove)="onMouseMove($event)">Click me.</button>
```

</details>
