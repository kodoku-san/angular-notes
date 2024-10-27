#### [⬅️ Home](https://github.com/kodoku-san/angular-notes/blob/main/README.md)

![Angular](angular.svg)

# Router navigate in Angular

In Angular, `this.router.navigate()` is used to navigate from one component to another. It is versatile, supporting various types of navigations, from static routes to dynamic routes with parameters and query strings.

```typescript
import { Router } from '@angular/router';

@Component({
  selector: 'demo-router',
  standalone: true,
  imports: [
    ...
  ],
  templateUrl: '...',
  styleUrl: '...',
})
export class DemoRouterComponent {
  constructor(private router: Router) {}
}
```

### Using `router.navigate()`

1. **Basic Syntax**:
   ```typescript
   this.router.navigate(['/path']);
   ```
   - Here, `'/path'` is the route you want to navigate to. This method will automatically take the user to `/path`.

2. **Using an Array to Construct URL Segments**:
   - When you pass an array, Angular combines each element as segments in the URL path, ordered left-to-right:
   ```typescript
   this.router.navigate(['/path', 'subpath', 'id']);
   ```
   - This would create a URL like `/path/subpath/id`.

3. **Dynamic Route Parameters**:
   - You can add dynamic parameters by including values in the array, which is common when navigating to specific item pages:
   ```typescript
   this.router.navigate(['/users', userId, 'profile']);
   ```
   - If `userId` is `1`, this results in `/users/1/profile`.

4. **Query Parameters**:
   - To add query parameters (values after `?` in a URL), use the `queryParams` option:
   ```typescript
   this.router.navigate(['/path'], { queryParams: { id: 1, name: 'John' } });
   ```
   - The resulting URL would be `/path?id=1&name=John`.

5. **State**:
   - The `state` option allows you to pass data to the new component without showing it in the URL:
   ```typescript
   this.router.navigate(['/path'], { state: { data: 'example' } });
   ```
   - The new component can access this data via `history.state`.

6. **Relative Navigation**:
   - You can use `router.navigate()` to navigate relative to the current route, which is helpful for nested or context-based navigation:
   ```typescript
   this.router.navigate(['../'], { relativeTo: this.route });
   ```
   - Here, `../` moves the navigation one level up. You need to pass the current `ActivatedRoute` (`this.route`) to set the correct base route. You need to inject ActivatedRoute and pass it as the base route to the navigation command using the relativeTo property

    #### Step-by-Step Example

    6.1. **Inject `ActivatedRoute`** into your component:
    ```typescript
    import { Component } from '@angular/core';
    import { Router, ActivatedRoute } from '@angular/router';

    @Component({
      selector: 'app-example',
      templateUrl: './example.component.html',
    })
    export class ExampleComponent {
      constructor(
        private router: Router,
        private route: ActivatedRoute // Inject the current route
      ) {}
    }
    ```

    6.2. **Use `relativeTo`** when calling `this.router.navigate()`:
    ```typescript
    this.router.navigate(['../'], { relativeTo: this.route });
    ```
    - In this example, `['../']` navigates one level up from the current route. 
    - The `relativeTo: this.route` option tells Angular to interpret `['../']` based on the current route provided by `this.route`.

7. **Navigation with `replaceUrl`**:
   - Use `replaceUrl: true` if you want to navigate without adding the new route to the browser history:
   ```typescript
   this.router.navigate(['/new-path'], { replaceUrl: true });
   ```

### Additional Notes

- **CanDeactivate Guard**: If a route has a `CanDeactivate` guard, it will be checked during navigation with `router.navigate()`.
- **Return True/False in Guards**: Guards like `CanActivate` affect `router.navigate()` by blocking navigation if they return `false`.

### Combined Example

```typescript
// Navigate to `/admin/user/edit/1` with query and state options
this.router.navigate(
  ['/admin/user/edit', 1],
  {
    queryParams: { view: 'details' },
    state: { fromDashboard: true },
    replaceUrl: true
  }
);
```

With `router.navigate()`, Angular offers flexible navigation handling, allowing you to manage routes and URL states effectively within your application.

`KODOKU`
#### [⬅️ Home](https://github.com/kodoku-san/angular-notes/blob/main/README.md)
