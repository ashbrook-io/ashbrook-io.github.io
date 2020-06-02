---
layout: post
published: false
title: Hosting Blazor App on O365 Sharepoint
date: '2020-06-02'
subtitle: I said what I said
image: >-
  https://lh3.googleusercontent.com/proxy/EGIpudeeOqF_vjh8HwKjVpAoKKWlg1TjNbvxwGR171md71erZ4ENWt7NGoTZetSB9iIWOiQZV-7TvqgYLNtW0vVdnpYkT96N5sFfbMdcFMNuph1ee4lIyZypGa9M4g
---
> I probably have a larger article to write on hosting static sites on o365, especially with respect to SPA style things. But for now I'll just make a note of this particular item here so I don't forget or if anyone else is looking for this, they can find it. TLDR; don't do this, at least right now.

### A little background

Currently I have a SPA hosted on o365. It is just using html, custom js, bootstrap, and chartjs to show some businesses info internally. I started with a free bootstrap dashboard template. Originally a 'dashboard' solution, although it really is now a collection of graphs and tables that probably provide a bit more detail than a true KPI driven dashboard with some single number metrics. It works fine. There are requests to add items to it, however, from time to time that require me to do more extensive changes than I may want to the existing page. This has led me several times to consider converting it to react or using vue or some other library to make it a bit more standard. I am the only one working on it and it's not terribly complicated so the 'conversion' may have minimal practical pay off. Be that as it may, I do try to look at options when a change comes up and it makes sense.

**Enter Blazor and WebAssembly**

I will probably always be more comfortable with batch/shell scripting as that's where I started, but after that most of my development work was in the microsoft world. I've done java and a variety of other weirder languages depending on where I was, but I never studied C in school or anything so my background is more higher level programming languages. My preference has always been the microsoft world from a tooling perspective, however I always really use vscode in recent years and mostly stick to things I can produce with that as far as solutions I produce go. Lots of powershell and javascript is really where I live today. That being said, i have been watching blazor in the last year and it looked pretty cool and the idea of using c# and producing javascript sounded nice. 


