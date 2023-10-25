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

no such thing as private methods/fields in python, but if you prefix it with two (2) underscores, it's basically the same thing:

```
$ cat private_methods.py 
class Dog:
	def __init__(self, name):
		self.__name = name
	def getName(self):
		return self.__name
def main():
	d = Dog('spike')
	#print(d.__name)
	print(d.getName())
if __name__ == "__main__":
	main()
```
notice that `print(d.__name)` doesn't work which is why it's commented out.
