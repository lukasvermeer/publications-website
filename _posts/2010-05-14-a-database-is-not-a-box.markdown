---
layout: post
title: "a Database is not a Box"
date: 2010-05-14
tags: ["Database","Oracle"]
original:
  source_name: Wordpress
  source_url: https://lukasvermeer.wordpress.com/2010/05/14/a-database-is-not-a-box/
---

I've seen people using and building upon databases in lots of different ways; many of them surprisingly original. However, it seems to me that some people do not fully grasp the concept of a database. Using a database is clearly not the same as understanding how one works.

[caption id="attachment_77" align="alignleft" width="300" caption="Not a Database (image by Jared Tarbell)"][![Not a Database]({{site.baseurl}}{% link assets/2010-05-14-a-database-is-not-a-box-junk.jpg?w=300 %} "Not a Database (image by Jared Tarbell)")](http://www.flickr.com/photos/generated/3313311558/)[/caption]

I think we need to take a step back and consider the basic question "what is a database". You might think you know the answer. You might be one of those people who thinks the answer is not important as long as you know how to use it. I disagree, and I'm going to explain why. If you are one of those people, this article is for you.

**A database is not a box of data.**

A database is not a file drawer you put stuff in for later retrieval. A database is not a spreadsheet. A database is not a collection of arrays. Of course you know this, but many programmers forget this when they start coding. They treat the data in the database as if they need to manage it; as if it were their responsibility. 

Surprise! It's not.

[caption id="" align="alignright" width="500" caption="A better metaphor (image from The U.S. National Archives)"][![A better metaphor]({{site.baseurl}}{% link assets/2010-05-14-a-database-is-not-a-box-3930932575_e9798b89f4_d.jpg %} "A better metaphor (image from The U.S. National Archives)")](http://www.flickr.com/photos/usnationalarchives/3930932575/)[/caption]

A database is more like a library. Access to data (books) is the most important thing for a library; it is its reason for existence. And although the purpose of a library is to give you easy access to books, you are not expected to know exactly where each book is stored, nor are you expected to put them back on a shelve when you return them. You are not, in other words, the one who decides how the books are organized. That's the librarians job. 

**A database also has a librarian. We call her "the Optimizer".**

Databases, just like libraries, are built to facilitate fast and easy access to data. In later posts I will discuss some of the techniques used to improve performance. For now, let us assume that the database is quite different from a normal array or any other basic data structure. Just like a librarian in a library, the objective of the optimizer is thus to help you find what you are looking for faster. It does so by creating a plan to find the data every time you run a query.

As the Oracle9i Database Performance Tuning Guide and Reference puts it:

> The optimizer determines the most efficient way to execute a SQL statement after considering many factors related to the objects referenced and the conditions specified in the query. This determination is an important step in the processing of any SQL statement and can greatly affect execution time.

Every query you send to the database goes through this process. There is no way to go around it; and you should not want to. The trick is to work together _with_ the optimizer to get the best results.

I hope that working together will be easier now you know she exists. Next time, I will discuss some of the decisions the optimizer can take.

<span style="color:#bbb;">[This article is based on a presentation I prepared for [Ortec](http://www.ortec.com/), the company where I completed my Master thesis.]</span>