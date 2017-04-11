---
layout: post
title:  "Why Unit test is important"
date:   2016-03-08 18:30:46
categories: work
shortcut: As a back end developer, do you do unit test?
---
Why I think every programmer should write unit test by themselves.

It's been a few months since I started to use Spock, a test framework. It takes some time to fit in, and after that TDD has never been that smoothie.

For some programmer, once it comes to unit test, it is JUnit or testng, and that’s it. Writing a few unit test cases and you think you have mastered it, then you complain how simple it is, just need some time to write test cases.

BUT IT'S NOT.

Before getting into this topic, I have to say that a lot of my colleagues have no idea what a unit test is, how to do unit test or what can unit test do. I know some companies have QA departments and you don't have to do unit test on your own, but I think it's a shame for a developer to avoid learning new things, especially things that in fact are important to you.

So what is unit test for?

It seems clear enough: it helps check if a block of code works correctly. But have you ask yourself this question: Is unit test needed for every situation? For example you need to develop a service called MessageService, which sends emails to user. I bet 10 pounds that the way you do test is to run it in QA and actually tries to send an email to a QA user. You’ve got your reason: Writing unit test requires other dependencies, which affects the independency of unit test; Unit test cannot easily monitor the case where the email server is down; The email server itself cannot guarantee 100% availability, which affects the reliability of unit test, etc.
(If you can’t solve it using JUnit, Spock is what you are looking for. Using the Mock feature, it ensures the reliability and availability. )

From my experience, writing unit test is not easy. It might be even more difficult than writing code. Think about the scenarios of using unit test:
1. TDD. Your QA colleague has written complete unit test before developing, which has explained all system feature demands.
2. You have to refactor your code. Unit test is the way to check if the new code works the same with the old code.
3. You are developing something, and you write some test cases to verify if your code works as required.

Case 1 is in Utopian. Nobody does that. Case 2 is the standard way to do refactor, but ask yourself how many times you promise you’ll refactor your code and how many times you’ve actually done it? Case 3 is what happens in real world. But as I mentioned, unit test after development is of no use. 

By studying and applying Spock, I’ve found TDD is the way to guarantee the quality of code. What’s more important, it raises your design ability because the complex unit tests require good design, so you have to think hard before writing any code. 