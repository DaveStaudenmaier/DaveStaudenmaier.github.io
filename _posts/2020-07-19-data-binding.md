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

{% raw %}
```html
  <p>Name: {{ name }}</p>
```
{% endraw %}

Here is what the user sees:

<img src="/images/blog/data-binding/name.png" width="300px">

But what is meant by 'template expression'?   Angular will convert to a string any expression within the curly brackets. 

{% raw %}
```html
Number = {{ 1 + 2 }}
```
{% endraw %}

Will display as **Number = 3**

If we have a boolean property called 

```typescript
wasHeABeatle = true
```
 
And in the template we have 

{% raw %}
```html
He was a Beatle = {{ wasHeABeatle }}
```
{% endraw %}

Will display as **He was a Beatle = true**

### Property Binding

In the property binding, a template expression appears in quotes to the right of the = symbol as in [property]="expression".

Let's say we wanted to display a picture of this Richard Starkey.  We could use property binding like this:

**component** 
```typescript
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

Another example of property binding is passing a property reference from a parent component to a child component.  In our example app, I created a component called `name` that uses the `@Input` decorator to get a reference to the name property in `app.component`.

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

Here we pass a reference to name in `app.component.ts` (i.e. `="name"`) to the name.component.ts name (i.e. [name]), which we can now display in our child template.   I created an input element in name.component.html and defaulted the value to the name from app.component.ts.

```html
<input matInput [(ngModel)]="name">
```
Whoa, what is this ngModel?   We could have simply displayed the name in our name.component.ts using property binding (i.e. `[value]="name"`), but we want to update our name property with the user updated name.   This is called two-way data binding which will we discuss in the next section.

Here is what the user sees so far:

<img src="/images/blog/data-binding/name-with-update.png" width="300px">


### Event binding

Event binding allows you to listen for certain events such as keystrokes, mouse movements, clicks, and touches.

In our app, I added a button in `home.component.html` that uses event binding to listen for the click event and bind to the onSubmit method in home.component.ts.

**home component HTML**
```html
<button mat-raised-button
        color="primary"
        (click)="onSubmit()">
  Submit
</button>
```
**home component**
```typescript
@Output() newName = new EventEmitter<string>();

onSubmit() {
  this.newName.emit(this.name);
}
```

The onSubmit() method uses the @Output decorator to emit the latest value of the name property (see two-way binding) on how that was updated.  

And in our app.component.html file, we are also using event binding to bind to `newName` in `name.component.ts` on the @Output decorator, and bind to the updateName method in `app.component.ts`, which updates the name property which is then reflected through string interpolation to the app.component template, as we saw above.    Note that we pass the event to the updateName method using the `$event` object.   Since all we emitted in the home.componet was the string 'name', then that is the value of the event object, but typically the event object has a rich set of properties you may want to use. 

**app component HTML**
```html
<app-name
  [name]="name"
  (newName)="updateName($event)">
</app-name>
```

**app component**
```typescript
updateName(name: string) {
  this.name = name;
}
```

Let's go over two-way data binding and then we'll see a video of how the app looks with what we've done so far.

### Two-way data binding 

Two-way binding gives our app a way to share data between a component class and its template, but in both directions.  Without two-way binding, if we wanted to update the value of the name property in `name.component.ts` with the updated value, we would need to use event binding to update the value based on some event with the input, such as `blur`.  

Two-way data binding uses the ngModel directive to make this simple by combining property binding and event binding.   So `property binding` + `event binding` = `two-way binding`.

For our app, we saw above that we defaulted the value of the input to the `name` property.  When the user updates the name, we want the `name` property to be updated automatically.  All we have to do is the 'banana-in-a-box' with ngModel [()].  The syntax of `[(ngModel)]="property"` tells Angular to do what we need.  

```html
<input matInput [(ngModel)]="name">
```

Let's see how our app looks now in this video I've posted on YouTube.

<a href="https://youtu.be/WeE-RYaOPvg"><img src="/images/blog/data-binding/ringo.png" width="200px" target="_blank"></a>

### Class binding

You can add and remove CSS class names from an element's class attribute with a class binding.  

To create a single class binding, start with the prefix class followed by a dot (.) and the name of the CSS class (for example, [class.foo]="hasFoo"). Angular adds the class when the bound expression is truthy, and it removes the class when the expression is falsy (with the exception of undefined).

Let's change our name header on our app.component to turn red when it is updated.  To do that we add a new CSS class and use class binding to add the class when a boolean is set to true when the name is updated.   

**CSS**
```css
.name {
  color: red;
}
```

**component**
```typescript
updatedName = false;
```

**HTML**
```html
<p [class.name]="updatedName">Name: {{ name }}</p>
```
When `updatedName` is set to `true`, the `name` class will be applied to the element.  

### Style binding

Similar to class binding, you can set styles dynamically using style binding.  For instance `[style.width]="width"`.

Let's create a new property in app.component.ts called `fontWeight` and change it when the name is updated.

**component**
```typescript
fontWeight = 'default';

  updateName(name: string) {
  this.name = name;
  this.updatedName = true;
  this.fontWeight = '900';
  }
```

**HTML**
```html
<p [class.name]="updatedName" [style.font-weight]="fontWeight">Name: {{ name }}</p>
```

Now let's see what our class binding and style binding looks like in this video on YouTube:

<a href="https://youtu.be/VvRNwXgrrgM"><img src="/images/blog/data-binding/ringo.png" width="200px" target="_blank"></a>

## Conclusion

We have used all of the different types of data binding Angular provides in our little app that you can find on my [GitHub](https://github.com/DaveStaudenmaier/data-binding).

Feel free to contact me at [dave@dev-reboot.com](mailto:dave@dev-reboot.com) if you have any questions or comments!

Keep on developing!
