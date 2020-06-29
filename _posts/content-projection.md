---
layout: post
title: Use ng-content (content projection), ng-template, ngTemplateOutlet and ng-container for a reusable component in a real-world use case with nice example code
---

In this blog, I will define a use case where it makes sense to employ content projection using ng-content as well as ng-template with ngTemplateOutlet and ng-container.   I will explain what these directives are and why we are using them.  

As usual, you can find a fully working project on my [GitHub](https://github.com/DaveStaudenmaier/content-projection)

Angular Version: 9.1

----
****

## Our requirements (use case)

As part of a web application for RV users, we have been given these requirements to create a reusable card component that will assure a consistent look-and-feel and branding for cards used throughout the application:
- Card must use our logo icon

<img src="/images/blog/content-projection/logo.png" width="80px">

- All cards must have the title "RV Like Me" with a consistent look and feel that is not configurable by the user of the reusable component.
- For phase one, the component should accept a card type of 'lifestyle' or 'rig' and this should be displayed nicely as a subtitle. 
- The next area of the card should be an area for a picture, or multiple pictures, or really anything the user of the reusable component wants to put there.  
- Next, should be a text area the user of the reusable component could fill with something like location or other meta information. For flexibility, the may configure the content and look-and-feel of this area.
- The next area should be a text area that the user of the reusable component may fill with something like a description.  For flexibility, the may configure the content and look-and-feel of this area.
- There are two buttons under the description, LIKE and SHARE that are not configurable and should always have the same look and location on the card.

Here is an example of a card:

<img src="/images/blog/content-projection/example.png" height="300px">

## What is content projection and how can it help with our requirements?

Based on these requirements, it makes sense to accept the card type as an @Input from the parent component using our reusable component.   But the rest of the requirements give much flexibility to the user of our component to use something like @Input.  Instead we will allow the user to 'project' their HTML content into our component!


## What is ng-template and ng-templateOutlet and why are they useful for our use case?

## What is ng-container and how can that help us?


