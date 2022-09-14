---
layout: ../../layouts/BlogPost.astro
title: "Ilograph"
author: "Cam Beatty"
tags:
- Diagrams
- Architecture 
pubDate: 'August 30 2022'
heroImage: "/placeholder-hero.jpg"
---

As a solution architect, I tend to have to build process flows and diagrams depicting systems and how they interact with each other. I have tried a few different solutions that are available out there with limited success. I find that my biggest issue with existing solutions is they allow too much customization. I get bogged down in the minor details of how these visualizations look and quickly get stuck in a cycle of moving elements around to make everything look pretty. You may not suffer from this same issue, and if that is the case, then you can happily continue using other tools like [Visio](https://www.microsoft.com/en-us/microsoft-365/visio/flowchart-software), [Lucid](https://lucid.co/), or [Draw.io which was rebranded as diagrams.net](https://app.diagrams.net/) to name a few. If you struggle with a similar issue with those platforms, I would recommend [Ilograph](https://www.ilograph.com/). 

### What is Ilograph
Ilograph headline is “Interactive System Architecture Diagrams For Web and Desktop”. I think that is a pretty good explanation, but of course, there is more than just that. I think the [docs](https://www.ilograph.com/docs/getting-started/what-is-ilograph/) they provide explain in much greater detail all the possibilities that they have to offer, but I’ll highlight the things I have found to be most enjoyable.

### Diagram as code
The way the author defines all pieces of the diagram is by writing yaml. I know there are some opposing thoughts on yaml, but in my opinion, defining everything this way has multiple benefits that outway the yaml annoyance. By writing code, you can easily store the definition with the project you are diagramming and check it into git. That can maintain your version history or you can use the built-in version history, whatever is easiest for you. Also, this code doesn’t offer a large number of options for where to place items in the graph which makes the concept of trying to make everything look pretty not an option. This limitation is one of the biggest reasons I have found so much enjoyment in this product. One thing I could see being added in the future would be a definition for positioning, but I would assume it would be opt-in.

### Multi-Level Resources
Resources are the definition of “what” the author is describing. These resources only need to be defined once and can be used for multiple perspectives. Perspectives are different views of the defined resource, these can describe the interactions differently for the different audiences of the diagram. One of the properties on a resource is “children”. This "children" property allows the author to define a sub-resource that is nested inside the parent. Where I have found this to be useful, is the author can have multiple levels of understanding. For instance, I can define the top-level systems and how they interact and within the same diagram, I can dig into one of those systems and define a finer grain of detail without having to copy elements or create a whole new diagram. 

### Walkthroughs
One of the biggest eye-catching pieces is the interactive abilities. Ilograph allows you to define walkthroughs, which helps the consumer of the diagram understand what the author was trying to depict with only words. With these walkthroughs, the author can do more than add text, they can decide to select, highlight or expand a resource. What this does in practice is show interactions between multiple resources, or bring a single resource to the consumer's attention. I have heard some people I have shown these to as a “beefed up PowerPoint.” That’s a good analogy as it adds animations and movement to the diagram, and it’s a lot easier to do. Also, these walkthroughs can have additional text on the side to help explain in more detail what the animations are depicting.

### Give it a try
If any of this sounds exciting or intriguing, I think you should give it a try. The best way I was able to understand its true power was to migrate one of our existing diagrams. There was a slight learning curve as you will have to write code. I found by looking at an example diagram, helped me migrate and play around with the walkthroughs and animations. With how positive this post is, you may think I have some conflict of interest, but I assure you, I have only found this product and really enjoy it. I hope you all give it a try and find as much enjoyment as I have!