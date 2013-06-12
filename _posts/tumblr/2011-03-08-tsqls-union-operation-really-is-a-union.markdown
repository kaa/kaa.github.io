--- 
tags: 
title: TSQL's UNION operation really is a UNION
layout: post
---
<p>This one caught me a bit off guard. It turns out UNION in TSQL really is <a href="http://en.wikipedia.org/wiki/Union_(set_theory)">the mathematical union</a> of the two result sets not just the concatenation of the involved results.</p>
<p>Include the keyword ALL to ensure duplicates are also returned.</p>
<blockquote><dt>ALL</dt><dd>
<p>Incorporates all rows into the results. This includes duplicates. If not specified, duplicate rows are removed.</p>
</dd></blockquote>
<dd>
<p>- <a href="http://msdn.microsoft.com/en-us/library/ms180026.aspx">Transact-SQL reference</a></p>
</dd>
