---
layout: ../../layouts/BlogPost.astro
title: "SSR, is it needed?"
description: "Why is SSR so hot right now and should you care?"
author: "Cam Beatty"
tags:
- SPA
- Front End  
pubDate: 'September 15 2022'
heroImage: "/stock/header-3.jpg"
---
### Introduction
I have been working on expanding my knowledge outside of the .Net world, which first led me to the node stack, primarily because of the Youtube algorithm. The node stack is valuable from the web development perspective since both the front and back end can be written in javascript or typescript. This allows for the natural progression of rendering existing spa frameworks on the back end and sending the result to the user where the SPA(single page application) framework eventually takes over, otherwise known as Server Side Rendering (SSR). Does rendering the HTML on the server sound familiar? 

### Cyclical Patterns
Starting in the .Net ecosystem in 2015, MVC (Model-View-Controller) was the main pattern I used when developing applications for the web. I even ran across a few Web Forms projects; however, I quickly learned I was not a fan of those. The industry kept moving forward and realized that the experience on the client side was acceptable but a nightmare to maintain. This led to the first SPA framework came to be, AngularJS. AngularJS moved the MVC model from the back end to the front end, a reasonable first step for the SPA movement. A few years later, ReactJS came out and gained a large market share. New SPA frameworks continue to be developed and released. One complaint that has come up from the beginning has been the time from the first page request to the app becoming useful. SPA frameworks have come up with a few different ways to combat this. Splitting the app into small chunks and tree shaking were some of the first implementations to reduce the amount of javascript sent in the first request. This did reduce the time to make the web app responsive but left users with initial loading pages. How did the tech community solve the loading pages? Render the initial state of the application on the backend and send it to the front end where the SPA framework can take over and make it interactive. MVC worked similarly by rendering the initial state of the app on the backend and sending it to the user where javascript was added to increase the functionality. One cycle.

### SSR Pros
- No additional wait once the response is sent from the web server
- Great for search engine optimization (SEO)
- Could simplify application architecture

### SSR Cons
- Additional development if the app is already working
- Increased wait time till the first byte
- It may require additional infrastructure to host
- Depending on how the SPA framework is handling route transitions, the app can feel slower

### Which would I choose?
As always, it depends. For existing applications, I would keep moving forward with what you currently have unless SEO is a large concern. For new applications, if your web application is behind a login screen or is an internal line of business application, I would skip the extra work. So what does that leave for using SSR? A public web app or a site with SEO as an essential piece, you should walk down the SSR path. If your site has limited interactivity, you should look into static site generation (SSG), as that will give you great SEO without all the overhead of a SPA framework. SSR should be used when appropriate, but be cautious of its drawbacks.
