---
title: Format numbers and return it as a string.
description: Zoho Deluge function to format numbers and return it as a string.
image: assets/images/deluge.svg
categories:
- Zoho
- ZohoCreator
- Zoho Deluge
layout: post
keywords:
- Text format
- Regex
- Low code
- Zoho Deluge
- Zoho

---
## Regex used to replace

{% highlight javascript linenos %}
string Utilities.formatNumber(float decimal)
{
	if(decimal != null)
	{
		formattedString = decimal.round(2).toString().replaceAll("\.",",").replaceAll("(?<!\.\d)(?<=\d)(?=(?:\d\d\d)+\b)",".");
		return formattedString;
	}
	else
	{
		return decimal;
	}
}
{% endhighlight %}