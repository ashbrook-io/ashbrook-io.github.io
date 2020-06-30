---
layout: post
published: false
title: Migrate users from ASP.NET Forms-based Auth to ASP.NET Core Identity
---
Do you have a very old ASP forms based app? Do you want to run `dotnet new webapp` and migrate your pages, but you don't know how you'll simply move the users over to the new world? Well, let me give you a solution *if* you are moving over to ASP.Net Core Identity. If you don't know what that is, think about it like new forms auth I suppose.

We're going from [this](https://support.microsoft.com/en-us/help/308157/how-to-implement-forms-based-authentication-in-your-asp-net-applicatio) to [this](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/identity?view=aspnetcore-3.1&tabs=netcore-cli).

At least that's what I'm going to cover. Basically the idea of having a brand new site, but I add all of my existing users to the backend. No re-registration, no changes for the users if you can change the app behavior to match. Just easy.

You can check the second link up above to see what the end table looks like, but for now I'm going to just talk about the rest of this situation like you have an old app and plain text username/password pairs.

Short version of this story is I wrote an app to generate the hashes I needed (based on the source from dotnetcore on github) and then just injected guids into the other fields that needed information. Probably not perfect, but it seems to work fine.
