--- 
tags: 
title: 6 reasons I chose MooTools as my javascript framework
layout: post
---
<p>Keeping the <a href="http://www.codinghorror.com/blog/archives/000916.html">Magpie Developer</a> in mind, I have decided to adopt a javascript framework. After mature deliberation I decided on MooTools, here are my reasonings:</p>
<ol>
	<li>It&#8217;s in active development</li>
	<li>It lets me maintain references to the real DOM elements, unlike <a href="http://www.jquery.org">JQuery</a>.</li>
	<li>I can call into the library when I need it, no need to <a href="http://www.mochikit.com/doc/html/MochiKit/index.html#notes">ask it not to clutter the namespace (MochiKit)</a> with hundreds of functions.</li>
	<li>I generally build my DOM from javascript (progressive enhancement) and so I already have references to the elements that I need. I dont need <a href="http://www.jquery.org">a query language (JQuery)</a> to find my elements.</li>
	<li>I am not looking to add glitz to my sites with a few lines of code .</li>
	<li>I don&#8217;t think Javascript needs to <a href="http://mochikit.com/">&#8220;suck less&#8221; (MochiKit)</a>.</li>
</ol>
<p>I should note that this is by no means final, hell I was half way writing this about <a href="http://www.prototypejs.org">PrototypeJS</a> before I dug deeper into MooTools and found it excellent.</p>
