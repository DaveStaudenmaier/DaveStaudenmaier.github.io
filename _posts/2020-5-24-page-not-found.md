---
layout: post
title: Handle 'Page Not Found' with Angular routing
---

Learn how to add a Page Not Found component you can redirect users to for invalid routes, and then automatically redirect them to your home page after several seconds.

Angular Version: 9

----
****

Let's create a new Angular project using the Angular CLI.   On your command line create a new project like this

**ng new page-not-found**

Angular will ask you if you want to add routing to your project.  Type y

```
? Would you like to add Angular routing? (y/N)
```

Angular will also ask you what stylesheet format you would like to use.   Select your favorite.

We already have an Angular application created with support for routing!  

![routing](../images/routing-set-up.png)

Now, let's create a couple of components that we can route to.  The Angular CLI can do that for us.   Go to your page-not-found project directory and add a component called 'home' and a component called 'other' like so

```
ng generate component home
ng generate component other
```

Great!  Now we have two components to route to.  Let's add a header in the 'home' component's template, replacing the default HTML, so it's easier to see where we are.  Do the same for the 'other' component as well.

```html
<h1>This is the home page</h1>
```

Now let's add the routes for these components!

In the app-routing.module.ts file add these routes the the Routes array created by the Angular CLI. The empty path will redirect to the home path.   Don't forget to import these components into the routing module as well!

```typescript
const routes: Routes = [
  {path: '', redirectTo: 'home', pathMatch: 'full'},
  {path: 'home', component: HomeComponent},
  {path: 'other', component: OtherComponent}
];
```

Next, we add the router outlet to app.component.ts.   Delete the default template the Angular CLI created and replace it with the router outlet.  The router-outlet tag acts as a placeholder that Angular dynamically fills with your content based on the current router state.

```html
<router-outlet></router-outlet>
```

Great!  Go head and run the app using ng serve and you should see the home page like this:

![home page](../images/home-page.png)

We're almost there.  Let's use routerLink to add a navigation link to 'other' on the 'home' component.  Then do the same from 'other' to 'home'

```html
<h1>This is the home page</h1>

<a routerLink='/other'>Go To Other Page</a>
```
Now, you should be able to toggle between the 'home' and 'other' pages.  Notice the URL changes to reflect the active route.  You can also change routes by entering the url.   http://localhost:4200/other and http://localhost:4200/home.  

So, what do we do if a enters a page that we do not have such as http://localhost:4200/page?   The user ends up on a blank page and that's not very friendly.   Let's handle this page-not-found situation.  

First, we add a new component we can route to when the path is invalid.   On your command line enter 

```
ng generate component page-not-found
```

In the page-not-found component's template, replace the default with this, explaining to the user that the page they are seeking was not found and that they will be redirected to the home page.

```html
<h1>Page Not Found</h1>
<p>Sorry, this page was not found</p>
<p>You will be redirected to the home page in a few seconds</p>
```

To handle the redirect, make the page-not-found.component.ts file look like this.  We use the ngAfterViewInit lifecycle hook to redirect to the home page after a 3 second delay, using the navigateByUrl method on Angular's Router class.  Note that 'this' when used in the setTimeout callback function refers to the function itself, not our page-not-found class.  Defining self within the ngAfterViewInit() function handles this situation for us. 

```typescript
import { Component, OnInit, AfterViewInit } from '@angular/core';
import { Router } from '@angular/router';

@Component({
  selector: 'app-page-not-found',
  templateUrl: './page-not-found.component.html',
  styleUrls: ['./page-not-found.component.css']
})
export class PageNotFoundComponent {

  constructor(private router: Router) { }

  ngAfterViewInit() {
    let self = this;

    setTimeout(function () {
      self.router.navigateByUrl('/home');
    }, 3000)
  }
}
```

The final step is to add a wildard(**) to the Routes array in the app-routing.module.ts. The router will select this route if the requested URL does not match any paths for the defined routes. 

```typescript
const routes: Routes = [
  {path: '', redirectTo: 'home', pathMatch: 'full'},
  {path: 'home', component: HomeComponent},
  {path: 'other', component: OtherComponent},
  {path: '**', component: PageNotFoundComponent}
];
```

And that's it!  Now if you type in a bad URL such as http://localhost:4200/bad, you will be redirected to the 'page-not-found' component which will explain to the user what happened and then redirect them to the 'home' component after 3 seconds. 

Here is a [stackblitz](https://stackblitz.com/edit/angular-ivy-medwvu) with the all the code so you can see it in action!

Keep on developing!
