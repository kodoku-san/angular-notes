#### [⬅️Back to home](https://github.com/kodoku-san/angular-notes/blob/main/README.md)

![Angular](angular.svg)

# Debounce Time in Angular

## What is Debounce Time?

Debounce Time is a function that limits the rate at which a function can fire. It is a technique to ensure that time-consuming tasks do not fire so often, making it more efficient. It is a way to throttle the execution of a function so that it doesn't fire so often, making it more efficient.

Exemple in a search bar, when the user types a letter, the search function is called. If the user types quickly, the search function is called multiple times. This can be a problem if the search function is time-consuming. Debounce Time can be used to limit the rate at which the search function is called.

## How to use Debounce Time in Angular?

To use Debounce Time in Angular, you need to use the `debounceTime` operator from the RxJS library. The `debounceTime` operator is used to limit the rate at which a function can fire.

Here is an example of how to use `debounceTime` in Angular:

```typescript
import { Component } from "@angular/core";
import { debounceTime, fromEvent } from "rxjs";

@Component({
  selector: "app-root",
  template: ` <input type="text" (input)="onSearch($event.target.value)" /> `,
})
export class AppComponent {
  onSearch(value: string) {
    fromEvent(document.getElementById("search"), "input")
      .pipe(debounceTime(300)) // 300ms
      .subscribe(() => {
        console.log("Search:", value);
      });
  }
}
```

-  In this example, the `onSearch` function is called every time the user types a letter in the input field. The `debounceTime` operator is used to limit the rate at which the `onSearch` function is called. In this case, the `onSearch` function is called every 300ms.


In addition, if you want to delay any function, you can use `Subject` combined with `Subscription` in angular to do this.

```typescript
import { Component, OnDestroy } from "@angular/core";
import { Subject, Subscription } from "rxjs";

@Component({
  selector: "app-root",
  template: ` <button (click)="onClick()">Click me</button> `,
})
export class AppComponent implements OnDestroy {
  private subject = new Subject();
  private subscription: Subscription;

  constructor() {
    this.subscription = this.subject
      .pipe(debounceTime(300)) // 300ms
      .subscribe(() => {
        this.handleClick();
      });
  }

  onClick() {
    this.subject.next();
  }

  handleClick() {
    console.log("Button clicked");
  }

  ngOnDestroy() {
    this.subscription.unsubscribe();
  }
}
```

- In this example, the `onClick` function is called every time the user clicks the button. The `debounceTime` operator is used to limit the rate at which the `handleClick` function is called. In this case, the `handleClick` function is called every 300ms.

`KODOKU`

#### [⬅️Back to home](https://github.com/kodoku-san/angular-notes/blob/main/README.md)