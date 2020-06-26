---
layout: post
title: Angular data binding
---

In this blog, I will explain the different types of data binding that Angular supports and show examples of each in a simple app. 

As usual, you can find a fully working project on my [GitHub](https://github.com/DaveStaudenmaier/data-binding).

Angular Version: 9.1.0

----
****

## What is data binding?

**Definition from the [Angular documentation](https://angular.io/guide/glossary#data-binding)** 

Data binding is a process that allows apps to display data values to a user and respond to user actions (such as clicks, touches, and keystrokes).

In data binding, you declare the relationship between an HTML widget and a data source and let the framework handle the details. Data binding is an alternative to manually pushing application data values into HTML, attaching event listeners, pulling changed values from the screen, and updating application data values.

## What different types of data binding does Angular support?

Angular supports these different types of data binding which we will discuss through examples
- Interpolation
- Property binding
- Event binding
- Two-way data binding with ngModel
- Attribute binding
- Class binding
- Style binding

## Data bindings in action

I will show snippets of an Angular demo app you can find on GitHub and explain through these exmaples.   The app displays the name of a person with a picture and allows you to update the name from a child component.  So we have app.component and name.component.

### Interpolation

Embedding expressions (i.e. template expressions) into marked up text is referred to as `interpolation`.  Interpolation allows you to incporporate a component property into the text between HTML element tags and within attribute assignments.   Interoplation uses double curly brackets to embed the {{ property }}. 

In app.component.ts we have a property called name which we display in the template via interpolation:

**component**
```typescript
 name = 'Richard Starkey';
```

**HTML**
```html
  <p>Name: {{ name }}</p>
```

Here is what the user sees:

<img src="/images/blog/data-binding/name.png" width="300px">

But what is meant by 'template expression'?   Angular will convert to a string any expression within the curly brackets. 

**Number = {{ 1 + 2 }**

Will dsplay as **Number = 3**

If we have a boolean property called **wasHeABeatle = true**
 
And in the template we have **He was a Beatle = {{ wasHeABeatle }}**

Will display as **He was a Beatle = true**

### Property Binding

In the property binding, a template expression appears in quotes to the right of the = symbol as in [property]="expression".

Let's say we wanted to display a picture of this Richard Starkey.  We could use property binding like this:

**component** 
```template
profilePic = './../assets/images/ringo-Keystone-hutton-archives-getty.jpg';
```

**HTML**

```html
<img class="image" [src]="profilePic">
```
The square brackets around the element attribute `[src]` indicate to Angular that this is an expression to be evaluated.  

Note that you could also have used interpolation in this example, in which case you would not use the square brackets:

```html
<img class="image" src="{{ profilePic }}">
```

Another example of property binding is passing a prooperty reference from a parent component to a child component.  In our example app, I created a component called `name` that uses the `@Input` decorator to get a reference to the name property in app.component.

**name component**

```typescript
@Input('name') name: string;
```

**app.component HTML**

```html
<app-name
  [name]="name"
</app-name>
```

Here we pass a reference to name in our app.component.ts (i.e. `="name") to the name.component.ts name (i.e. [name]), which we can now display in our child template.   I created an input element in name.component.html and defaulted the value to the name from app.component.ts.

```html
<input matInput [(ngModel)]="name">
```
Whoa, what is this ngModel?   We could have simply displayed the name in our name.component.ts using property binding (i.e. [value]="name"), but we want to update our name property with the user updated name.   This is called two-way data binding which will we discuss in the next section.

Here is what the user sees so far:

<img src="/images/blog/data-binding/name-with-update.png" width="300px">

### Two-way data binding 


As a reminder, you can find a fully working project on my [GitHub](https://github.com/DaveStaudenmaier/data-binding)

Feel free to contact me at [dave@dev-reboot.com](mailto:dave@dev-reboot.com) if you have any questions or comments. 

Keep on developing!
