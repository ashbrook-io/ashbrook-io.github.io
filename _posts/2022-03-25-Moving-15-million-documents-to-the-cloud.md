---
layout: post
published: false
title: Moving 15 million documents to the cloud
subtitle: >-
  Are we sure we don't like on-prem?
date: '2022-04-01'
---

Moving an image repository to the cloud is not quite what I would call turnkey. Starting up a new one is, I think, relatively simple. At the simplest you could just grab a SaaS solution that offers the capability that you want. At the most complex, you could analyze all the difference services that you want and build something yourself. Moving an existing one has an array of considerations, however. Mostly I think these can be broken down for many non-IT folks as:

1. Does it work as least as well as it did before?
2. Does it cost less than it did before?

I've worked on several similar projects in the past. I think most were actually bigger than what I'm doing now, but they were at bigger companies so there were lots of decisions that were already made. Things like which vendor to use or what hardware to use or whether or not the cloud could be used were normally matters of internal policy and not something the project team was able to change or address.

Currently, I am working at a relatively small company for myself. ~500 people and ~100MM in revenue. I'm not sure on the exact numbers, but just for sizing it's around there. We are a transportation company and everything transported has papers associated with it. We also have expenses and receipts that need to be tracked. I figured I would write about migrating this system to the cloud. 

### As-Is

Currently, we already have a new repository system in place. All new images go there and all that system works fine, but we did not want to move all of the old stuff over to keep costs down. The new system is not cheap in terms of storage and that's a discussion for another day. We had a team that was still adding some data to it while they migrated that pinned the old system in place until recently. Now they have moved to the new system, so we can address the old system in terms of what to do with it for a longer term home. I'll call this system the legacy system. The legacy system is used for reference and searching for older documents on an as needed basis. It's not frequently needed, but it *is* still needed. Currently we have about 16 million documents on an old server. Normally there is an id number that exists in another system that we would look up the image in this system, but there are about 30 other fields that are used to index the documents as well for searching.

The existing system has a little over 16 million files stored and about 133million indexed values. Some of these documents have multiple pages, but they are generally just single page. The document table shows that there are about 13.5 million documents and maybe 10-20k more pages than there are document objects. Seems this system may have been purged prior to my time here in the last 5 years. This makes some senser as there are documents going back to 2002 for some reference data, even though I don't see any documents dated earlier than 2006. As best we can, we'd like to take everything if possible since this is just an archive anyway.

Current system is a win2k8 machine running an old version of sql that has been migrated probably since sql 6.5 or sql 7 a few times. This is one of the last machines closing out a datacenter migration project. All major systems have been moved for some time, but now it's time to handle a few infrastructure servers and this one so all of the equipment can be decommissioned. The old version of windows and sql limits a few tooling decisions and wizards, but for the most part we'll just be working around those.


### To-Be

Target is currently to get the index data into some location where we can setup a simple web application to search the existing index data. When searching for solutions that involved storing blob data and using an existing index, I didn't see much. I'm guessing because it would seem that this is fairly simple, I think. Copy the blob data somewhere. Copy the index somewhere. Make a little app to search the index and give a link to the blob data.

This is kind of high level. As this system maybe has a dozen users who may search it, it doesn't not require anything incredibly robust, but we don't want something that is bad either. The main goal for target state is maintaining the search and getting rid of the old infrastructure.

To that end, the current plan is something like this:

1. Handle SQL data (Index)
  1. Create a new temporary sql VM in azure.
  2. Copy a .bak file and restore it.
  3. Use the new server to purge anything needed, then migrate to sql azure
2. Handle Files
  1. ~11 Million files have already been moved into a Azure Storage Container
  2. Move the remaining files (~2.5 Million)
3. Create new searcher app
4. Turn off old machine (equipment will decomm in another effort)

### What happened?

As the new system is already in place and I'll probably have to manually reconcile some delta for some reason, I imagine my plan will likely get tweaked a little while I'm doing the actual work. Especially since I have not decided on the final solution for how to index things. So here's what actually happened. Basically my notes on things as I went through the process.

#### Day 1

Initial day was spent analyzing current state and coming up with some of my initial plans. Getting access to the current server, examining the data that we had out there. Looking at some options for target state. Looking at what we had on Azure already for this. Even looking at possibly using some OCR solutions to perhaps auto index this. In the end. I went with the most simple approach of just trying to move the blobs and setup what was minimally required to replace the current solution.  It's a lot of documents and I really need to keep the work time down as low as possible as well as the cost so, let's get on with things!

Initial high level plan after this day:

- Stage Data
  - Copy SQL to Cloud
    - Temporarily created sql VM
      - Patch, etc, blah
    - Copy up the .bak file
    - Restore the file to db
  - Copy Files to Cloud
    - Robocopy to temp server (in progress)
    - Move into azure storage
- Build replacement
  - Prepare data if required
  - Build solution on top of data


#### Day 2

Create temporary sql server. I simply searched for sql and picked the most recent windows server with the latest version of sql. I called this server DB01

snip from the export template:

```
"hardwareProfile": {
    "vmSize": "Standard_DS12"
},
"storageProfile": {
    "imageReference": {
        "publisher": "microsoftsqlserver",
        "offer": "sql2019-ws2022",
        "sku": "sqldev-gen2",
        "version": "latest"
    },
```

As soon as it was up, there seemed to be some windows updates. So I did those and restarted. Then I went and grabbed the RDP file for download. I'm using a mac and have the microsoft rdp app to use for this.

![image](https://user-images.githubusercontent.com/7390156/159936739-ee042d8b-b6d0-4637-901d-8bd5fd583509.png)

I opened RDP with no issues and just start>run and typed in the UNC of the source server, authenticated, and dragged the file over to the data drive on the new server. The backup file was about 100GB, so I didn't bother with any other fancy copy commands with auto-resume or something as I expected it to work fine in relatively short order and it did. I think it maybe took ~15-45 minutes to copy or so. I moved on to some other items while that was going in the background so I didn't watch it too closely and it was done when I came back to it.

Once it was done copying, I restored it to sql. I simply had to change the target file locations to match the servers drives and no issues there. I think maybe this took 30-60 minutes. As above, I just peaked on it to see it was still going from time to time and that's about it.

The new SQL server had, by default, ~1TB data and log drives so plenty of room for my purposes. We have ~4 or 5TB total blob data, but there is only ~0.5TB left on the active server as everything else has already been archived to Azure previously. 

To copy the files, I simply used robocopy to mirror the source folder using something like this: `ROBOCOPY \\sourceserver\sourceshare\docfolder f:\docfolder /MIR /MT:128`.

##### Summary

Initially I forgot to put the MT flag and it ran all night and only copied about 5% of the data. So the next morning I remembered I should do that so stopped the command and ran it again with 128 threads. It uses 8 threads by default. Just doing that increased the speed by about 10x for the copy and it finished in about an hour or two the next morning I believe.

I also originally created my vm with some wrong config and had to wipe it and start over. Not a big deal, but just worth noting as it was part of the experience. =)

At the end of the day I had the files copying and the database restored to a sql server in azure.

Plan for the next day:

Initial high level plan for next steps:

- Move DB from temp server to Azure SQL
- Finish moving files to temp server
- Move files from temp server to blob storage
- Get started on moving data to Azure Tables (or.... whatever?)

#### Day 3

First thing was getting the right MT switch on robocopy and getting that copy going. This worked without an issue and I moved on to copying the files to blob storage. These files are for archive purposes, but for now we are just putting them into cool storage. I already have a storage account named 'cool' that we use for this purpose. It has all of the older data in it already. I believe when I moved it over several years ago, I used azcopy to do this. In modern times, I can just use the azure storage explorer tool. It uses azcopy under the hood (you can actually copy the command once you've started the process) so it just makes my life easy. These blobs are stored in a single folder, then in that folder there are folders numbered 000-999, then each of those folders has the same numbered folders, then under those folders are the files. For top level folders we only have numbers 000 through 017 total. I think the old system must use roughly one folder per year as we do have ~18 years of data, but I'm not certain. For our purposes, it doesnt' really matter. It just matters that we only have folders 015, 016, and 017 left on the server.

015 and 016 have actually previously been copied, but as that was several years ago, I went ahead and mirrored those up to db01 anyway, and then i'll just deal with duplicates when using azure storage explorer. There is an upload button in this tool:

![image](https://user-images.githubusercontent.com/7390156/159946274-ea7ef4d7-f985-4105-9631-dd6077d759f8.png)

So I just went into the root folder as this is where I have all of the numbered folders. Hit the upload button, and then picked the 015 folder first and uploaded it to the root. I picked the default values as these were fine for me. By default I already have this container set to cool. 015 processed and said only 1 new file. It pops up with a question on duplicates and I chose the option to basically not upload duplicates. Same process with 016 and there were no new files, then with 017 which had all new files.

See screenshgot below. `doclink` is the name of the storage container (the software we're archiving from is called doclink) and the F: path is from the db01 where I am running this process.

![image](https://user-images.githubusercontent.com/7390156/159947272-cd77ad1c-d200-485a-be27-23c3d0e3fb6f.png)

I then pulled statistics on these three folders to make sure the file count was the same as on db01. You can pull statistics by going into the folder and hitting the appropriate button. See screenshot:

![image](https://user-images.githubusercontent.com/7390156/159948261-4014ff8f-7be9-4ed5-8723-fa3458e8d39c.png)

And here are the statistics results:

![image](https://user-images.githubusercontent.com/7390156/159947762-f204515c-9571-499d-a242-a480e8c901f6.png)

And then I simply right clicked on each folder on db01 to get a count:

![image](https://user-images.githubusercontent.com/7390156/159948702-3437eb5f-ccba-4e36-8527-372a9756f296.png)

Everything matches, so yay, this part is done. Files are moved.

On the original server, Folders 015 and 016 were moved to another location so they don't show up in the app any longer. Just in case there are some kind of versioning updates or anything, I just want to minimize that. When a file is not there, the app will toss an error with the file name and we simply go and retrieve that out of cool storage. This is already an existing practice so nothing that will cause any unforseen experiences. 017 is left as I'll probably go back later and take one last look to see if anyone snuck a document in somehow as we have not completely disconnected all scanners, just repointed to them to the new system.

While this was going on, I was also working on the database migration. This part was relatively straightforward as well, I simply opened up management studio on db01 and picked the deploy to azure task. We have moved a number of databases to azure sql over the years, and this is generally how I do this.

![image](https://user-images.githubusercontent.com/7390156/159950866-8c1b6b1b-d68b-45d9-8fe2-a690f7ed7452.png)

There were a number of logins and linked servers and other references that failed when I tried initially. This is kind of normal for older servers as these things may be busted and not working already, but not throwing any errors since the server may have light use or some other situation like ours. I just deleted all of these objects and then the process went off without an issue. This process did take quite some time, but finished by the evening without issue.

I also copied the original .bak file to the cool storage and set it to archive. There isn't a real reason to think that we'll need this original file, but just in case this will be much easier than trying to retrieve tapes as that is where the old servers backups went. Very minimal cost for a tremendous reduction in headache should we need it in the future since we would do the same thing and just copy it to a temp server and restore it for whatever reason.

Azure SQL was made premium for the transfer, and then it was reduced in size and the database was moved into our sql pool so we could give up that instance. This database is also going to be removed eventually, but this way I can work with sql for now and we can drop the temp sql server.

Plan for the next day:

1. kill db01
2. move data to azure table

#### Day 4

So first thing I did on Day 4 was start writing this blog post. I had already intended to keep a running narrative, but I realized I was on Day 4 and just had notes of what I was doing which isn't exactly what I wanted to do originally. So I caught up to here. =)

I'm still not exactly sure what shape the data is going to take, so I cleared out all of the data from the database just to narrow the objects in my head space for now. DB01 was turned off for now and I am using Azure Data Studio for all of my sql work on my mac. The key tables with metadata that I need are these:

- PropertyCharValues
- PropertyDateValues
- PropertyFloatValues
- DocumentFiles
- 
There are some other 'type' tables and also a table for mapping the file to the documentid as well as a handful of other items. But mainly we need the index data for the property value tables and which file it goes with. There are some versioning considerations also, but mainly my point is there are another 300+ tables that we don't need. So I first want to clean this out just so we can retain this base copy of all of the metadata, but skip all of the junk we don't. So next I went through all of the tables and other objects and cleaned things out.

First I wiped all of the views using `Select 'drop view [' + [name] + ']' From sys.objects where type = 'v'`. Note that this will actually catch some system views that won't delete. But I wasn't worried about this since this is basicaly a scratch type of db and worked for my purposes.

To do this I used variations on this sql:

```sql
SELECT
'drop table [' + t.NAME + ']' AS TableName,
MAX(p.rows) AS RowCounts,
(SUM(a.total_pages) * 8) / 1024.0 as TotalSpaceMB,
(SUM(a.used_pages) * 8) / 1024.0 as UsedSpaceMB,
(SUM(a.data_pages) * 8) /1024.0 as DataSpaceMB
FROM sys.tables t
INNER JOIN sys.indexes i ON t.OBJECT_ID = i.object_id
INNER JOIN sys.partitions p ON i.object_id = p.OBJECT_ID AND i.index_id = p.index_id
INNER JOIN sys.allocation_units a ON p.partition_id = a.container_id
WHERE i.OBJECT_ID > 255
and t.type = 'u'
AND i.index_id IN (0,1)
GROUP BY t.NAME
having MAX(p.rows) < 1
ORDER BY TotalSpaceMB DESC
```

which is a variation of the script found [here](https://blog.sqlauthority.com/2021/02/12/sql-server-list-tables-with-size-and-row-counts/?msclkid=42a165deab9911ec9ea61f00cf49541a). There are tons of ways to do this, but I just picked the first one from google for listing all table row counts.

I also used a variation of the view query above to sort some of the tables by names when i knew groups of names were not needed. `select * From sys.objects where type = 'u' and name like 'app%'` for example.

First I wiped all tables that had 0 records. One of those had a FK, so I checked it out and it was part of a group of tables that started with ERM, there appeared to be a group with that and ERT. I think these had to do with printing. I wiped these as well and dropped about 5GB and maybe 20 million rows total from the DB.

After this, I did a couple more batches of tables and every time got a handful of constraint issues. After dealing with a few manually, I hit up google for a 'wipe all fk constraints' script. Used this:

```sql
declare @sql nvarchar(max) = (
    select 
        'alter table ' + quotename(schema_name(schema_id)) + '.' +
        quotename(object_name(parent_object_id)) +
        ' drop constraint '+quotename(name) + ';'
    from sys.foreign_keys
    for xml path('')
);
exec sp_executesql @sql;
```

which was one of the solutions [here](https://stackoverflow.com/questions/12721823/how-to-drop-all-foreign-key-constraints-in-all-tables?msclkid=54954aa9aba411eca4db441e338e3170)

Dropping audit tables got me back another 10-15GB and 25million rows or so. I basically repeated this process several times over about an hour or so to just keep peeling out data. While I was doing this, I would pull out some kind of batch by name or some other criteria, maybe take a look through the tables to make sure there was nothing I wanted to keep, and then purge any other tables. After about an hour I had peeled ~25GB out of the ~100GB db. Obviously I hadn't made it to any of the big tables yet and *hopefully* we would end up keeping quite a bit as most of this data *is* index data, but you never know.

I had a soft target of at least getting below 50GB as I had to increase my pool DB max size from 50GB to fit this particular database, so I wanted to target that so I could put things back where they were. =)

Finally, I got everything down to 11 tables. Most were pretty small as they are just lookup tables so I wouldn't need those long term, but I'm going to hang onto them for the moment. Since there is no report helper in azure data studio, I had to look up a query to pull the same data.

query from [here](https://stackoverflow.com/questions/14685596/queries-to-generate-a-disk-usage-by-top-tables-report-in-sql-azure?msclkid=97bf34acabae11ec9366f59a613f23a1)
```sql
 SELECT TOP 1000
        a3.name AS SchemaName,
        a2.name AS TableName,
        a1.rows as Row_Count,
        (a1.reserved )* 8.0 / 1024 AS reserved_mb,
        a1.data * 8.0 / 1024 AS data_mb,
        (CASE WHEN (a1.used ) > a1.data THEN (a1.used ) - a1.data ELSE 0 END) * 8.0 / 1024 AS index_size_mb,
        (CASE WHEN (a1.reserved ) > a1.used THEN (a1.reserved ) - a1.used ELSE 0 END) * 8.0 / 1024 AS unused_mb

    FROM    (   SELECT
                ps.object_id,
                SUM ( CASE WHEN (ps.index_id < 2) THEN row_count    ELSE 0 END ) AS [rows],
                SUM (ps.reserved_page_count) AS reserved,
                SUM (CASE   WHEN (ps.index_id < 2) THEN (ps.in_row_data_page_count + ps.lob_used_page_count + ps.row_overflow_used_page_count)
                            ELSE (ps.lob_used_page_count + ps.row_overflow_used_page_count) END
                    ) AS data,
                SUM (ps.used_page_count) AS used
                FROM sys.dm_db_partition_stats ps
                GROUP BY ps.object_id
            ) AS a1

    INNER JOIN sys.all_objects a2  ON ( a1.object_id = a2.object_id )

    INNER JOIN sys.schemas a3 ON (a2.schema_id = a3.schema_id)

    WHERE a2.type <> N'S' and a2.type <> N'IT'   
    order by a1.data desc 
```

results:
![image](https://user-images.githubusercontent.com/7390156/160002173-fc69d3cb-d164-4c21-bf93-f6e8db9e5888.png)

Not too shabby, but I think we can probably kick some of these indexes off. Given the size difference, I started with the Documents and DocumentFiles tables to see how performance was dropping the non PK indexes. Most likely, this will all simply end up as a single table using the file name as the key since that file name matches the blob path to the file. As this is an archive, there will be no more property types or anything to add, so there isn't really a need to map types to tables,etc. We can just use columns. I'm thinking this will be easier/faster to do, in terms of manipulating the data, within SQL. But once I get a canned query, I may just extract the data a year at a time and import it.

It also *appears* that I could just add these values as tags onto the blobs and I think searching that would just be a traffic cost which is minimal as far as I can tell. Most of the pricing data I could find had to do with using azure search which would be more in tune with using AI to build an index and search based on extracting data. But since I already have the index, that doesn't seem really needed. These are just thoughts I'm thinking while I wait for the initial index drop to take place on the smaller tables. =)

...time passes...








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
