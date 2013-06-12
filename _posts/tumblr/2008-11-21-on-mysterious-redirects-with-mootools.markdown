--- 
tags: 
title: ...on mysterious redirects with MooTools
layout: post
---
<p>While developing some javascript classes with MooTools&#8217; <code>new Class()</code> pattern I found myself being mysteriously redirected from <code><a href="http://www.example.com/test.html">http://www.example.com/test.html</a></code> to <code><a href="http://www.example.com/">http://www.example.com/</a>[object Object]</code> every time I opened the page.</p>
<p>Turns out instead of saying</p>
<blockquote><code>var C = new Class({...});<br/>var c = new C();</code></blockquote>
<p>I had, forgotten the second <code>new</code> and written</p>
<blockquote><code>var C = new Class({...});<br/>var c = C();</code></blockquote>
<p>I can&#8217;t explain the redirect, but once I added the missing <code>new</code> keyword everything was back to normal. This happened in a much more convoluted situation which made the missing keyword harder to spot. I guess it goes to show that you shouldn&#8217;t try to be clever, even when &#8220;just writing an unit test&#8221;.</p>
