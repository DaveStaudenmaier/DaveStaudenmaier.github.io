---
layout: post
title: Angular PWA Header/Footer/SideNav Navigation for Devices
---

In this blog, we will create from scratch a navigation scheme suitable for a progressive web app that includes a fixed footer and header as well as a slide-out side-nav.  

Angular Version: 9

<img src="/images/home1.jpg" height="300px">

You can find the complete code on my GitHub at [https://github.com/DaveStaudenmaier/AngularPWANavigationShell](https://github.com/DaveStaudenmaier/AngularPWANavigationShell).

----
****

Let's create a new Angular project using the Angular CLI.   On your command line create a new project like this:

**ng new pwa-nav**

We are using Angular Material so we will need a few things installed.  Go to the pwa-nav directory and run this:

**npm install @angular/material @angular/cdk @angular/animations**

Now add Material to the project like this: 

**ng add @angular/material**

Finally, we're going to need flex layout.  Add it like this:

**npm install @angular/flex-layout**

A couple more set-ups and we'll be ready to go.  in pwa-nav, add a directory called **features** and another new directory called **navigation**

In features, add the following four new components like this:

**ng generate component home**
**ng generate component forums**
**ng generate component messages**
**ng generate component about**

Just a few navigation components now.  Got to the navigation folder and add three components like this:

**ng generate component header-mobile**
**ng generate component footer-mobile**
**ng generate component sidenav**

Great!  We're ready to start adding code.   Go ahead and edit the project.

