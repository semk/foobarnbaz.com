---
title: "Pagination, Tags and more in Voldemort"
date: '06-04-2012'
time: '00:25'
tags: ['Python', 'Voldemort', 'Projects']
layout: 'post.html'
---

![Voldemort](/images/posts/2012-04-06-pagination-tags-and-more-in-voldemort/term.png)

I have been adding a few useul features to [Voldemort](https://github.com/semk/voldemort) for the last few months and it did come out pretty good. It now sports automatic Atom Feed and sitemap generation, thanks to the contibutions from my friend [Stanislav Yudin](http://endlessinsomnia.com). Here are the highlights.

* Generates Atom Feed automatically for your blog
* Generates sitemap.xml
* Pagination supported. Just add `paginate: True` in the page metadata section.
* You can tag posts using metadata. Eg: `tags: ['Python', 'Tips']`. A special template file named `tag.html` is used to generate tags on the fly.

You can see these changes live in this blog. Try clicking any of the tags below to see it yourself.


