---
layout: post
title: Add a 'Page Not Found' to your Angular 2+ routes
---

----
****

Learn how to add a Page Not Found component you can redirect users two for invalid routes and then automatically redirect them to your home page after several seconds.

```typescript
const routes: Routes = [
  { path: '', redirectTo: 'home', pathMatch: 'full' },
  { path: 'home', component: HomeComponent },
  { path: 'other', component: OtherComponent },
  { path: '**', component: PageNotFoundComponent }
]
```

Here is a [stackblitz](https://stackblitz.com/edit/angular-ivy-medwvu) with the all the code
