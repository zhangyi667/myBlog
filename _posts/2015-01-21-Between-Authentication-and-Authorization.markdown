---
layout: post
title:  "Between Authorization and Authentication"
date:   2015-01-21 18:30:46
categories: work
---

What's the different?

Authentication answers this question: "How can you prove that you are you?" Normally it's handled by password. 

Authorization answers this question: "What can you have?" Normally it's handled by token or access control.

For instance, we have a database system. It has some users. There's one administrator of this system, some VIP users and some normal users.

When using it, the authentication progress is when you enter your password. By doing the authentication, the server also do the authorization. It recognises users' level, and give them different permissions: administrator can reach the whole resource of the system, while VIP and normal users can only get limited resource. This is authorization. 

This [Essay][authorization] gives a good explanation.

[authorization]: https://technet.microsoft.com/en-us/library/cc512578.aspx