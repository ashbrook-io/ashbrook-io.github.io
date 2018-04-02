---
layout: post
published: true
title: Quick Performance Monitoring from Powershell
image: 'https://i.imgur.com/yNlQWRM.jpg'
subtitle: but why tho
---
If, you wanted to take a quick peak, using powershell, on some performance metric and show it in write-progress, here's how you would do it.

![why?](https://static3.fjcdn.com/comments/But+why+tho+_e036e04ca4d759b2a2c28302c03d1c8d.jpg)
"But why?" I hear you saying. "Can't I just do that in perfmon or any other tool made for this?" Well, sure, but where's the fun in that. =)

```powershell
$a = "server"
$b = "\Processor(_Total)\% Processor Time"
Get-Counter -com $a -cou $b -s 1 -con | %{
    $cv = [math]::round($_[0].CounterSamples.CookedValue)
    Write-Progress -Activity $a `
        -PercentComplete $cv `
        -CurrentOperation "$b - $cv%" `
        -Status "Measuring..."
}

#or
$a = "server"
$b = "\Memory\Committed Bytes"
Get-Counter -com $a -cou $b -s 1 -con | %{
    $cv = [math]::round($_[0].CounterSamples.CookedValue/1024/1024/1024,4)
    Write-Progress -Activity $a `
        -PercentComplete $cv `
        -CurrentOperation "$b - $cv GB" `
        -Status "Measuring..."
}
```

If you want to see them both, should be simple enough to setup multi-threading and get some feedback. [Here's](http://www.get-blog.com/?p=22) an example of someone doing something similar.