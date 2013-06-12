--- 
tags: 
title: ISeeYourMouthMovingButIDontUnderstandAWordYouAreSaying
layout: post
---
<p>Who the hell can read this sh*t?</p>
<pre><code>public void LoopThroughAllTypesAndRegisterForOpenGenericsOfType(Type openGenericInterface)</code></pre>
<p>or</p>
<pre><code>Type closedGenericInterfaceWithParameters = openGenericInterfaceType.MakeGenericType(closedGenericInterfaceWithoutParamerters.GetGenericArguments());</code></pre>
<p>From the source code to <a href="http://code.google.com/p/codecampserver/">CodeCampServer</a> specifically from <a href="http://code.google.com/p/codecampserver/source/browse/trunk/src/Infrastructure/DependencyRegistry.cs">/trunk/src/Infrastructure/DependencyRegistry.cs</a>. Please also note the typing error in the second declaration.</p>
<p>Is this what we get for relying on keyword completion (Intellisense) too much these days?</p>
