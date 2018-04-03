---
layout: post
published: false
title: Queue up or pre-download windows updates with powershell
---
I hope no one reading this ever actually has to do this. In my case, I have some old'ish servers that have very limited downtime windows. While I was updating these I noticed that a lot of them still did not actually 'download' the updates even though they were set to download and install later.

Every time I would get on one to do updates it would first have to run a lot of checks and take a lot of time to go check vs just running the updates it had first. We can check after the first reboot! OK, computer?

So to fix this, I used the following powershell to download the updates.

```powershell
    $o = new-object -c microsoft.update.session
    $s = $o.CreateupdateSearcher()
    $u = @($s.Search("IsHidden=0 and IsInstalled=0").Updates)
    $items = $u | where IsDownloaded -eq $false
    $d = $o.CreateUpdateDownloader() 
    $d.Updates = new-object -c microsoft.update.updatecoll
    $results = foreach($item in $items){
        [void]$d.Updates.add($item)
        $r = $d.Download()
        [void]$d.Updates.RemoveAt(0)
        [pscustomobject] @{
            item = $item
            result = $r
        }
    }
    #view results
    $results | %{ [pscustomobject] @{title = $_.item.Title;resultcode= $_.result.resultcode} }
 ```
 
 [Source](https://msdn.microsoft.com/en-us/library/windows/desktop/aa387291(v=vs.85).aspx) for some of the results you'll get back. You can also just add all of the downloads to the updatecoll and submit all of them at once, but i wanted to do these one at a time just to be safe.
