#### [⬅️ Home](https://github.com/kodoku-san/angular-notes/blob/main/README.md)

![Angular](angular.svg)

# Subject RxJS in Angular

## What is RxJS?

RxJs is a library for reactive programming using Observables, to make it easier to compose asynchronous or callback-based code. It provides one core type, the Observable, satellite types (Observer, Schedulers, Subjects) and operators to allow you to define complex asynchronous workflows by using functional programming.

## What is a Subject?

A Subject is a special type of Observable that allows values to be multicasted to many Observers. While plain Observables are unicast (each subscribed Observer owns an independent execution of the Observable), Subjects are multicast.

Every Subject is an Observable. Given a Subject, you can subscribe to it, providing an Observer, which will start receiving values normally. From the perspective of the Observer, it cannot tell whether the Observable execution is coming from a plain unicast Observable or a Subject.

Example in an application context: A Subject can be used to share data between components, or to send notifications to multiple components at the same time.

### Example 1: Using Subject to share data between components

```typescript
import { Injectable } from '@angular/core';
import { Subject, BehaviorSubject, Observable } from 'rxjs';

interface User {
  id: number;
  name: string;
}

@Injectable({
  providedIn: 'root'
})
export class UserService {
  private userSubject = new BehaviorSubject<User | null>(null);
  private notificationSubject = new Subject<string>();

  user$: Observable<User | null> = this.userSubject.asObservable();
  notification$: Observable<string> = this.notificationSubject.asObservable();

  constructor() {}

  setUser(user: User) {
    this.userSubject.next(user);
  }

  clearUser() {
    this.userSubject.next(null);
  }

  sendNotification(message: string) {
    this.notificationSubject.next(message);
  }

  getCurrentUser(): User | null {
    return this.userSubject.getValue();
  }
}
```

In this example:

1. `BehaviorSubject`: Used for `userSubject` because it needs to store the current value and emit it to new subscribers.

2. `Subject`: Used for `notificationSubject` because it doesn't need to store the current value.

3. `Observable`: Created from Subjects using the `asObservable()` method. This allows components using the service to only subscribe but not emit new values.

To use this service in a component, you can do the following:

```typescript
import { Component, OnInit, OnDestroy } from '@angular/core';
import { UserService } from './user.service';
import { Subscription } from 'rxjs';

@Component({
  selector: 'app-user-profile',
  template: `
    <div *ngIf="currentUser">
      <h2>Welcome, {{ currentUser.name }}!</h2>
    </div>
    <div *ngIf="notification">{{ notification }}</div>
  `
})
export class UserProfileComponent implements OnInit, OnDestroy {
  currentUser: User | null = null;
  notification: string = '';
  private userSubscription: Subscription;
  private notificationSubscription: Subscription;

  constructor(private userService: UserService) {}

  ngOnInit() {
    this.userSubscription = this.userService.user$.subscribe(user => {
      this.currentUser = user;
    });

    this.notificationSubscription = this.userService.notification$.subscribe(message => {
      this.notification = message;
    });
  }

  ngOnDestroy() {
    this.userSubscription.unsubscribe();
    this.notificationSubscription.unsubscribe();
  }
}
```

In this component:

1. We subscribe to the Observables `user$` and `notification$` from the UserService.

2. When something changes, the component updates its properties accordingly.

3. We unsubscribe in `ngOnDestroy` to avoid memory leaks.

This is how RxJS and Angular work together to manage state and events in the application in a reactive way.

`KODOKU`
#### [⬅️ Home](https://github.com/kodoku-san/angular-notes/blob/main/README.md)
