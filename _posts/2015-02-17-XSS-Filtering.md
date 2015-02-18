---
layout: post
title: Xss Filtering
published: true
---

Last a few days, I was trying to send regular expression which contains escape"\\" by json over http request. But 
I found server does allow me to do that and XssFilter throws an exception saying: INTRUSION: two encodings found. 

I looked at the code, the filter basically try to decode the http request using URLEncoder, EntityEncoder and JavaScriptEncoder. If any decoded text different from the original one, the encoding will be counted. As long as two encodings found, server will throw the expcetion, refuse the request.

Then I looked Xss-[Cross Site scripting](http://en.wikipedia.org/wiki/Cross-site_scripting). The basic idea is that the attacker can embed java script to the content of the request body or parameter. If there is not a Xss Filter, the server may accept the script as a content. The script may be run in another user's browser sending information to somewhere. Or by reflection, providing a malicious site link, if the user click will lead to execution of injected script. So basically, Xss attach is kind of code injection.

The Css based on Web underlying concept of trust know as the [same origin policy](http://en.wikipedia.org/wiki/Same-origin_policy). So if the a injected script get to run on a session with privilege, then it can obtain the information as having that privilege. So information can be get by hackers without privilege.

For instance, if I dont have any permission to view information from a table but can insert into it. And I insert some thing like:

{% highlight ruby linenos %}
blablabla
<script>
	send all information on the page to hacker server ip:port
</script>
blablabla
{% endhighlight %}

When this data get displayed on the priviledged user's browser.

{% highlight ruby linenos %}
<table>
	<tr>
		<td>
			blablabla
			<script>
				send all information on the page to hacker server ip:port
			</script>
			blablabla
		</td>
	</tr>
</table>
{% endhighlight %}

The user will see regular data but the script will run when the page loaded and send the information over to hacker's server

The the server may save this as a data content, and when this information displayed for priviledge user, the script section will send out all informations I should not be able to see to my hacker server.

So base on this idea, I don't want to skip the filter, or stop the check. I compromised to using wildcard matching instead of regular expression mapping.