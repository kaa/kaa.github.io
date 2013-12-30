--- 
tags: 
title: Using SQLite with NHibernate on 64 bit Windows
layout: post
---
<p>If you encounter an exception like this when initializing the ISessionFactory under FluentNHibernate with a 64 bit version of Windows</p>
<code>NHibernate.HibernateException: The IDbCommand and IDbConnection implementation in the assembly System.Data.SQLite could not be found. Ensure that the assembly System.Data.SQLite is located in the application directory or in the Global Assembly Cache. If the assembly is in the GAC, use &lt;qualifyAssembly/&gt; element in the application configuration file to specify the full name of the assembly.</code>
<p>Then you should probably ensure that the project is compiled for &#8220;x86&#8221; instead of &#8220;Any&#8221; in the project properties dialog.</p>
