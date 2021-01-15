---
title: 'Site Info'
date: '2020-08-22'
---
## Thanks for taking a look around

In this post, I'll be talking about some of the tools and techniques I employed to build this simple website.

---
### Appearance

Every webpage is composed of components from my library, [react-funk](https://www.npmjs.com/package/react-funk). I'll be discussing how I built that in a later post.


---
*DISCLAIMER* : [react-funk](https://www.npmjs.com/package/react-funk) is nowhere near production ready, so please use at your own risk.

---

### Infrastructure

* *Server* - This site is deployed on [Vercel](https://vercel.com/). It's a great platform for personal accounts as it provides users with a small CI/CD platform linked directly to your Github repository. I highly recommend looking into it, especially if you plan on using NextJS to build your application. Setting up a custom server to properly serve Next apps is time consuming and usually not worth it. Oh one more thing, *it's completely free!*
* *Blog* - All my blog posts are stored in a static bucket in AWS. I'm not a huge fan of S3's API, but this is definitely a quick, cost effective solution.
* *API* - NextJS supports API routes out of the box. This was extremely helpful as it eliminated the need for a separate API server. Since I'm not managing a ton of data or doing complex server side operations, making api requests to the same server that hosts my static content is not a problem.

### Pages

NextJS is a SSR framework, so it would be foolish not to leverage some of the great benefits of static page generation and server side rendering. Here's a table that shows which techniques each route on my website uses.



| Method | / | /about | /portfolio | /blog | /posts/[id] |
| --- | --- | --- | --- | --- | --- |
| SSR | ❌ | ❌ | ❌ | ✅ | ✅ |
| Static Generation | ✅ | ✅ | ✅ | ❌ | ❌ |

The informational pages can be generated at build time as their data isn't volatile. However, blog posts are different. I built a lightweight CMS so I can quickly edit, delete, and publish new posts. Having to redeploy my website after modifying or creating a post would be annoying. Therefore, I chose to get posts at the time of each request. Currently I'm making remote calls to AWS every time. Since I don't expect the number/content of posts to change extremely frequently, the next item on my todo list is to implement a caching layer.

### Page Transitions

"Single Page Web Applications" are the craze these days. Assuming the application has a client-side routing solution, instead of making a remote call every time a user navigates to a new page, you can download the entire application bundle on the first load. This seems great at first glance because it means new pages are loaded lightning fast and you can start to do cool things like implement custom transitions between pages. However, a major problem is the initial load time. Since many apps nowadays rely on heavy frameworks like React, or AngularJS, downloading tons of Javascript can take a while. Downsides include poor SEO and a bad user experience for clients with a less than stellar network connection. Luckily, with code splitting, SSR, and static page generation, NextJS mitigates these effects. Using a custom webpack config, NextJS splits your bundle into chunks so it loads only what it needs to. Additionally, by default Next will prefetch chunks for any `Link`'s visible on the page.

### Blog Content Management System

I think writing is a great way to explore a question and gain control over it. I wanted a platform that would let me share my thoughts fast and with ease. Take a look:

![Blog CMS](https://i.ibb.co/YTk5r43/Screen-Shot-2020-08-22-at-5-48-54-PM.png)

Some notes:


* I used AWS's Javascript SDK to dispatch read/write requests to S3
* Each post is saved in AWS as a markdown file, and rendered in the browser with [react-markdown](https://www.npmjs.com/package/react-markdown)

This isn't a full-fledged CMS by any means, but not too bad for a day's worth of hacking 😁.

### Wrapping Up

Well, that's about it! Thanks for taking the time to read through this post. If I missed anything or you just want to reach out, please feel free to [contact me](mailto:rohan@rohankrishnaswamy.com?Subject=Hey%20Rohan%20(Site%20Inquiry)).
