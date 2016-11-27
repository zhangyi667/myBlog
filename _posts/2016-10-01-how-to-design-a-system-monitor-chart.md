---
layout: post
title:  "How to design a system monitor chart"
date:   2016-10-01 18:30:46
categories: work
shortcut: This article shows my idea to design a system monitor to show info chart...
---
Last a few days I've been allocated a project to show system info chart. The demands is to collect system info(including service's memory usage, CPU rate and thread counts) every a few minutes and show the info as a chart.

The theory is simple, run a job to collect system info at that time and record it in the database. Yet how to show them as chart is a little tricky. But let's first see what do we need to store in database. If our service is run in a distributed system where more than one server is running then we need to tell database which server's info are we storing. So if we design a table named "service_info", it should contain id, server_id, cpu_rate, mem_used, thread_count and timestamp. And when we want to see the info from time T1 to time T2, we just search the database and get all results.

That's it, isn't it?

Not really, because if you record it too frequently(like once per second), the size of database increases quickly and the size of data from API would be large. And even though storing a lot of data in databse is no harmful, is it clever to response everything you had?(What if I'm looking the trend of 30-days-info, and you give me more than 10k rows of records? Do I really need that?)

Take a break and consider this: when we talk about the info-chart, what do we need from it? If we are looking at a large range of window(like 15-days/30-days/one-week), we just want to see the trend. If my chart has no more than 60 dots, in a 30-day range each dot represents 12 hours, or 1 minute in a one-hour range. Not every chart is the same but this helps you to decide how often do you record your data in database.

So if you followed my advise and record data every 1 minute, you have to reduce the size of data when you are seeing 30-days info. How do you do that? 

What I came out is to keep your records in different tables. For example, we create 3 tables 'sys_info_hour' H, 'sys_info_day' D, and 'sys_info_week' W. Table H records data of every minute, table D records data of every hour and table W records data of every day. So if you are looking for range of 1 hour, server searches table H and if you are looking for range of one week, server searches table W.

It seems good. It reduces the calculates and the logic is clear. Is there anything that needs to worry?

Well, collecting one record and use it to represent the whole does not relfect the fact. Think about this case: the server crashes at 23:30 and restart at 23:45. In 00:00 you collect and record in table D and you cannot see the crash info in the 'one-day' chart. You thought everything's fine, but it's not. So can we calculate the average value before recording it into the database?

Yes you can, but it still cannot represent the fact. Let's say in the first half hour the server crashed and in the second half it runs in a heavy load(let's say the max memory is 1GB and the service used 950MB). After calculation the average value is 450MB, which is normal, you would not know what's going wrong unless you see the chart hour by hour.

So how can we avoid that?

We can do some check before returning data to client. We already know the number of records that in database, because we know how often we run the job to write records into database. For example, if server ask database the data of last 24 hours, and the job collects and records once a minute, we know database should store 24 * 60 = 1440 records. So if database only return us 700 records, what does this mean? Of course it means system failed to record, and the only reason is that system was crashed. In this situation, calculating average value would not reflect the fact, so we just return 0. As to the chart, it shows a drop, or drops. That's enough.

But also it requires extra calculation. What we do about it?

Well, the system design is about trade-off. Yes it would be relatively slow, but take a look at the senario, who will see the system monitor info chart? Genarally the administrator, is it? If it's the admin, it's probably that there's only one request at a time, so even the server returns slower(about 0.5 second slower?) what's the matter?

Finally in most cases data older than 1 month are useless, so we can regularly delete or move to backup tables.