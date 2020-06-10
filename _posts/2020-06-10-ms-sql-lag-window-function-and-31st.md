---
layout: post
published: false
title: MS-SQL LAG window function and 31st
subtitle: All 30 day months!
date: '2020-06-10'
image: >-
  https://png.pngtree.com/png-clipart/20190630/original/pngtree-march-31st-date-on-a-single-day-calendar-png-image_4153496.jpg
---
## Note

I will use some dummy tables for this topic and post something to generate some test data as well as well as the full SQL down at the bottom. For now it's important to know this will involve a table I'll name @SalesByDay with two fields named SalesDate and TotalSales. This table has Sales totals by day and has a date in the SalesDate field and the total sales for that day in TotalSales.

## Can we see last month's numbers?

I have a dashboard chart that shows sales by day. I have other similar charts that show X-other-thing by day. Requests for 'forecast' data come up for some of these charts. This requires some discussion but eventually we have some forecast data to look at. This gives us 'today' and some way to 'look ahead' which led to the natural next request of a 'look back' view. This presented as 'let's look at this value on the same day last month.'

Well, there aren't the same number of days in each month, but that problem was easy to solve with some discussion and it was determined that looking on the same date of the month is sufficient. I use a Date table for various date info, so initially this was accomplished with a simple look back based on the previous month and the day of the month via select top 1 subselect. This would yield nulls for the days where there was no day info, and provide the proper info based on the last month and day of the month as needed. While I was doing some other queries in another dataset that did not have a date table for this, I decided to try out the lag function. I was 'aware' of these functions, but hadn't found a use that really demanded them much before. In this case, I could work around it, but upon testing it out the window functions seemed **much** more performant than what I was doing. Great!

## That's weird...

So I switch over the the lag function. I partition by the day of the month value, then order by date and go back 1 value. So all the 1st are together, and the 2nds and so on as part of the partition, then you would go back one row to get the value from the previous month. If you aren't familiar at all with this function, I'd suggest checking [this article](https://www.sqlservertutorial.net/sql-server-window-functions/sql-server-lag-function/) as I feel it does a great job explaining it with some sets of data and arrows.

This works great and everyone is happy. While this is being setup, COVID-19 happens. Why does that matter? Well mainly because I work for a transportation company that moves fuel around amongst other things. As you can imagine, with people staying home, there is a dramatic difference in how much fuel we move around pre vs post stay-at-home guidance.

In May I notice that the 'previous' numbers for the end of the month seem to spike up. Weird, but maybe more info got put in or more business got processed at the end of the month. I'm not an expert in our operations, so sometimes oddities like this are explained away with some special operational processes that I'm not familiar with. I ask a couple of people, no one seems concerned. OK, then. 

Spoiler Alert: this is because things are not actually 'OK'.

## What's wrong?

If you found this page by searching for anything about this problem, you are already familiar with this issue. The problem is that if you are missing a previous day in a month, LAG will simply go back to the actual previous value in that partition. So if you are in May, and you are on the 31st, and you look back to the 'last 31st day value) you will get the value from March. This produced a large spike as we had understandably reduced volume in april, similar to may but not as bad, but no where near as high as in March before all of the changes had really taken effect. So a big spike on the 31st because we have Marches data.


|SalesDate              |TotalSales|SalesPrev|actualSalesPrev|
|-----------------------|----------|---------|---------------|
|2020-01-29 00:00:00.000|83036.05  |NULL     |NULL           |
|2020-01-30 00:00:00.000|43137.25  |NULL     |NULL           |
|2020-01-31 00:00:00.000|52124.10  |NULL     |NULL           |
|2020-02-29 00:00:00.000|1017.37   |83036.05 |83036.05       |
|2020-03-29 00:00:00.000|69213.87  |1017.37  |1017.37        |
|2020-03-30 00:00:00.000|80259.65  |43137.25 |NULL           |
|2020-03-31 00:00:00.000|86825.51  |52124.10 |NULL           |
|2020-04-29 00:00:00.000|11277.62  |69213.87 |69213.87       |
|2020-04-30 00:00:00.000|43474.88  |80259.65 |80259.65       |
|2020-05-29 00:00:00.000|55417.99  |11277.62 |11277.62       |
|2020-05-30 00:00:00.000|34638.87  |43474.88 |43474.88       |
|2020-05-31 00:00:00.000|50828.97  |86825.51 |NULL           |
|2020-06-29 00:00:00.000|19150.60  |55417.99 |55417.99       |
|2020-06-30 00:00:00.000|32841.09  |34638.87 |34638.87       |



``` sql

```

Enter text in [Markdown](http://daringfireball.net/projects/markdown/). Use the toolbar above, or click the **?** button for formatting help.

Searched for lag rownumber missing rows sql
12:45 PM • Details • 
Logo for Search
Search
Searched for sql window function get value from same day last year
12:19 PM • Details • 
Logo for Search
Search
Searched for window function get value from same day of last month sql
12:19 PM • Details • 
Logo for Search
Search
Visited 10 SQL tricks that you didn't think were possible - JAXenter
12:15 PM • Details

Logo for Search
Search
Searched for lag doesn't work for "31" sql
12:14 PM • Details • 
Logo for Search
Search
Searched for lag doesn't work for "31sts" sql
12:14 PM • Details • 
Logo for Search
Search
Searched for lag doesn't work for 31sts sql
12:14 PM • Details • 
Logo for Search
Search
Searched for sql lag partition by how to deal with nulls