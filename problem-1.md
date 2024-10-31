#### [⬅️ Home](https://github.com/kodoku-san/angular-notes/blob/main/README.md)

![Angular](angular.svg)

## Problem:
Want to close a modal when pressing the Escape key by adding an event listener to `keydown`. The initial code might look like this:

```typescript
document.addEventListener('keydown', this.escapeClose.bind(this));

escapeClose(e: KeyboardEvent): void {
  if (e.key === 'Escape') this.closeModal();
}

ngOnDestroy(): void {
  document.removeEventListener('keydown', this.escapeClose.bind(this));
}
```

### Explanation of the Issue:
The problem arises because `this.escapeClose.bind(this)` creates a new function reference each time it’s called. When you add the event listener with this bound function, it won’t match the one you pass to `removeEventListener` in `ngOnDestroy`. This means the event listener is not removed when the component is destroyed, which can lead to memory leaks and the following error:

```
NG0953: Unexpected emit for destroyed OutputRef
```

### Solution:
To avoid this, you should bind the function only once, store it as a property on the class, and then use that single reference to add and remove the event listener.

### Implementation in Angular:
Here’s how to refactor the code to prevent this issue:

```typescript
import { Component, OnDestroy } from '@angular/core';

@Component({
  selector: 'app-your-component',
  templateUrl: './your-component.component.html',
  styleUrls: ['./your-component.component.css']
})
export class YourComponent implements OnDestroy {
  // Store the bound function as a property
  private escapeCloseBound = this.escapeClose.bind(this);

  constructor() {
    // Add the event listener using the pre-bound function
    document.addEventListener('keydown', this.escapeCloseBound);
  }

  escapeClose(e: KeyboardEvent): void {
    if (e.key === 'Escape') this.closeModal();
  }

  closeModal(): void {
    // Modal closing logic here
    console.log('Modal closed');
  }

  ngOnDestroy(): void {
    // Remove the event listener with the same reference
    document.removeEventListener('keydown', this.escapeCloseBound);
  }
}
```

### Why This Works:
1. **Consistent Reference**: By binding `escapeClose` once in the constructor, `this.escapeCloseBound` always points to the same function reference.
2. **Correct Event Removal**: Since the function reference remains the same, `removeEventListener` in `ngOnDestroy` correctly identifies and removes the event listener.
3. **Avoiding Memory Leaks**: This ensures the event listener is cleaned up when the component is destroyed, preventing unwanted behavior and memory leaks.

### Demo Explanation:
1. **Add Listener**: In the constructor, `document.addEventListener('keydown', this.escapeCloseBound)` attaches the event listener when the component is initialized.
2. **Event Handling**: The `escapeClose` method checks if the key pressed is 'Escape' and calls `closeModal` to close the modal.
3. **Cleanup**: In `ngOnDestroy`, `removeEventListener` removes the event listener using the exact same reference, ensuring that Angular doesn’t throw errors related to emitting from a destroyed component.

`KODOKU`
#### [⬅️ Home](https://github.com/kodoku-san/angular-notes/blob/main/README.md)