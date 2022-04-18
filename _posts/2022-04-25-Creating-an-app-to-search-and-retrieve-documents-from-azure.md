---
layout: post
published: false
title: Creating an app to search and retrieve documents on Azure
subtitle: >-
  Famous last words: This should be easy!
date: '2022-04-25'
---

After moving all of our documents and our index into the cloud (Azure), I needed to create a UI to search the index and retrieve the blobs. This is a sort of diary of my experience building one. This takes place right after migrating the data, but they are somewhat disconnected efforts, so I wanted to split up these two tasks from a narrative perspective. Moving the data and keeping it safe was also a very different level of 'acceptable' vs what I wanted to accomplish with the UI, as will become evident as I record my thoughts on theh process, I think.

#### Prelude and Background

To recap, briefly, the current state of why this is happening, this system is going to replace an existing commercial document management system that has much more functionality. We have already have replaced that system with another offering, but we did not move the archive of what was in the old system, to the new system. We want to remove our dependency on the commercial software because we really just need to search the archive, so why not move the items into the cloud and search things there? The blob and index data have been moved to the cloud at this point, but we need a UI to search it.

Moving on to discussion of the system, the functional requirements are pretty simple:

1. Work similar to the previous system
2. Performance and Cost should be the same or 'better'.

This are my general guidelines for any system that needs to be replaced with an alternative. =)

On #1, this is relatively simple because the current user experience is pretty simple. Login, pick some type of document type filter and search by some fields (one or many) then click search and view results. Click a result to view/download a document. 

![image](https://user-images.githubusercontent.com/7390156/163855483-64991e9f-5c19-4776-ae42-a4ead760c411.png)

Well... first we have to build it, but the basic flow of the system. is pretty simple. =)

On #2, performance should be fine as this is pretty basic. Any weirdness can be solved with indexing, but I don't expect major issues because this is static data that doesn't update. And cost will just be lower because we won't pay for the software and all estimates show this will cost dollars per year, not thousands for our usage. On cost you may think 'oh... are you one of those labor is free people? because it still takes time to migrate!!!' and you would be right to ask that, but wrong on the labor is free. Although from a business perspective, they probably wouldn't worry about this in terms of prioritization, the reality is that this machine is running on some of our last bits of hardware to be removed from an old data center. We already had backups so the plan for some time has been 'if this physical machine breaks, we'll migrate to the new thing'. This is 'the new thing' in that description. The physical machine hasn't broken, but we are at the time to sunset it, so yay! In short, we can't 'do nothing' anyway because we need to empty the data center. =)

So, now we know what to do functionally, but what do we build it on? React? Supabase is hot! Azure functions, lambda, blah blah? Well, for this I generally look at where I am now. I am a one man band for software work here currently, so I try not to stray too far from what we have. I don't want anyone who comes after me to have to maintain C, Java, Powershell, MS Flow jobs and GO or something. Even thought I'd love to write in as many languages as possible, it's just not a great stewardship of the environment one is in. Currently our environment for custom code consists of some legacy apps we wouldn't copy, dotnetcore apps, and sveltejs apps that use o365/msgraph for the service tier. All automated jobs are in powershell. SQL is all on azure.

I have now introduced a couple of variables into the equation. A database in cosmos (although there is a copy in sql) and a set of blob data in azure storage. We have had blobs on azure storage for years, but only as a cool archive that is accessible by administrative staff, not something tied to an application where a user may have an error. Also no users access that needs to be audited, etc.

Some considerings for me, personally, as I work on this are that I could *probably* follow a very simple tutorial and built a blazor/wasm app in dotnet, which I have considered replacing some of our existing applications with. The other major option for me is implementing an app on sveltekit. I use sveltejs currently for one of our key internal applications, but I backend the services using some o365/msgraph magic and I think moving to a more 'traditional' setup with sveltekit would make more sense and provide a path to update the internal svelte app. This application is probably simple enough to provide some good proof-of-concept on both platforms and give some insight into how that would work.

Some additional details about the environment are that our users would be using azuread for authentication anyway, and likely using rbac in some way for authorization to the app. Possibly more on that later, but ironically our dotnet apps do not use azure auth, but our sveltejs app does. =) Plus our dotnet apps use sql for backends, and the svelte app uses o365/msgraph for it's data. This was more of an organic development than planned, but I think it provides a little color on the situation.

I think that's enough current state info. Now I'm going to just hop into a play-by-play of what I did.

_note: I wrote this prelude/background section after a few days of the play-by-play below, so there may be some recap or extra details about info here. But I wanted to leave it as I wrote it while I was doing it and creating a narrative around this process is what I was hoping to do._


#### Day 1

Now that all of the data was uploaded, It was time to write some code to actually pull the data out and do some cleanup. To finally cleanup, I wiped the sql vm in the cloud and wiped the full backup duplicate azure sql database. I still have the replica of the id,k,v table on azure sql, but I at this point I expect to use cosmosdb for this purpose. I also still have an azure table with a small subset of the data for possibly doing some prototyping on. It is not as if we could *not* put data into azure tables slower, there just doesn't appear to be any value to it vs sql or cosmos so I'm not going to worry about it for now. Ideally, I'm hoping to just publish this solution on GitHub, but we'll see.

While I'm not 100% sure exactly what framework or platform I'm going to use, for starting I just wrote some node scripts to query the db, and then retrieve a file. The important packages for this were:

```
"@azure/cosmos": "^3.15.1",
"@azure/storage-blob": "^12.9.0",
```

I just followed the instructions for node.js on the MS site for doing the initial tests with these and fed in the queries with hand edits to test.

...time passes for prototyping and development.

#### Day 2

I like sveltejs, so I am going to start with that. I am, generally, more of a microsoft fan, but I use svelte pretty regularly for a very frequently updated project day to day, and I have not moved it to svelteKIT yet, so I think I'll go with that. To start, anyway. Current options I'm considering:

1. dotnet core or blazor/wasm app
2. sveltekit
3. sveltejs or sveltekit in spa only mode
4. react/vue/angular (just because i haven't made one of these in a while)
5. something else...?

Microsoft stuff was top of mind since this is, for me, all running on azure services, but after writing my test node scripts I decided to just init a sveltekit project and see how that went. In general if I was using Firebase, or Supabase (lots of tutorials lately on that) I could just handle all of the access there, I think. But in my particular case I have users who have o365 access and I want to lock everything down with AzureAD. The document repo itself is only accessible to admins currently as the previous history retrivals were specific requests for a specific file from the old system. Now that we'll be creating a new index search app, why not tie it all together with MS auth and use the services available within azure?

To that end, I headed over to the cosmosdb, opened up IAM, then added some appropriate users as readers. This app has no 'write' requirement as it is just for searching the index so this is all we need.

I believe I need an app registration as well, so I go and create one over there I set the following things:

- single tenant, no implicit/hybrid stuff (don't click check boxes)
- platform config to SPA (for now)
- the redirect URI to localhost:3000 for now
- API permissions, user impersonation for cosmos and azure storage, grand admin consent

I did some fiddling on this throughout the day, but had to take care of some other things, so basically just got the sveltekit project stubbed out and got msal running and then plugged in some basic queries with env variables client side to test communication. Ran into some CORS issues to look into next.

#### Day 3

I was able to get over the CORS issues by specifying any source. Using localhost didn't seem to work and I found a bug resolution on cosmosdb about this that indicated you could just disable the check. For now, I just set the allowed location to * because I'm hoping these queries will actually come from the SPA vs a specific API, but we'll see.

One key item for me was that our users typically just use the old system to search using what are called 'SavedInquiries' in doclink. These filter out the types of document types that will be shown.

I was able to get a basic prototype working where basically you logged in and hit a button and it would query some random collection of documents (of the appropriate type) from cosmos and display them along with a download button that you could click.

This is great, but I was not using the user's credentials directly, instead I was using the application/service keys and I wanted to really use the user's credentials and rbac for managing access. But this was just the initial prototype for a simple UI with some simple details, so at least the plumbing works.

As this only has limited users and no real 'roles' other than 'has access' I can just add the users directly to the app if desired, and then I could make sure all queries to get the data and blob happen on the server. This seems like the 'typical' way to do things, but I'd love to just have the users go straight to the service if I can from the client side and skip having a service tier to just relay data if possible.

We currently house some data on o365 and use msgraph for this purpose in another app and it works great. But msgraph doesn't appear to have a channel to just route to cosmos or sql. Maybe I can go through a data gateway and built some other service routing mechanism in o365 but I feel like that's just making things complicated at that point. The current o365/msgraph stuff works well because it's simple because the data is on sharepoint.

#### Day 4

Off and on tasks bumped in front of this over the course of about a week somewhat stalling any changes. When I came back to things on a fresh monday, I was ready for a fresh start an decided I would just take a hard turn and spin up a blazor/wasm app to test things out that way. This day was mostly spent cleaning up the existing progress, bundling all of the updates/changes I had fiddled with into a PR on my repo, and putting together my planned next steps. Which *really* just amounted to 'make a blazor/wasm version of this'.

At this point, I'd like to share some code here, but a lot of this prototyping I am just using hardcoded keys so I would have to clean it up. I may come back and do that and edit this in the future. =)

I'm on a pretty new macbook and I haven't installed dotnet tools on here yet! So this also gives me a chance to run through some of those steps that I did.

1. [Install dotnet of course!!](https://dotnet.microsoft.com/)

That's it. =P Dotnet installed!
```
Last login: Fri Apr 15 11:53:29 on ttys000
roy@work gh % dotnet --list-sdks
6.0.202 [/usr/local/share/dotnet/sdk]
```

I spent a lot of time on some apps that are based on business users changing data structures. To that end, I don't typically use typescript as I am normally shuffling data through generic table handlers or using basic table/row structures that I don't want to create types for. I could, but I haven't. However, I believe everything in blazor/wasm will require typing, so maybe I'll feel like switching to typescript after this. You never know!

I perform some basic steps to spin up something to open in vscode:

1. create a new folder in terminal, switch to it
2. dotnet new up an app like `dotnet new blazorserver -f net6.0`
3. setup loca git on this code with `git init && git add . && git commit -m "initial"`
4. open folder in vscode with `code .`
5. vscode asked me to install some dependencies and I clicked ok
6. open up terminal in vscode and run the app with `dotnet watch run`

Now we have a running app with some sample stuff in there. 🎉

Tomorrow, will be time to connect some dots on the blazor app.

#### Day 5

Things I did:

- First thing I did was copy/paste the fetchdata page since I was going to want to fetch data, that seemed like a good place to start.
- Rename this page to something meaningful. I think rather than having a single page with 3 buttons, I am just going to have three pages in the blazor application so they'll just pick one from the left nav bar. I'll call this one Billing for now.
- Now that it's renamed, open up Billing.razor and change the `@page` line to use `billing` instead of `fetchdata`
- Open up shared/navmenu.razor and copy/paste the fetchdata link (or whatever you want) and update it to go to billing and have the right label.

Now the page shows up. 🎉

_note: I could also run a `dotnet` command to generate this, but i just copied it and will work that way for now.

Ran `dotnet dev-certs https --trust` and restarted app so I don't get warnings on local https.

Added `dotnet add package Newtonsoft.Json` so i can support handle cosmos data. It's right about here that I start to whistfully think about just using the old sql database and generating code. Sadly, it seems that cosmos doesn't support generating the model or any classes. So I just write a little sql code to generate this big class with newtonsoft decorations. Data output looks similar to this:

```csharp
namespace Data
{
  using Newtonsoft.Json;

  public class SearchResult
  {
    [JsonProperty(PropertyName = "1")] public string? x { get; set; }
    [JsonProperty(PropertyName = "2")] public string? y { get; set; }
    [JsonProperty(PropertyName = "3")] public string? z { get; set; }
    //etc etc x ~60 properties
```


















stub post copied below for editing. =)
I got tired of copying/pasting while adding icons from [devicon.dev](https://devicon.dev/) to my GitHub profile page recently, so I created this little helper script so I could just click on the icons and it would generate some markup for me.

{% gist 64a1333684c40b4d885fb6d353913266 %}

### Some more history on this:
Recently, I 'discovered' that I could add a README.md to my GitHub profile page. I'm quite late on this 'discovery' only coming across it when I went to [someone's profile page](https://github.com/dfinke) and they had a lot of cool things there.

A handful of youtube videos later and I was ready to spin one up. I wanted to add some icons for the dev items and came across the [devicon.dev](https://devicon.dev/) site which had a ton of great icons I could include via CDN.

This was cool, but since I wanted to simply add the img tag, I had to select an icon, go over to the left side, then copy and paste the img tag which wasn't really convenient setup to do this. See below for a dramatic recreation of this activity!

![Animation](https://user-images.githubusercontent.com/7390156/146087974-f3345676-a611-43a0-af16-97fd2c8e11d3.gif)

Maybe not so dramatic.. 

Well... anyway, since I was fiddling around with formatting and things, I ended up going back and forth and back and forth while I figured out which icons I wanted to use, etc. After a bit, I decided that this was kind of a pain and I wished I could just click on the icons I wanted and copy and paste some markdown. So I put together the little js above and ran it in my browser console. It could be tweaked a lot, but as with all things like this, [it may not be worth it](https://xkcd.com/1205/).

If i was passing these through a resizer, I'm guessing I could use normal image tags, but this setup seemed to be similar to what I found on some other profiles and kept all of them in a line, so that's what I went with. It's easy enough to tweak the line that outputs the markdown if needed.

I actually ended up just kind of hand tweaking things on my file rather than using this too much, but it helped me while I was fiddling around and gave me a little entertainment, so maybe it'll help someone else! =)

- [read more on the github docs](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-github-profile/customizing-your-profile/managing-your-profile-readme)
- [great tool to automate this](https://github.com/rahuldkjain/github-profile-readme-generator) - A better way to do this!
- [great article with a bunch of great tools to automate this](https://javascript.plainenglish.io/the-best-readme-generators-for-your-github-profile-ea4f50559d87)

