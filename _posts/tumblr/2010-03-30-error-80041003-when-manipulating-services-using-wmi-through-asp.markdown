--- 
tags: 
title: Error 80041003 when manipulating Services using WMI through ASP
layout: post
---
<p>You can run into quite strange errors when attempting to access WMI though ASP, note that there are a few steps to remember for bliss.</p>
<ol><li>Ensure you give permissions for the user in question to access WMI. Computer -&gt; Manage -&gt; Configuration -&gt; WMI Control -&gt; Properties, in the Security tab ensure your user has permissions to the right namespaces (probably Root/CIMV2). The permission required is usually </li>
<li>Use either SC.exe or preferrably SetACL.exe to set access control lists for the relevant user on the service, for example:
<blockquote><code>SetACL.exe -on SERVICE_NAME -ot srv -actn ace -ace "n:USER_OR_GROUP;p:start_stop"</code></blockquote>
</li>
<li>Finally you need to enable impersonation to work, for that we need to pass the password as plaintext. To do this ensure you use Basic authentication instead of Windows Authentication.</li>
</ol>
