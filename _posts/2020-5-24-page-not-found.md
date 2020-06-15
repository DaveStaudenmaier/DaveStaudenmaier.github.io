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
The best way to add routing to an Angular application is when
```typescript
const routes: Routes = [
  { path: '', redirectTo: 'home', pathMatch: 'full' },
  { path: 'home', component: HomeComponent },
  { path: 'other', component: OtherComponent },
  { path: '**', component: PageNotFoundComponent }
]
```

Here is a [stackblitz](https://stackblitz.com/edit/angular-ivy-medwvu) with the all the code
