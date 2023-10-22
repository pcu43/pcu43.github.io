---
layout: page
title: "Python"
permalink: /python
---

{% highlight python %}
with open('dog.txt') as f:
	lines = f.readlines()
for line in lines:
	print(line.strip())
{% endhighlight %}
