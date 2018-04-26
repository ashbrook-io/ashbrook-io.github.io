---
layout: post
published: true
title: Listing Documents via Jekyll without collections
image: 'https://png.icons8.com/ios/1600/easy.png'
subtitle: >-
  How to avoid configuration files for document meta-data? Just use the file
  names!
---
Recently, I moved all of our static sites over to hosting on github using [github pages](https://pages.github.com/). Github uses [jekyll](https://jekyllrb.com/docs/home/) for generation of the static content and there are tons of themes out there to utilize.

You can also just publish straight up html files and it will work the same way. The ins and outs of all of this is better left to the links above.

This worked really well for us, however we had one issue where we needed to publish lists of pdf documents, but I wanted to have the lists generated automatically. These documents had some meta data, but I didn't want to have to maintain a [collection](https://jekyllrb.com/docs/collections/) to be updated or use some other data structure. The goal was for me to be able to hand off the 'updating' of the site to someone else and make it very simple. So I wanted to just use the file names to hold the metadata. I decided on a format something like `DATE-TITLE-TYPE.pdf` for my first process. And to keep the different documents for different pages away from each other, I put them into different sub folders in assets.

So in a simple example, my site structure looked like this:
```
/
--/assets
----/page1
------20180101-Some Document Title-t0.pdf
------20180102-Some Document Title-t0.pdf
------20180104-Some Document Title-t1.pdf
page1.md
```

And then on page1.md, to show these files, I did something like this:

```
---
title: page1
---

# {{page.title}}

{% assign fs = site.static_files | where_exp: "f","f.path contains 'page1'" %}
{% for f in fs %}
{% assign g = f.basename | split: '-' %}
[{{g[1]}}]({{ f.path }})
{% endfor %}

```

You can also filter based on the t0/1 document type if you want to assign with another filter.

In my case, I did get quite detailed and had a few different structures for each of the items. You can definitely make a case *not* to do this, but the goal was to make this easy for other people to add content and in all of our particular use cases, we are just adding items to show up in a list. Maybe a couple of items a quarter or so. So this way I could make the instructions something like 'name the file this, then copy it here' and that's it. =)

![easy](https://static1.squarespace.com/static/564e0683e4b094eaa9d90b4b/t/58ee95a9f7e0abff4db166fb/1492030892939/)