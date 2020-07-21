---
layout: post
title: Different ways to pass data between components in Angular
---

In this blog, I will show you three ways to pass data between components in Angular using a real-world example.

As usual, you can find a fully working project on my [GitHub](https://github.com/DaveStaudenmaier/AngularShareData)

Angular Version: 9.1

----
****

## Conclusion

We looked at three different ways to pass data between components in Angular including:
- Parent to child using @Input
- Sibling using Query Parameters
- Sibling using a service

I wanted to focus on these three, but you can also pass data from child to parent using @Output and call methods and pass data to a child using @ViewChild.

Also, for a large application, using a service can get cumbersome.  This is when you want to check out Angular's NGRX state management system, which I will save for another blog.

Feel free to contact me at [dave@dev-reboot.com](mailto:dave@dev-reboot.com) if you have any questions or comments!

Keep on developing!
