--- 
tags: 
title: Anonymous clone, authenticated push with Git over HTTP
layout: post
---
<p>It seems that you can&#8217;t configure Apache to support the anonymous-clone authenticated-push scenario over the http-backend.</p>
<p><a href="http://git.661346.n2.nabble.com/git-http-backend-and-Authenticated-Pushes-tp4703506p4703506.html"><a href="http://git.661346.n2.nabble.com/git-http-backend-and-Authenticated-Pushes-tp4703506p4703506.html">http://git.661346.n2.nabble.com/git-http-backend-and-Authenticated-Pushes-tp4703506p4703506.html</a></a></p>
<p>Fortunately the situations where I need anonymous access are rather limited (<a href="http://youtrack.jetbrains.net/issue/TW-11410">TeamCity does not support authentication over http for git</a>). I was able to solve it by serving repositories without authentication under a different url like so:</p>
<pre><code>ScriptAlias /git/ &#8220;C:/Progra~1/Git/libexec/git-core/git-http-backend.exe/&#8221;<br/>ScriptAlias /_git/ &#8220;C:/Progra~1/Git/libexec/git-core/git-http-backend.exe/&#8221;</code></pre>
This after figuring out <a href="http://therightstuff.de/CommentView,guid,2e19b03d-6ee1-49c1-b8c9-da5f3db7826f.aspx">how to make TeamCity accept our selfsigned certificate</a>.
