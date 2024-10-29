#### [⬅️ Home](https://github.com/kodoku-san/angular-notes/blob/main/README.md)

![Angular](angular.svg)

# ng-content in Angular

In Angular, `ng-content` is used to create **components that can contain dynamic content**. It allows you to pass external content into a component while maintaining its structure, making it highly useful for building **reusable components** like modals, cards, or containers where the inner content varies depending on the specific use case.

### Basic Usage of `ng-content`

The following example creates a `card` component that uses `ng-content` to insert content within it:

1. **Generate the component**:

   ```bash
   ng generate component card
   ```

2. **HTML structure for the component (`card.component.html`)**:

   ```html
   <div class="card">
     <h2>Card Title</h2>
     <ng-content></ng-content> <!-- ng-content area -->
   </div>
   ```

3. **Using the `card` component within another component**:

   ```html
   <app-card>
     <p>This is content inside the card!</p>
   </app-card>
   ```

When Angular renders this, the HTML output will look like:

```html
<div class="card">
  <h2>Card Title</h2>
  <p>This is content inside the card!</p>
</div>
```

### Using `ng-content` with Selectors

`ng-content` can also use **selectors** to insert different sections of content in various parts of a component. Here’s an example:

**`card.component.html`**:

```html
<div class="card">
  <h2><ng-content select="[header]"></ng-content></h2>
  <div class="content">
    <ng-content></ng-content>
  </div>
  <footer>
    <ng-content select="[footer]"></ng-content>
  </footer>
</div>
```

**Using the `card` component**:

```html
<app-card>
  <div header>Header Content</div>
  <p>Main Content inside the card</p>
  <div footer>Footer Content</div>
</app-card>
```

The rendered result would look like this:

```html
<div class="card">
  <h2>Header Content</h2>
  <div class="content">
    <p>Main Content inside the card</p>
  </div>
  <footer>Footer Content</footer>
</div>
```

### Benefits of `ng-content`

1. **Reusability**: Enables the creation of reusable components with dynamic content.
2. **Preserved HTML structure**: The elements passed into the component retain their original structure.
3. **Flexible content configuration**: You can control the content placement using selectors and attributes.

With `ng-content`, Angular provides flexibility for injecting customizable content into components, making it easier to create adaptable and reusable UI elements.

`KODOKU`
#### [⬅️ Home](https://github.com/kodoku-san/angular-notes/blob/main/README.md)