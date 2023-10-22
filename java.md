---
layout: page
title: "Java"
permalink: /java
---

this is a new page with LANG set to C

{% highlight java %}
package org.fuzzier.demo;

public class Dog {
  public static void main(String[] args) {
    System.out.println("woof!");
  }
}
{% endhighlight %}

How cool is that.

Creating a new project using Maven:

```
mvn archetype:generate -DgroupId=org.fuzzier -DartifactId=demo -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

where `demo` is the name of the project and the folder that will get created as the root directory of the project. Don't know why it has to download do much dependencies though.
