--- 
tags: 
- programming
title: Solved a really serious mystery today
layout: post
---
<p>Our customer has a e-mail campaing system that sends out around 30.000 opt-in emails to its customers every now and then. Nothing spectacular but it handles keyword substitution, independent operation for long running tasks, some pretty wild cyrillic texts.</p>
<p>Recently though sending newsletters to customers with non ASCII characters in their names stopped working, something which did work before, the only recent changes were some added tracing instructions and exception handling.</p>
<p>Here&#8217;s why!</p>
{% highlight csharp %}
  using System.Net.Mail;
  using System.Reflection;
  using NUnit.Framework;
  
  namespace Tests {
    [TestFixture]
    public class TestMailMessage {
      [Test]
      public void TestSending() {
        string email = "test@example.com";
        string name = "Börje Bengtson";
        MailAddress a = new MailAddress(email,name);
        MailAddress b = new MailAddress(email,name);
        b.ToString();
        BindingFlags flags = BindingFlags.Instance |
                             BindingFlags.NonPublic |
                             BindingFlags.InvokeMethod;
        string encodedStringA = typeof(MailAddress)
          .InvokeMember("ToEncodedString",flags,null,a,null);
        string encodedStringB = typeof(MailAddress)
          .InvokeMember("ToEncodedString",flags,null,b,null);
        /** BOOM! **/
        Assert.AreEqual(encodedStringA, encodedStringB);
      }
    }
  }
{% endhighlight %}
<p>Calling the <code>ToString()</code> method before <code>ToEncodedString()</code> (which is called when sending the message) will pollute the <code>MailAddress</code> internal state with an unencoded version of the address that is returned as a cached version on subsequent calls to <code>ToEncodedString()</code>.</p>
<p>Turns out some of the added tracing code printed out the address of the receiver it was processing at the moment, triggering the bug in the framework.</p>
<p>Now, if I could only find somewhere to report this&#8230;</p>
