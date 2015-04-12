---
layout: post
title:  "Tomcat 7, Appache and Windows 2012"
date:   2015-01-20 18:30:46
categories: work
---
This is my colleague’s experience. Let’s call him J. 

One day he got his client’s call, complaining about the issue that he had encountered after upgrading tomcat from 6 to 7.

J, in the first time, wasn’t serious on that, since he had done it(upgrading tomcat from 6 to 7) himself, and all he did was copying some files and everything went well. But this time the client didn’t let him go. J had to do on-line chatting with him to help.

Funny things happened. J went through all the upgrading steps, but this time the upgrading failed. Error showed that something went wrong about the certificate of the agent, which cause the SSO failed. 

“But why I made it while this failed?” J wondered. He did all he can do but still, it went wrong. He apologised to the client and continued to find the source.

He tried to reproduce the issue by testing on his workstation, however, it worked fine under every conditions. At last he came with the idea, "Is it possible that the bug is related to the OS and tomcat instead of our software?" 

He was right. This issue happened when tomcat 7 and Windows server 2012 R2 are implemented simultaneously. But why? J read the logs sent from the client, from where he saw that the error is "Peer Not Authenticated" error.
What's this mean? This exception tells that connection made to server URL is not from an authenticated client. By doing the Wireshark, he had an idea: "Is this because the protocol was too old?" At that time, the protocol used by Appache was SSL. J updated the httpclient to the newest version, which used TSL, and that solved the issue.