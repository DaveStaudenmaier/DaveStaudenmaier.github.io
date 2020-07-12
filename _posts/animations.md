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

For this blog I am using several components to illustrate animations.   For brevity, I will only be going through the animations code, not the rest.   So, I recommend downloading the project from GitHub and running `npm install`.

Angular Version: 9.1.0

----
****

## What will the animations look like?

This YouTube video shows the animations you will see when you run the project:

<a href="https://youtu.be/j_7ug2WHK4w"><img src="/images/blog/animations/animations.png" width="200px" target="_blank"></a>

## Project description

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

So, what are we doing here?  

The `state()` function allows you to collect a set of styles in an animation state, and give the state a name, such as 'open' and 'closed' or, in this case, 'in' and 'out'.  The `state()` function takes two arguments: a unique name and a `style()` function. You can define whatever styles you want in the object passed to the `style()` function. For our slide-in animation, all we want to do is expand the identfied element (we will get to this) to it's full height when the state is 'in' and not show anything when the state is 'out'.  Using `maxHeight` with `overflow` hidden accomplishes this. 

Note the use of camel case is required, so instead of `max-height` as you would use it in CSS, we use `maxHeight`.  Be sure to always use camel case.

The `transition()` function allows us to specify the animations to apply as we move from one state to another.  The transition() function accepts two arguments: the first argument accepts an expression that defines the direction between two transition states, and the second argument accepts one or a series of `animate()` steps.  In our case, we move from 'out', where the maxHeight of the element is zero and thus not shown, to 'in', where the maxHeight is 300px, and so the element is visible.  Later we will see other types of transitions and the use of wildcard.  

The `animate()` function is where the magic happens.  Use the `animate()` function to define the length, delay, and easing of a transition, and to designate the style function for defining styles while transitions are taking place. The animate() function accepts the timings and styles input parameters.  In our example, because our styles are defined in the `state()` function, we only need to define the timings. 

The timings parameter takes a string defined in three parts: animate ('duration delay easing').

In our example, we do not have a delay.  We want the slide-in (transition from 'out' to 'in') to occur over 800 milliseconds.  We could have expressed this as `.8s` or just `800` as well.   The third argument, easing, controls how the animation accelerates and decelerates during its runtime. For example, ease-in causes the animation to begin slowly, and to pick up speed as it progresses.   You can find some good advice on easing in [Material Design documentation](https://material.io/design/motion/speed.html#controlling-speed).   

Valid values for easing are `ease`, `ease-in`, `ease-out`, `ease-in-out`, or a `cubic-bezier() function call`.  Here is a nice [blog](https://css-tricks.com/ease-out-in-ease-in-out/) by Chris Coyier on these easing options.   The one I chose, `ease-in-out` is slow at the beginning and the end and faster in the middle. 

In our example, I wanted the slide-out of our element to happen faster, so I chose a duration of `400ms`.  

Now, how do we use this animation?   

In the `post.component.ts` I import `slideDownAnimation` from `animations.ts` and add it to the `animations` array in the `@Component` decorator. 

```typescript
import { slideDownAnimation, addItemFadeAnimation } from './../animations';

@Component({
  selector: 'app-post',
  templateUrl: './post.component.html',
  styleUrls: ['./post.component.css'],
  animations: [
    slideDownAnimation,
    addItemFadeAnimation
  ]
})
```

## Fade in animation on new post

## Thumbnail photo expansion animation example

## Draw attention to an element animation example

## Add-to-list animation example

## Route animation

## Conclusion

Hopefully, by using real-world, practical examples, you can get an idea of how powerful and useful Angular animations are and how you might apply it in your own project!

As a reminder, you can find a fully working project on my [GitHub](https://github.com/DaveStaudenmaier/animations).

Feel free to contact me at [dave@dev-reboot.com](mailto:dave@dev-reboot.com) if you have any questions or comments. 

Keep on developing!

