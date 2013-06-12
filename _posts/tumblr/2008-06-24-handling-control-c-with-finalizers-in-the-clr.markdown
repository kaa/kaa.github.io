--- 
tags: 
title: Handling Control-C with finalizers in the CLR
layout: post
---
<p>Yesterday was spent debugging an online-banking payment verification service for one our our customers. After a while I found that my test data was running out even though it was supposed to start from the beginning with every round I ran the code. As it turns out the problem was that locks were not beeing freed even though they were supposed to be guaranteed by <code>try { } ... finally { }</code> blocks.</p>
<p>Anyway, so now I find out that <a href="http://geekswithblogs.net/akraus1/archive/2006/10/30/95435.aspx">finalizers are not called when exiting due to Control-C</a>, and a number of other reasons too. Fortunately there is a solution provided in the previous link, Jolly!</p>
