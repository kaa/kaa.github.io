--- 
tags: 
title: On Sql Server 2008 Express in User Instance mode
layout: post
---
<p>When faced with</p>
<pre><code>Failed to generate a user instance of Sql Server due to a failure in starting the process for the user instance</code></pre>
<p>when connecting to Sql Server 2008 Express from Visual Studio, make sure the account running Sql Server has access to the following directory</p>
<pre><code>C:\Users\<i>&lt;UserName&gt;</i>\AppData\Local\Microsoft\Microsoft SQL Server Data</code></pre>
