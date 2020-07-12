---
layout: post
title: Practical, real-world animation examples with Angular
---

In this blog, I will who show some examples of practical, real-world animations using Angular, and will explain how they work.   Here are the animations we will cover: 
- A slide-down animation exposing an Add Post component's template
- A fade in for the new post applied
- A thumbnail photo that will expand smoothly to full-size when clicked and shrink back down when clicked again
- An animation that will draw attention to an element
- Two variations of animation when adding to a dynamic list.  One will fade in.  The other will slide in from the left. 
- Fade animation when changing routes, configured in one place

As usual, you can find a fully working project on my [GitHub](https://github.com/DaveStaudenmaier/animations).

I recommend downloading the project from GitHub because I will not be going through details like how to set up Angular or create a post component.   We will focus on the animations involved.  

Angular Version: 9.1.0

----
****

## What will the animations look like?

This YouTube video shows the animations you will see when you run the project:

<a href="https://youtu.be/j_7ug2WHK4w"><img src="/images/blog/animations/animations.png" width="200px" target="_blank"></a>

## Project set-up

In the project there is an `animations.ts` file where all of the animation configurations are stored.  

The project uses Angular Routing because we will go over how to animate a route. 

There are four components:
- app.component
- post.component
- add-post.component
- other.component

`app.component.html` simply holds the router outlet.  The default page is `post.component.html`, which also contains the `add-post-component` which we will slide in.  

`other.component` is another component to hold some examples and another component to route to for our routing example.

## Add Post slide-in animation example

In the `animations.ts` file this animation makes the add-post slide in:

```typescript
export const slideDownAnimation = trigger('slideDownAnimation', [
  state('in', style({
    overflow: 'hidden',
    maxHeight: '300px'
  })),
  state('out', style({
    overflow: 'hidden',
    maxHeight: '0px'
  })),
  transition('out => in', animate('800ms ease-in-out')),
  transition('in => out', animate('400ms ease-in-out'))
]);
```

So, what are we doing here?  We identify two states that define the styles associated with the 'out' state, when the add-post is not shown, and the 'in' state, when the post is shown.  The only difference is the maxHeight.  maxHeight is set to zero which causes it not to show when in the 'out' state and `300px` for hte 'in' state, causing it to show. 

We use `transition` to tell Angular what to do when transitioning from 'out' to 'in' and from 'in' to 'out'.  When transitioning from 'out' to 'in'
## Thumbnail photo expansion animation example

## Draw attention to an element animation example

## Add-to-list animation example

## Route animation

## Conclusion

Hopefully, by using real-world, practical examples, you can get an idea of how powerful and useful Angular animations are and how you might apply it in your own project!

As a reminder, you can find a fully working project on my [GitHub](https://github.com/DaveStaudenmaier/animations).

Feel free to contact me at [dave@dev-reboot.com](mailto:dave@dev-reboot.com) if you have any questions or comments. 

Keep on developing!

