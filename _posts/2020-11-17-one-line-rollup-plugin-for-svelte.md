---
layout: post
published: false
title: one line rollup plugin for svelte
date: '2020-11-17'
---
I am a simple man. I have simple needs. Like I need to output the results of `git log -1` into a file with some formatting when I build. Not so much to ask. Here is a line to do that: `{generateBundle(){require('child_process').exec("git log -1 --pretty=format:%H%n%ae%n%ad > .\\public\\build\\commit.txt")}},`. Basically you just add this to your rollup.config.js in the plugins section. How about a little more explanation?

## um... wut?

I have a svelte project. I wanted to include a way to tell what git commit i was using when I deployed it around. [Here](https://www.hanselman.com/blog/adding-a-git-commit-hash-and-azure-devops-build-number-and-build-id-to-an-aspnet-website) is a great blog post on how to do this properly in the dotnet world. I am using devops to deploy, but my app is a svelte app and I simply have the pipeline set to deploy the public folder on every checkin to a vnext site. I wanted a similarly simple solution for adding some git hash to my build output.

## ok, so about the one line..

Yes, yes, yes. I know it's a _long_ line. =). So if you aren't familiar with [svelte](https://svelte.dev/), it is a js framework and it uses [rollup](https://rollupjs.org/), by default, as it's bundler. You can use whatever you want, but I'm using rollup since that's what it came with.