---
layout: post
published: true
title: Uploading data to o365/spo with Powershell
date: '2020-06-24'
subtitle: ...finally figured it out
image: /img/finally.png
---
I have a static website that I host on o365 for some internal use. I upload some data that is rendered via js at some scheduled times. This works today but I wanted to deploy code via script, so I took another look at how I was doing things to refine it.

While there are a lot of ways to do this, I would point to the following two articles as the most helpful to me personally:

- [Older Article](https://support.microsoft.com/en-us/help/2529243/sharepoint-2010-howto-upload-a-file-of-the-size-of-1-gb-using-client-o)
- [Newer Article](https://docs.microsoft.com/en-us/sharepoint/dev/solution-guidance/upload-large-files-sample-app-for-sharepoint)

Many years ago I did more fun things with sharepoint, webparts, and just things like that in general. But for many years my use of sharepoint has been pretty typical with the occasional desire for some slightly more advanced views or perhaps hosting a static site with some data or something similar. I say that because my familiarity with the object model is utilitarian in nature and as I was going through this exercise, I ran into a lot of gotcha moments that were simply due to my lack of understanding and my desire to just rush through to a functional/working solution.

Here is what I ended up using, although I am adding some heavy annotation here to cover the items that caused me some issues.

``` PowerShell

# check for and load dependency dlls
# these can be found by downloading the sp2013 sdk tool from here:
# https://www.microsoft.com/en-us/download/details.aspx?id=35585
# and then extracting these dlls from that file
$dlls = "MICROSOFT.SHAREPOINT.CLIENT.DLL","MICROSOFT.SHAREPOINT.CLIENT.RUNTIME.DLL"
$dlls | % {Add-Type -path $_}
$loaded = [AppDomain]::CurrentDomain.GetAssemblies() | ? {$dlls -contains $_.ManifestModule}
if($loaded.Count -ne $dlls.Count) {
    "dlls failed to load"
    exit # exit if dlls fail to load
}

# server we want to deploy to
$s = "URL to sharepoint site"
# Note this is the full URL to the site in some format like this:
# https://myprefix.sharepoint.com"
# for me, i was using a subsite of a subsite, so my URL was something like this:
# https://myprefix.sharepoint.com/site1/site1a"

# 'directory' we want to deploy the project to
# Note that this is the server relative url to the actual
# library we will deploy to. i am just using the default
# Pages library as I have a default.aspx there as the home
# page for the site and a bunch of files and folders
$d = "/site1/site1a/Pages"

# files that will be deployed
# Note that ASPX files must be MANUALLY COPIED as they are disallowed by spo
# it's possible there is a workaround for this, but i couldn't locate one
# and it is just as easy to simply drag/drop the one default.aspx i was using
# Note that the following list can be modified to anything that can be
# passed to a gci call, or even replaced with some other way to list
# and iterate the files, but i'm just copying some img/js type files
# so this worked for me
$f = "*.png,*.ico,css\*.css,js\*.js" -split ","
    
# credentials to be used for deployment
$user = "o365 user" # formatted like an email address
$pass = "o365 pass" # plain text as it will be converted to secure string below

# some try/catch blocks so we can properly closed out the context and
# the filestream object, i added some messaging but that just goes to the
# screen and won't be logged as it is 'write-host'. it was helpful
# to me as i was boiling things down though. largely i'm just rethrowing
# everything anyway on errors
try
{
    #auth to site
    # note: this was a sticking point for me early on as i wasn't really
    # understanding exactly what this context was. i guess the important thing
    # i picked up was understanding that this context doesn't actually *DO*
    # anything yet. we are basically just setting up a context that we will
    # try to use later. You could put a bogus URL into $s and these calls
    # will not fail, but later when you try to use the context it will fail.
    # so it's almost like just preparing headers to pass in a webclient request.
    $ctx=New-Object Microsoft.SharePoint.Client.ClientContext($s)
    $cred=New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($user,(ConvertTo-SecureString $pass -AsPlainText -Force))
    $ctx.Credentials=$cred

	# this could be any kind of loop, but for me this is easiest as i can
    # be very loose with the file list up top, and as long as i can run
    # gci on it, it works fine
    foreach ($file in (Get-ChildItem $f)) {
        try {
        	# this is where I had some major issues, trying to figure this out
            # the most important thing is that the $dst value needs to be a
            # *FULL* server relative path, not just relative to the site. I
            # think that you can use $ctx to load a child folder, and then maybe
            # it's possible to tweak this, but this is the easiest way
            # so i just grab my files, get a relative path that looks like
            # /pathto/file/file.txt and then append the full server relative
            # url for the folder we are putting these in, in this case
            # $d = "/site1/site1a/Pages" so a full file path will look like
            # "/site1/site1a/Pages/pathto/file/file.txt". for me personally
            # I am not using anything other than one folder deep, so the files
            # all looked like "/site1/site1a/Pages/file.txt" if in root or
            # "/site1/site1a/Pages/folder/file.txt" in a folder
            # NOTE: It is important to note that the folders need to already
            # be there. There are examples on the web on how to get around
            # this by checking first and creating them, but that requires a
            # lot more code and for me personally I am doing the initial deploy
            # by hand and am only concerned with pushing updates
            $dst=$d + ($file | resolve-path -relative).replace("\","/").Substring(1)
            Write-Host -NoNewline "Trying: $file ==> $dst...... "
            $fs = $file.OpenRead();
            # here we open the file stream above, and pass in the context, relative
            # url where we want the file to end up, and set overwrite to true
            [Microsoft.SharePoint.Client.File]::SaveBinaryDirect($ctx, $dst, $fs, $true);
            Write-Host "Success!"
        } catch {
            Write-Host "Failure!"
            $_ 
        } finally {
            if ($fs) {$fs.Dispose();}
        }
    }
} catch { $_ } finally {
    if($ctx) {$ctx.Dispose();}
}
```

I was using and still am using the following function for most of my jobs, so i'll include it here for reference. It shows another way of doing the upload. This way requires more steps, but also gives you some more options for interacting with the folder. I don't really need to do that so I moved to the method up top. I didn't include the above setup as a function because i just wrote it into a deploy script for pushing some code updates, and i haven't adopted it for my standard jobs that copy data out to SPO, but i think you can see the differences anyway.

``` PowerShell
function Copy-File-SPO(
    $user, #username
    $pass, #password
    $site, #sp site url
    $list, #sp list name
    $dest, #dest path in list
    $path #path for gci
    ){

    # check for and load dependency dlls
    # these can be found by downloading the sp2013 sdk tool from here:
    # https://www.microsoft.com/en-us/download/details.aspx?id=35585
    # and then extracting these dlls from that file
    $dlls = "MICROSOFT.SHAREPOINT.CLIENT.DLL","MICROSOFT.SHAREPOINT.CLIENT.RUNTIME.DLL"
    $dlls | ForEach-Object{ if (-Not (Test-Path $_)) {throw "$_ not found"} }
    $dlls | ForEach-Object{ [void][Reflection.Assembly]::LoadFile((Get-Item $_).FullName) }

    #Bind to site collection
    $Context=New-Object Microsoft.SharePoint.Client.ClientContext($site)
    $Creds=New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($user,(ConvertTo-SecureString $pass -AsPlainText -Force))
    $Context.Credentials=$Creds

    #Retrieve list
    $List=$Context.Web.Lists.GetByTitle($list)
    $Context.Load($List)
    $Context.ExecuteQuery()

    #Upload file(s)
    Foreach ($file in (Get-ChildItem $path)) {
        $file
        $FileStream=New-Object IO.FileStream($file.FullName,[System.IO.FileMode]::Open)
        $FileCreationInfo=New-Object Microsoft.SharePoint.Client.FileCreationInformation
        $FileCreationInfo.Overwrite=$true
        $FileCreationInfo.ContentStream=$FileStream
        $FileCreationInfo.URL = $dest + $File.Name
        
        $Upload = $List.RootFolder.Files.Add($FileCreationInfo)

        $Context.Load($Upload)
        $Context.Load($List.ContentTypes)
        $Context.ExecuteQuery()

        "File name $File uploaded successfully!!"

    }
}
```

Hopefully this helps someone, or at least helps maybe provide some additional information on why someone else's code isn't working. This was kind of bugging me for several months as an unresolved question for why I couldn't get the top method to work before. Basically it was just a combination of:

- my not knowing what I needed to do specifically on the destination path
- getting seemingly permissions errors back on various attempts to upload on the 'wrong' path