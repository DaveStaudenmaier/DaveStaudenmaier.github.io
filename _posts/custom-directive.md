---
layout: post
title: Create a custom attribute directive in Angular
---

In this blog, I explain the different types of directives in Angular and we will create a custom attribute directive and use it in a template.

As usual, you can find a fully working project on my [GitHub](https://github.com/DaveStaudenmaier/custom-attribute-directive)

Angular Version: 9

----
****

## What is a directive and what are the different types?

## What might we use an attribute directive for?

Let's suppose a crazed visual designer has asked us to create a new look and feel for an input element in your app.   I say crazed because the visual designer has provided the following requirements for our input field:
- input should be an oval shape with no indication of focus to mess up the shape
- Limit of 10 characters
- Input should be twice the height of normal inputs
- Font size should be 24px
- When the user enters an odd number of characters, the Input background switches to blue
- When the user enters an even number of characters, the Input background switches to green
- Therefore as user enters characters the background will switch from blue to green to blue to green, etc.
- When the user reaches the 10 character limit, the shape returns to normal
- Placeholder value should be, quite fittingly, 'my funky input'


Assuming this is a one-off request, we grit our teeth and decide to implement this in the component like this:

**HTML**
```html
<div>
  <input #funky
          maxlength="10"
          [ngClass]="keyCount < 10 ? 'funky-input': 'normal-input'"
          placeholder="my funky input">
</div>
```

**CSS**
```css
.funky-input {
  line-height: 2.5;
  border-radius: 50%;
  padding: 5px;
  outline: none;
  font-size: 24px;
}

.normal-input {
  line-height: 2.5;
  border-radius: default;
  padding: 5px;
  outline: default;
  font-size: 24px;
}
```

**Typescript**
```typescript
export class AppComponent implements AfterViewInit {

  constructor(private renderer: Renderer2) {}

  keyCount = 0;
  @ViewChild('funky') element: ElementRef;

  ngAfterViewInit() {
    this.renderer.listen(this.element.nativeElement, 'keydown', event => {
      if (this.keyCount % 2 == 0) {
      this.renderer.setStyle(this.element.nativeElement, 'background-color', 'green');
      this.renderer.setStyle(this.element.nativeElement, 'color', 'white');
      } else {
        this.renderer.setStyle(this.element.nativeElement, 'background-color', 'blue');
        this.renderer.setStyle(this.element.nativeElement, 'color', 'white');
      }
      this.keyCount++;
    });
  }
}
```

Renderer2 allows us to adjust the style of our Input element from typescript and listen for the `keydown` event on our input element.

And it meets the requirements (click to view on YouTube)

<a href="https://youtu.be/wiDZ0qVknqk"><img src="/images/blog/custom-attribute-directive/funky-input-screen-shot1.png" width="200px"></a>

Unfortunately, the visual designer loves it so much, he wants to implement it consistently across the entire application!  Not only that, but he wants to make the colors configurable to match color schemes in various components.  Ugg.  Now we will have to duplicate this code in every component with an input element and maintain all that code and make sure it is consistent!

But wait, what if we create a custom attribute directive?  Then all of our code would be in one place and we could easily implement it consistently across the entire application!

So, we use the Angular CLI to add a directive to our app:

**`ng g d funky-input`**

And we code up our Directive like this: 

```typescript
import { Directive, ElementRef, Renderer2, OnInit, Input } from '@angular/core';

@Directive({
  selector: '[appFunkyInput]'
})
export class FunkyInputDirective implements OnInit {
  @Input() oddColor: string;
  @Input() evenColor: string;

  keyCount = 0;

  constructor(private element: ElementRef,
              private renderer: Renderer2) { }

  // The element that the directive is applied to is associated with our element reference
  ngOnInit() {
    this.renderer.setAttribute(this.element.nativeElement, 'maxlength', '10');
    this.renderer.setStyle(this.element.nativeElement, 'line-height', '2.5');
    this.renderer.setStyle(this.element.nativeElement, 'border-radius', '50%');
    this.renderer.setStyle(this.element.nativeElement, 'padding', '5px');
    this.renderer.setStyle(this.element.nativeElement, 'outline', 'none');
    this.renderer.setStyle(this.element.nativeElement, 'font-size', '24px');
    this.renderer.setProperty(this.element.nativeElement, 'placeholder', 'my funky input');
    this.element.nativeElement.focus();

    this.renderer.listen(this.element.nativeElement, 'keydown', event => {
      if (this.keyCount % 2 == 0) {
      this.renderer.setStyle(this.element.nativeElement, 'background-color', this.evenColor);
      this.renderer.setStyle(this.element.nativeElement, 'color', 'white');
      } else {
        this.renderer.setStyle(this.element.nativeElement, 'background-color', this.oddColor);
        this.renderer.setStyle(this.element.nativeElement, 'color', 'white');
      }
      this.keyCount++;

      if (this.keyCount > 9 ) {
        this.renderer.removeStyle(this.element.nativeElement, 'border-radius');
        this.renderer.removeStyle(this.element.nativeElement, 'red');
        this.renderer.removeStyle(this.element.nativeElement, 'outline');
      }
    });
  }
}
```

We've taken in the odd and even colors as inputs using the @Input decorator.   It turns our Renderer2 is pretty powerful.  We can pretty much do anything that we might normally do through our HTML and CSS.  Now, we're using Renderer2 to set styles, attributes and properties, set focus to our element and remove styles, as well as listen for the `keydown` event on our input field!  

And the html is very simple now.  All we have to do is use our directive's selector on an input field and use property binding to communicate our color choices.  Here are two implementations with different color choices. 

```html
<div>
  <input appFunkyInput
          [oddColor]="'blue'"
          [evenColor]="'green'">
</div>

<br>

<div>
  <input appFunkyInput
          [oddColor]="'orange'"
          [evenColor]="'purple'">
</div>
```

It still looks awful but at least all the code is in one place when the visual designer comes to his senses and wants us to change it!

<a href="https://youtu.be/_obj-qX_8rA"><img src="/images/blog/custom-attribute-directive/funky-input-screen-shot1.png" width="200px" target="_blank"></a>

## Conclusion
