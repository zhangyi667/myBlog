---
layout: post
title:  "Filter Pattern"
date:   2015-01-23 18:30:46
categories: work
shortcut: his pattern is applied in a BUG-Fixing job. In the previous...
---
This pattern is applied in a BUG-Fixing job. In the previous version of our software, our server queries an LDAP server to retrieve all DISABLED users, which gives back an iterator that used to, of course, iterates all the DISABLED users.

However, something went wrong in the system. The LDAP server just returned the data of all users, instead of those DISABLED. The best way to solve the problem, of course, was to find where it went wrong and let LDAP server return the right result. But what if we just want to change as little code as we can, in the same time do it as fast as we can, and make sure it would work?

That's the scenario where the `Filter Pattern` is perfect to answer this question.

Notice: There is a very important pre-condition which is the prerequisite that the Filter Pattern can be applied. The Iterator returned from the LDAP server. Having noticed that even if we get the result(the iterator) from LDAP server correctly, we still need to loop it to get all the user data, only based on this very prerequisite can I applied this filter pattern to solve the problem. 

[Filter pattern][fp] is very simple but efficient handling such problem. I wrote a FilterIterator that implementing the Iterator, whose job is to “filter” all the data that doesn’t match the query when looping the Iterator from the LDAP server. In the end, even though the result from the LDAP server is still wrong, after looping the iterator, we still got the right result, and that half-hour work saved about 10-hours-refactoring-code-and-fixing-bug-work.

[fp]:http://www.tutorialspoint.com/design_pattern/filter_pattern.htm