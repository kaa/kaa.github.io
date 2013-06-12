--- 
tags: 
title: On .config files and NUnit, once more
layout: post
---
<p>Everytime I need to provide my test cases with a .config file my brain freezes over and I completely forget how I did it last time. Of course I <a href="http://www.google.com/search?q=nunit%20solution%20config%20files">google for &#8220;nunit solution config files&#8221;</a> and I get lots of <a href="http://nunit.com/blogs/?p=9">unhelpful answers</a>. In particular the suggestion that &#8230;</p>
<blockquote>If you use NUnit’s Visual Studio support to load a Visual Studio project or solution, such as mytests.csproj or mytests.sln, the config file must be named mytests.config.</blockquote>
<p>&#8230; which is plain wrong. So I move around the .config file with all possible combinations of paths and assembly names to no avail. Finally I fire up trusty old <a href="http://technet.microsoft.com/en-us/sysinternals/bb896645.aspx">Process Monitor</a> and of course find out that what NUnit is <i>REALLY</i> looking for is <i>mytests.sln.config</i> . From there it&#8217;s a slap on the head and plain sailing.</p>
