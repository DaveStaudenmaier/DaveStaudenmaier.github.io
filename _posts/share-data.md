---
layout: post
title: Different ways to pass data between components in Angular
---

In this blog, I will show you three ways to pass data between components in Angular using a real-world example.

As usual, you can find a fully working project on my [GitHub](https://github.com/DaveStaudenmaier/AngularShareData)

Angular Version: 9.1

----
****

## Introduction

To use Angular effectively it is critical to understand the many ways to pass data between components.   In this blog, I will discuss three of them.
- Parent to child using @Input
- Using Query Parameters to pass data between unrelated components
- Using a service to pass data between unrelated components

The example application in [GitHub](https://github.com/DaveStaudenmaier/AngularShareData) is a fictional national parks web app where you can filter the national parks list by state.  The home page has a search filter and a hook to a child component that shows the national parks.  Clicking on a national park will bring you to the detail about the park.  

So, we need to pass the search filter from the parent component to the child component and then pass the selected national park to the details component.

## Parent to child using @Input
Using @Input is the easiest way to pass data between from a parent component to a child component.  In the example from GitHub, the parent component is a search filter applied to the data shown below from a child component.   So, we need to pass the search text to the child component. 

<img src="/images/blog/share-data/search.png" height="500px">

We use property binding to bind the value of the search input box in the child component to the search property in the child component, where we can use it to filter our array of national parks.

<img src="/images/blog/share-data/Input.png" height="500px">

You must import Input like this 

*** typescript
import { Input } from '@angular/core';
***

## Sibling using Query Parameters

## Sibling using a service

## Conclusion

We looked at three different ways to pass data between components in Angular including:
- Parent to child using @Input
- Using Query Parameters to pass data between unrelated components
- Using a service to pass data between unrelated components

I wanted to focus on these three, but you can also pass data from child to parent using @Output and call methods and pass data to a child using @ViewChild.

Also, for a large application, using a service can get cumbersome.  This is when you want to check out Angular's NGRX state management system, which I will save for another blog.

Feel free to contact me at [dave@dev-reboot.com](mailto:dave@dev-reboot.com) if you have any questions or comments!

Keep on developing!
