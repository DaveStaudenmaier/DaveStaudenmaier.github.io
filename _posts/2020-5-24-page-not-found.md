---
layout: post
title: Add a 'Page Not Found' to your Angular 2+ routes
---

Learn how to add a Page Not Found component you can redirect users to for invalid routes, and then automatically redirect them to your home page after several seconds.

Angular Version: 9

----
****

Let's create a new Angular project using the Angular CLI.   On your command line create a new project like this

```
ng new page-not-found
```

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

```typescript
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

```typescript
<router-outlet></router-outlet>
```

Great!  Go head and run the app using ng serve and you should see the home page like this:

![home page](../images/home-page.png)

Here is a [stackblitz](https://stackblitz.com/edit/angular-ivy-medwvu) with the all the code
