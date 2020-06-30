---
layout: post
published: false
title: Migrate users from ASP.NET Forms-based Auth to ASP.NET Core Identity
---
Do you have a very old ASP forms based app? Do you want to run `dotnet new webapp` and migrate your pages, but you don't know how you'll simply move the users over to the new world? Well, let me give you a solution *if* you are moving over to ASP.Net Core Identity. If you don't know what that is, think about it like new forms auth I suppose.

We're going from [this](https://support.microsoft.com/en-us/help/308157/how-to-implement-forms-based-authentication-in-your-asp-net-applicatio) to [this](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/identity?view=aspnetcore-3.1&tabs=netcore-cli).

At least that's what I'm going to cover. Basically the idea of having a brand new site, but I add all of my existing users to the backend. No re-registration, no changes for the users if you can change the app behavior to match. Just easy.

You can check the second link up above to see what the end table looks like, but for now I'm going to just talk about the rest of this situation like you have an old app and plain text username/password pairs.


I have some very internal applications we are looking to modernize. They have some users. I think they were maybe written in asp.net 1.1 or so based on some of the artifacts in them, but there are no project/solution files and everything has been somewhat lost to time. Fortunately they are fairly straightforward applications, so redoing some of the pages will not be terribly inconvenient, but one of the roadblocks to just doing a mostly straight conversion (meaning, rewriting the pages) in a new framework was the security.

Now one option is to just re-register users in a new framework or do the new 'auth with x' setup where you just add some providers in. I had one very tiny app that I really just wanted to copy/paste and have it be pretty much transparent to the users. Styling could be done, logic for the pages was simple enough (basically some tables with filters, kind of like webified reports) but there was not a great communication channel with the users.

So lots of options for moving the world forward for these folks, but if I could just swap the app and it looked and operated the same, that would be great. So how can I setup their security. I was hoping there was a nice import or something, but sadly this security was old. While I'm not exactly sure how old, I'm guessing around ASP 1.1 or maybe 2.0 and I'm thinking [this article](How To Implement Forms-Based Authentication in Your ASP.NET Application by Using Visual Basic .NET) probably has a guide for how it was used back then. I remember writing apps that used this, but so long ago. Anyway, we want to move to something more like [this article](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/identity?view=aspnetcore-3.1&tabs=visual-studio) and cut the app over to the latest dotnetcore which is 3.1 as of when I write this.

So I need a way to get the users setup in a new app. I am going to skip a lot of the trial and error and just cut to what I did to solve this. Before I do that though, I'll say my biggest 'challenge' for this was the fact that the passwordhasher for the identity requires you to pass in a user. I didn't want a user and didn't really want to deal with all of that, so I wrote a hasher using the same basis from identity since it is available on github.

First of all I spun up a new app and got it setup where i could register a user just like [this article](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/identity?view=aspnetcore-3.1&tabs=netcore-cli). So a vanilla app using asp.net identity. I used a blank sql database instead of localdb by simply changing the connection string and did the rest of the steps as vanilla. This gives registration and confirmation pages, and all of the good login stuff. 

Enter text in [Markdown](http://daringfireball.net/projects/markdown/). Use the toolbar above, or click the **?** button for formatting help.
