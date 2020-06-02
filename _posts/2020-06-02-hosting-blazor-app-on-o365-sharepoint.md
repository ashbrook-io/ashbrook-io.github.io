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

### Enter Blazor and WebAssembly

I will probably always be more comfortable with batch/shell scripting as that's where I started, but after that most of my development work was in the microsoft world. I've done java and a variety of other weirder languages depending on where I was, but I never studied C in school or anything so my background is more higher level programming languages. My preference has always been the microsoft world from a tooling perspective, however I always really use vscode in recent years and mostly stick to things I can produce with that as far as solutions I produce go. Lots of powershell and javascript is really where I live today. That being said, i have been watching blazor in the last year and it looked pretty cool and the idea of using c# and producing javascript sounded nice. 

With the Build conference recently being virtual, I was able to get time to attend it and one of the things I was looking forward to was some info on this. Blazor was released and is now ready for production which is cool. So I grabbed the required things and played around with it a little bit. Easy enough to add some stuff into it, but then I decided to deal with my main concern/question which was can I just dump it onto sharepoint to host it there. I probably need to write an entry about how we use this and why it works for us, but for now I'll just say that there is one main caveat for hosting static stuff right on a sharepoint site: **json files aren't allowed**. The fix for this for me is I just produce json data and name it txt. Then I just parse it as json. No biggy. But for something like blazor, I figured some of the build steps may require json files or things to be in certain locations and I was right.

### How to make it work

So, first I'll say I did not go all the way through with getting this fully setup. But if you just start with `dotnet new blazorwasm -o MyApp` that will get you an app. Then you can `cd MyApp` and  `dotnet publish -c Release /p:TrimUnusedDependencies=true` to get some files you can push out as a static site. To get to these you can just `start .\bin\Release\netstandard2.1\publish\wwwroot\` if you are in that folder and you'll see the files you need to copy. You'll need to rename the html file to aspx for it to work. You will also need to change the base value in the html page to point to your actual base folder. For me I have this as a subsite in another site and I am using a publishing site which already has a Pages library. So I changed my base to something like `<base href="/mySite/mySubSite/Pages/" />` and that seemed to allow the webassembly js to load. However if you copy it all out just like that, it won't work. Since I have run into issues with json files in the past, I figured this was probably the case as well. Since the html has a single script file reference to `_framework/blazor.webassembly.js` I just started randomly debugging in there for what was going on. This was kind of impossible with the generated file so I used beautify to format it in vscode and then pushed it out there. In hindsight I could have just searched for `.json` since I kind of knew what I was looking for, but stepping through it was I guess interesting and I was hoping I would get some meaningful info. These are pretty _optimized_ functions though so it's not really easy to read. Anyway, down towards the bottom you can find `fetch("_framework/blazor.boot.json".....` which is really the culprit. Adding a suffix of `.txt` and adding `.txt` to the end of the actual file name and uploading it again seems to do the trick. This allowed the web app to load and I could see counter and the main page. I think the default route may need to be tweaked and there are other items that may need to be tweaked similarly, but it did work.

### Why I don't care


