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

So, now we know what to do functionally, but what do we build it on? React? Supabase is hot! Azure functions, lambda, blah blah? Well, for this I generally look at where I am now. I am a one man band for software work here currently, so I try not to stray too far from what we have. Currently our environment for custom code consists of some legacy apps we wouldn't copy, dotnetcore apps, and sveltejs apps that use o365/msgraph for the service tier. 








split from previous article.

#### Part 1, done. Thoughts.

My original high level to do:

1. Handle SQL data (Index)
2. Handle Files
3. Create new searcher app
4. Turn off old machine (equipment will decomm in another effort)

I found that as I was tracking this work for myself, I kind of got distracted by #3 while I was in the midst of #1 and #2. In part, I think this was largely because I thought I wanted to move to cosmos for the database access, but wanted to 'test' the azure tables approach which led to a lot of kind of back and forth mentally for me.

I also had several unrelated projects jump into the middle of this that I had to handle that did not really help that back and forth. Ultimately, I pulled the tricker on cosmos, and then imported the same CSV data into azure sql as a backup if I needed to do some adhoc query of the data and didn't want to do it on cosmos for some reason.

After solidifying that, Step 1 and 2 were done, and knowing Step 4 is not something I'm going to write about at all, I was left with Step 3. I moved on to doing that already, but realized I should probably cut this entry here as the problems/topics for that are completely different. Ultimately I expect that Step to boil down to 'how do i want to wrap and interact with this data in a way that makes sense to the existing users?'

I think that, as with much prototyping, this process would have been faster if I had focused on 'speed'. I could have simply grabbed the 2020s data which was tiny and then started prototyping a UI. But since I already had all of the data, I instead focused on getting all of the data out there so I didn't have to address that step again. Some of this is just due to the nature of the way work flows for me currently. I may have a project bump out all current work due to a business need at any time and sometimes I have to shelve work for months. So I am always in a state of thinking about how I may need to shelve something tomorrow and try not to get things 'halfway' setup that I can't easily delete. Creating a prototype that could be orphaned for a long time isn't valuable, but getting this data 'migrated' is valuable because we *could* lose or shut down the original machine in a pinch if this project is shelved for a year because at that time this archive will be even older and have even less frequent use.

That being said, if I could go back, I would rather have (from a dev standpoint) just dumped a couple years of data somewhere, prototyped away, and then wrapped things up with a '...now i just need to import the rest of the data....' type of idea. But had I used azure tables for that, I may have been sad in the future when I tried to upload all of that data though. =P

Regardless, I am declaring this part 'done' and moving on. Done > Perfect.

Next up, the indexing app. =)





#### Day 9

Now that all of the data was uploaded, It was time to write some code to actually pull the data out and do some cleanup. To finally cleanup, I wiped the sql vm in the cloud and wiped the full backup duplicate azure sql database. I still have the replica of the id,k,v table on azure sql, but I at this point I expect to use cosmosdb for this purpose. I also still have an azure table with a small subset of the data for possibly doing some prototyping on. It is not as if we could *not* put data into azure tables slower, there just doesn't appear to be any value to it vs sql or cosmos so I'm not going to worry about it for now. Ideally, I'm hoping to just publish this solution on GitHub, but we'll see.

While I'm not 100% sure exactly what framework or platform I'm going to use, for starting I just wrote some node scripts to query the db, and then retrieve a file. The important packages for this were:

```
"@azure/cosmos": "^3.15.1",
"@azure/storage-blob": "^12.9.0",
```

...time passes for prototyping and development.

#### Day 10

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

#### Day 11

I was able to get over the CORS issues by specifying any source. Using localhost didn't seem to work and I found a bug resolution on cosmosdb about this that indicated you could just disable the check. For now, I just set the allowed location to * because I'm hoping these queries will actually come from the SPA vs a specific API, but we'll see.

One key item for me was that our users typically just use the old system to search using what are called 'SavedInquiries' in doclink. These filter out the types of document types that will be shown.
- 














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

