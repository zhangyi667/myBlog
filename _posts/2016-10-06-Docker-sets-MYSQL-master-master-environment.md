---
layout: post
title:  "Memo of Docker setting MYSQL master-master environment"
date:   2016-10-06 18:30:46
categories: work
shortcut: A memo about how I set up MYSQL master-master env by Docker...
---
This is just a memo about how to set MYSQL master-master environment and the tips of doing that.

In order to make the database service scalable and reduce the pressure of reading, a lot of companies use master-slave or master-master configuration for their database service. The theory of sync between databases is the replication, which listens the change of bin-log of MYSQL and replicate the data.

The normal step to set up a MYSQL master-master or master-slave environment is like:

1.Install MYSQL on both machines. Create an account for replication, which should have at least the premission of SLAVE and REPLICATION.

2.If you already had a MYSQL service running, you should lock it before copying data to the other machine.

3.Modify the my.cnf on both side, activate "bin-log", set server-id, master-host and master-user.

4.Sometimes you have to restart service to make it work.

In my case I used Docker to monitor all the environment, which means I can run MYSQL service in containers to save time. But still, it didn't go well when setting them up.

What I did is to use docker-compose to run two mysql containers together. The conf file is a yaml file, where you define all the MYSQL conf and when you start the compose it should all be fine. However, things happened.

1.You are supposed to define the replicator user info in the .yml file so when the compose starts, MYSQL can create the user according to them. However it didn't. So we have to write a .sql file which runs the first time the MYSQL service runs. In the file we have this user creating script.

2.If you gonna use docker-compose, you have to give it permission to read and write by command "chron", which we didn't at the first time. The log shows when MYSQL service started, it tried to read .sql file but failed.

3.When master-master environment is set, both MYSQL services read the .sql file and tried to create the replicator user. Since they have the same name, there is always an sql error "User already exists.". This is confusing since the user is in database "mysql", which is already been set ignored by .yml file. Finally I have to use "Reset master" to reset the bin-log to solve that. It was not smart, I know.