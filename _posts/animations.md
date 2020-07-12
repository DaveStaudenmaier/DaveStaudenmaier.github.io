---
layout: post
title: Practical, real-world animation examples with Angular
---

In this blog, I will who show some examples of practical, real-world animations using Angular, and will explain in detail how they work.   Here are the animations we will cover: 
- A slide-down animation exposing an Add Post component's template
- A fade in for the new post applied
- A thumbnail photo that will expand smoothly to full-size when clicked and shrink back down when clicked again
- An animation that will draw attention to an element
- Two variations of animation when adding to a dynamic list.  One will fade in.  The other will slide in from the left. 
- Fade animation when changing routes, configured in one place

As usual, you can find a fully working project on my [GitHub](https://github.com/DaveStaudenmaier/animations).

This is a **LOT** to cover in one blog post, but I wanted you to have it all in one place in case you want to incorporate any of my code in your code and have a single place to reference.  For brevity, I will only be going through the animations code, not the various components code.   So, I recommend downloading the project from GitHub and running `npm install`.

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

In the project and on the video, you will see an animation that slides in an add-post template when the user clicks a button. 

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

The `state()` function allows you to collect a set of styles in an animation state, and give the state a name, such as 'open' and 'closed' or, in this case, 'in' and 'out'.  The `state()` function takes two arguments: a unique name and a `style()` function. You can define whatever styles you want in the object passed to the `style()` function. For our slide-in animation, all we want to do is expand the add-post template (we will get to this) to it's full height when the state is 'in' and not show anything when the state is 'out'.  Using `maxHeight` with `overflow` hidden accomplishes this. 

Note the use of camel case is required, so instead of `max-height` as you would use it in CSS, we use `maxHeight`.  Be sure to always use camel case.

The `transition()` function allows us to specify the animations to apply as we move from one state to another.  The transition() function accepts two arguments: the first argument accepts an expression that defines the direction between two transition states, and the second argument accepts one or a series of animation steps.  In our case, we move from 'out', where the maxHeight of add-post is zero and thus not shown, to 'in', where the maxHeight is 300px, and so the element is visible.  Later we will see other types of transitions and the use of wildcard.  

The `animate()` function is where the magic happens.  Use the `animate()` function to define the length, delay, and easing of a transition, and to designate the style function for defining styles while transitions are taking place. The animate() function accepts the timings and styles input parameters.  In our example, because our styles are defined in the `state()` function, we only need to define the timings. 

The timings parameter takes a string defined in three parts: animate ('duration delay easing').

In our example, we do not have a delay.  We want the slide-in (transition from 'out' to 'in') to occur over 800 milliseconds.  We could have expressed this as `.8s` or just `800` as well.   The third argument, easing, controls how the animation accelerates and decelerates during its runtime. For example, ease-in causes the animation to begin slowly, and to pick up speed as it progresses.   You can find some good advice on easing in [Material Design documentation](https://material.io/design/motion/speed.html#controlling-speed).   

Valid values for easing are `ease`, `ease-in`, `ease-out`, `ease-in-out`, or a `cubic-bezier() function call`.  Here is a nice [blog](https://css-tricks.com/ease-out-in-ease-in-out/) by Chris Coyier on these easing options.   The one I chose, `ease-in-out` is slow at the beginning and the end and faster in the middle. 

In our example, I wanted the slide-out of our element to happen faster, so I chose a duration of `400ms`.  

Finally, you will see all of this is wrapped up in the `trigger()` function. An animation requires a trigger, so that it knows when to start. The trigger() function collects the states and transitions, and gives the animation a name, so that you can attach it to the triggering element in the HTML template.

So, how do we use this animation?   

In `post.component.ts` I import `slideDownAnimation` from `animations.ts` and add it to the `animations` array in the `@Component` decorator. 

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

In `post.component.html` I tie a container element for the HTML we wish to animate to the trigger name prefixed with '@' and wrapped in square brackets. Then, you can bind the trigger to a template expression using standard Angular property binding syntax.   In our example we tie it to a property called `addPostOpen` which is defined in `post.component.ts` as follows:

```typescript
  addPostOpen = 'out';
```

And so, you can see that the default is our 'out' state where you remember `maxHeight` is zero and so it is hidden.   All we have to do to activate our animation to show the add-post template, is set the `addPostOpen` property to 'in'!   I simply created a button a user can click to add a post and then set the `addPostOpen` property to 'in' which animated the showing of my `app-add-post` component's template.  

```html
  <div [@slideDownAnimation]="addPostOpen">
    <app-add-post (post)="postAddComplete($event)">
    </app-add-post>
  </div>
```
## Fade in animation on new post

In addition to sliding add-post in and out, when I show the new post back in the post component, I want to slowing fade it in instead of abruptly showing it.  

Here is the animation in the `animations.ts` file:

```typescript
export const addItemFadeAnimation = trigger('addItemFadeAnimation', [
  transition('* <=> *', [
    query(':enter',
      [style({ opacity: 0 }), animate('3000ms ease-out', style({ opacity: 1 }))],
      { optional: true }
    ),
    query(':leave',
      animate('3000ms', style({ opacity: 0 })),
      { optional: true }
    )
  ])
]);
```

Here we start again with a trigger named 'addItemFadeAnimation'.  But in this example we start with transition.  Remember, the transition() function accepts two arguments: the first argument accepts an expression that defines the direction between two transition states, and the second argument accepts one or a series of animation steps.

In this example, when we change our triggered value in any way, we want the animation to trigger.  We define this using wildcard `*`.  If the value changes from anything to anything, apply the animation.

Let's look at how we apply this trigger first.   In `post.component.html` I wrapped an `*ngFor` directive with our animation trigger.  If the posts array length changes (i.e. we add a new element to the array), then apply the animation to the new element.  

```html
<div [@addItemFadeAnimation]="posts.length">
  <div *ngFor="let post of posts">
    <div class="author" >
      <div>
        <img class="image profile-image-post"
              [src]="profileImageUrl"
              style="width:40px;height:40px;">
      </div>
      <div class="name-and-date">
        <div class="display-name">
          Dave
        </div>
        <div>
          {{ postDate | date:'short'}}
        </div>
      </div>
    </div>
    <div class="body">
      <div>
        <span class="body-text">{{ post }}</span>
      </div>
    </div>
    <mat-divider></mat-divider>
  </div>
</div>
```

Back to the animation.  We are using the `query()` function, which is typically used to find an element in the HTML.  The `query()` function takes three arguments: (selector, animation, options).  The selector identifies the element to query, or a set of elements that contain Angular-specific characteristics

In our case we are setting the selector to the alias `:enter` which really means going from a state of `void` to anything (`void => *`) and `:leave` which is an alias for `* => void`.   I am not deleting posts in this example, so `:leave` will not be used, but I wanted to leave it for you to see what you could do.  For more information on 'Angular-specific characteristics' see the [Angular documentation](https://angular.io/api/animations/query). 

```html
query(':enter',
  [style({ opacity: 0 }), animate('3000ms ease-out', style({ opacity: 1 }))],
  { optional: true }
),
```
Next we apply the style using the `style()` function, animating from an opacity of zero (invisible) to 1 over 3 seconds.  

## Thumbnail photo expansion animation example

## Draw attention to an element animation example

## Add-to-list animation example

## Route animation

## Conclusion

Hopefully, by using real-world, practical examples, you can get an idea of how powerful and useful Angular animations are and how you might apply it in your own project!

As a reminder, you can find a fully working project on my [GitHub](https://github.com/DaveStaudenmaier/animations).

Feel free to contact me at [dave@dev-reboot.com](mailto:dave@dev-reboot.com) if you have any questions or comments. 

Keep on developing!

