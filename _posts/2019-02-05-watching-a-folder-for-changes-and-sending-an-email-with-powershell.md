---
layout: post
published: false
title: Watching a folder for changes and sending an email with powershell
---
I [previously wrote](http://ashbrook.io/2018-07-24-watching-files-with-powershell/) about watchintg a folder for changes and displaying some information on those changes. This was primarily used for some live monitoring of a file mover that was being sunset.

After some use, like all good techy people, I wondered why I should even display this to the screen. I mean I did want to log the changes, but really I just wanted to be notified that the event (the file moving) had started, and then I could review the logs. So, to that end, I tweaked my process a bit. I would monitor the files in one powershell window with a similar method to the previous one, showing all of the create/deletes, but I would add an extra window that would just shoot me a text (via email) when the event had started.

To do this, just need to have a separate console session and have the triggered action send an email and then *stop monitoring* otherwise you may get a million texts like I did the first time I tested this.



Enter text in [Markdown](http://daringfireball.net/projects/markdown/). Use the toolbar above, or click the **?** button for formatting help.
