---
layout: post
title:  "Update oracle in Grails"
date:   2015-02-20 18:30:46
categories: work
shortcut: What is the correct way to update an oracle database using Grails....
---
This is a story of how we are fixing an issue reported by our client.
Yesterday our client reported an issue that happened when he was doing the upgrade of our product. It displayed `no 'group.id’ found`. 
We traced the bug and found what was going wrong.

In our new version of product, we added a new feature called `person`, which has a many-to-many relationship with `group`, and that’s the reason we add a new table `person`, as well as column “group.id” in that table. So this table is what we guessed as the target. 
First we tried updating using MYSQL. It worked, and updating using ORACLE failed. Since our client using ORACLE, we narrowed down the issue.

Then we tried to reproduce the issue. According to the client’s description, we did the upgrade. And when we tried to upgrade the product, we found that this table, “persion”, was not there. In other words, table `person` was not created successfully. Good news, we narrowed down the issue. 
So how did this happen? 

First we thought if there’s anything wrong with the reserved words since ORACLE was more sensitive to a whole lot of words and in the `person` table we have attributes like `something_something`, “something_definition”, “alert”, etc.  [Here][oracle_reserve] we can see all reserved words of ORACLE. Didn’t see anything wrong. Then we guess if the attribute name was too long. But from [here][attr_length] we know that the length of words should be no longer than 30 bytes, which in our case was not happening.

Then we focused on our conf file. Every time we do the upgrade, we write a change-log file, which was a config file made of some SQL that helps change table name, column, primary key, etc. just in case. Every single execution of the SQL has an id so that when database executed the SQL, we knew whether it worked or not. (REF: [how to find ORACLE executed history][how_to])

From the log we found that one SQL failed to execute, which led to the failure of the table creation. And then we had another progress: If the database was a new one, our new version of product worked fluently. However, if it had already existed, the issue happened. 
But why? We didn’t know.

Our first guess was the upgrade of the database has gone wrong, because, after all, when it comes to the upgrade of the database, issue happened.

Based on our thought, we looked at the trace again, and found the error description: `"Cannot find 'd_system', object may not exist"`. I know this "d_system", it is a table name. What this meant was that when hibernate tried to upgrade Oracle database, it looked for d_system but cannot found. Because in the database what we have was `“d_system”`--careful about the quotation marks. 
So it turned out to be the hibernate issue. The low-version hibernate we used had some bugs.

In Grails, the DataSource.groovy does the job that can decide which environment/database can be used, as well as whether to create a new database or upgrade the current one. 
`dbCreate="update"`
This means the database would be only updated when the service starts. And when the service starts, hibernate would call this file and get the settings to modify the database, which would cause the error. Good news is, that Grails has a mechanism for database-update, where with favour of liquiBase, we can write specific database update script, and when the server does the upgrade, it executes the script so that the bug of hibernate can be avoid.

[oracle_reserve]: http://docs.oracle.com/cd/B19306_01/em.102/b40103/app_oracle_reserved_words.htm
[attr_length]:http://stackoverflow.com/questions/18248318/change-table-column-index-names-size-in-oracle-11g-or-12c
[how_to]: http://geeks.vivavivu.com/2013/10/oracle-check-history-of-executed-queries.html