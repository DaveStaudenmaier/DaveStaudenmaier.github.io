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

## Use Case for our new directive

A crazed visual designer has asked you to create a new look and feel for an input element in your app.   I say crazed because the visual designer has provided the following requirements for our Input field:
- Input should be an oval shape with no indication of focus to mess up the shape
- Limit of 10 characters
- Input should be twice the height of normal inputs
- Font size should be 24px
- When the user enters an odd number of characters, the Input background switches to blue
- When the user enters an even number of characters, the Input background switches to green
- Therefore as user enters characters the background will switch from blue to green to blue to green, etc.
- When the user reaches the 10 character limit, the shape returns to normal
- Placeholder value should be, quite fittingly, 'my funky input'

You decide to implement this in the component like this:

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

Renderer2 allows us to adjust the style of our Input element from typescript.

And it meets the requirements:

<img src="/images/home1.jpg" height="500px">

<img src="/images/blog/custom-attribute-directive/funky-input-screen-shot1.png" width="100px">

[![funky input](/images/blog/custom-attribute-directive/funky-input-screen-shot1.png)](https://www.youtube.com/watch?v=YOUTUBE_VIDEO_ID_HERE)
<video width="320" height="240" controls>
  
  <source src="/images/blog/custom-attribute-directive/funky-input1.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>
