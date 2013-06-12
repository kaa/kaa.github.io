--- 
tags: 
title: Custom ResourceLoaders in NVelocity
layout: post
---
<p>I needed a custom resource loader for NVelocity to enable me to unit test my new NVelocityView for the ASP.NET MVC app I was building. I studied some examples and went to work. Easy-peasy, I soon had a MemoryResourceLoader that loaded that looked something like this:</p>
<pre><code>public class MemoryResourceLoader : ResourceLoader {
  static IDictionary templates = new Dictionary();
  public static IDictionary Templates { get { return templates; } }
  public static void AddTemplate(string name, string template) {
    templates[name] = Encoding.UTF8.GetBytes(template);
  } 		<br/>  public override Stream GetResourceStream(string template) {
    byte[] templateBytes;
    if(!templates.TryGetValue( template, out templateBytes)) {
      throw new ResourceNotFoundException(<br/>        "MemoryResourceLoader Error: cannot find resource " + template);
    }
    return new MemoryStream(templateBytes);
  }
  public override void Init(ExtendedProperties configuration) {}
  public override long GetLastModified(Resource resource) {<br/>    return 0; <br/>  }
  public override bool IsSourceModified(Resource resource) { <br/>    return false; <br/>  }
}
</code></pre>
<p>Which I tried to get NVelocity to use by going:</p>
<pre><code>VelocityEngine engine = new VelocityEngine();<br/>ExtendedProperties properties = new ExtendedProperties();<br/>properties.AddProperty("resource.loader", "memory");<br/>properties.AddProperty(<br/>  "memory.resource.loader.class",<br/>  "NVelocityView.MemoryResourceLoader;NVelocityView");<br/>engine.Init(properties);<br/></code></pre>
<p>But no! All I got was:</p>
<pre><code>Problem initializing template loader: NVelocityView.MemoryResourceLoader
Error is: System.ArgumentNullException: Value cannot be null.
Parameter name: type</code></pre>
<p>I threw Process Monitor at it trying to figure out if it was looking for my assembly in a bisarre place, I tried fuslogvw.exe to see if there was a problem loading the assembly, I debugged and couldn&#8217;t find a solution. Until I decompiled with Reflector and noticed the somewhat strange line</p>
<pre><code>loaderClassName = loaderClassName.Replace(';', ','); </code></pre>
<p>in the decompiled source. Why would someone use <code>';'</code> in a type reference? Someone porting from Java of course with some strange configuration framework that uses <code>','</code> as a separator between values of course! As soon as I switched <code>','</code> to <code>';'</code> in the initialization code it was all dandy again.</p>
