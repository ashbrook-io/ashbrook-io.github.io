---
layout: post
published: false
title: Batch decode user agent strings
subtitle: mozilla who/what now??
date: '2020-06-18'
---
Recently I downloaded some web logs for an app I have running. This app is pretty old and hasn't been changed in quite some time. We're looking to freshen it up a bit, but it is a fairly simple app and there are no requests to 'change' it so we want to be careful not to break anything for any of the users.

To that end, I've been making a list of the changes, bugs, etc. Basically anything that we may want to look at changing. One component we need to look at is browser compatibility as we want this to be transparent to the users if possible. And to make that happen we're checking our logs. So I'm going to write this up just in case anyone else needs to do it as I couldn't find a quick/easy way to convert this information.

Maybe this isn't the quickest or easiest, but it's where I landed and I couldn't find anything that explained how to do this in one place so hopefully it helps someone else. If not, hopefully it helps future me. =)

So now I have some iis weblogs downloaded from azure. These are in iis log format. Those logs and powershell are required to start. 

``` PowerShell
# list of the log files full names
$logfile = (gci "C:\path\to\my\log\files\*.log").fullname

# the header we'll use for our csv conversion. you could probably use other methods, but this is how I usually do it and it works fine. the headers you use may vary, you should be able to find it in your file and just copy/paste it like this and split it by space like below
$header = "date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken" -split " "
# now i want to import the data as csv using a space delimiter and the header above, in the case of merged files (which i have had in the past where i just echoed a bunch of files together), and to avoid the header rows unless you want to skip them, i add this where clause
$data = ipcsv $logfile -Header $header -Delimiter " " | where date -ne "#Fields:"

# now that we have a variable with our data in it, let's filter out our user agents
$agentinfo = $data | # filter our agent info out of our log data
    where sc-status -eq '200' | # filter out successful calls
    where cs-username -ne '-' | # make sure there is a user, since those are the browswers we care about
    group "cs(User-Agent)" | # group by the user agent so we can see counts for these browswers
    select Count,Name | # get a count and a group name so we can see which are most impoortant
    sort count -desc | # sort it so they are ranked by use
    select Count,@{n="Name";e={$_.Name.Replace("+"," ")}} # get rid of the + signs in the useragent
    # the last step isn't really needed, but the + makes it hard for me to eyeball it, so i replace it
    # you can also send it without the plus to the service below, so that's how i do it
```

So now I have my agents. I found plenty of places online where I could copy/paste in the agent string and get the info I wanted. But as I may have to do this for some other websites, I wanted to get something a little bit more reliable and repeatable than that. One of the sites I used to copy/paste while checking was (this one)[whatismybrowser.com] and it also happens they have a free tier for an api that allows a limited number of queries. Since I really only need a very limited number of on demand runs of this, that works for me. So for the next part you'll need an api key from there so you can send a request in to get the info you may want. In my case, I'm really just looking for something like 'IE11 on Windows' or 'Chrome 70' or similar. I hadn't decided __exactly__ what I wanted to see, but definitely not the agent string itself. =)

So this is what I did next:


