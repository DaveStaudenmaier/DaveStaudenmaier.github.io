---
layout: post
title: Angular PWA Header/Footer/SideNav Navigation for Devices
---

In this blog, we will create from scratch a navigation scheme suitable for a progressive web app that includes a fixed footer and header as well as a slide-out side-nav.  

Angular Version: 9

<img src="/images/home1.jpg" height="400px">

You can find the complete code on my GitHub at [https://github.com/DaveStaudenmaier/AngularPWANavigationShell](https://github.com/DaveStaudenmaier/AngularPWANavigationShell).

----
****

Let's create a new Angular project using the Angular CLI.   On your command line create a new project like this:

**`ng new pwa-nav`**

Angular will ask you if you want to add routing to your project.  Type y.  Then select the default stylesheet. 

**`? Would you like to add Angular routing? (y/N)`**

We are using Angular Material so we will need a few things installed.  Go to the pwa-nav directory and run this:

**`npm install @angular/material @angular/cdk @angular/animations`**

Now add Material to the project like this: 

**`ng add @angular/material`**

Finally, we're going to need flex layout.  Add it like this:

**`npm install @angular/flex-layout`**

A couple more set-ups and we'll be ready to go.  in pwa-nav, add a directory called **features** and another new directory called **navigation**

In features, add the following four new components like this:

**`ng generate component home`**

**`ng generate component forums`**

**`ng generate component messages`**

**`ng generate component about`**

Just a few navigation components now.  Got to the navigation folder and add three components like this:

**`ng generate component header-mobile`**

**`ng generate component footer-mobile`**

**`ng generate component sidenav`**

Great!  We're ready to start adding code.   Go ahead and edit the project.

Go to app.component.html and add the following below. `<mat-sidenav>` is an angular directive that is used to create a side navigation bar and main content panel with material design styling and animation capabilities.  `<mat-sidenav-container>` is the main container, `<mat-sidenav>` is the side panel and `<mat-sidenav-content>` is the content panel.

`<app-sidenav>` is the selector for the sidenav component we created in the navigation folder. `<app-header-mobile>` is the selector for the header-mobile component we created and '<footer-mobile>` is the selector for the footer component we created.

```html
<mat-sidenav-container fullscreen>

  <mat-sidenav #sidenav role="navigation" mode="push">
    <app-sidenav (sideNavClosed)="sidenav.close()"></app-sidenav>
  </mat-sidenav>

  <mat-sidenav-content fxLayout="column">

    <div class="header">
      <app-header-mobile (sidenavToggle)="sidenav.toggle()" fxHide fxShow.lt-sm>
      </app-header-mobile>
    </div>

    <main class="container mat-app-background">
      <router-outlet></router-outlet>
    </main>

    <app-footer-mobile (sidenavToggle)="sidenav.toggle()"  fxHide fxShow.lt-sm>
    </app-footer-mobile>

  </mat-sidenav-content>

</mat-sidenav-container>
```

```css
mat-sidenav-container, mat-sidenav-content, mat-sidenav {
  height: 100vh; // Component content fills entire verticle space
}

mat-sidenav {
  width: 40vw; // Width sidenav takes up when open
}

.header {
  margin-bottom: 8px; // Leave space at bottom of toolbar so elevation shadow effect will show
}

// Component content area
.container {
  height: 100vh; // Component content fills entire verticle space
  overflow: auto; // Keeps header from scrolling away
  padding: 10px; // Component conent padding around edges
  margin-bottom: 50px; // Matches height of footer so can see all content when scrolling to bottom
}
```
