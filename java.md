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

# sorting lists

Sorting objects with a comparator is pretty easy:

```
    List<String> lines = new ArrayList<>();
    lines.add("dog");
    lines.add("cat");
    lines.add("rat");
    lines.add("pig");
    Collections.sort(lines);
    System.out.println(lines);
...
[cat, dog, pig, rat]
```

But sorting objects without a comparator doesn't compile:

```
class Dog {
  String name;
  int weight;
  public Dog(String name, int weight) {
    this.name = name;
    this.weight = weight;
  }
  public String getName() { return name; }
  public int getWeight() { return weight; }
  public String toString() {
    return String.format("{ \"name\": \"%s\", \"weight\": %d }", name, weight);
  }
}
...
    List<Dog> dogs = new ArrayList<>();
    dogs.add(new Dog("spike", 20));
    dogs.add(new Dog("fluffy", 15));
    dogs.add(new Dog("spot", 10));
    dogs.add(new Dog("comet", 30));
    Collections.sort(dogs);
    System.out.println(dogs);
```

But newer versions of java have the `Comparator.comparing` method to help make sorting easy, we just need to provide a method to be used to sort on:

```
    List<Dog> dogs = new ArrayList<>();
    dogs.add(new Dog("spike", 20));
    dogs.add(new Dog("fluffy", 15));
    dogs.add(new Dog("spot", 10));
    dogs.add(new Dog("comet", 30));
    //Collections.sort(dogs);
    Collections.sort(dogs, Comparator.comparing(Dog::getName));
    System.out.println(dogs);
...
[{ "name": "comet", "weight": 30 }, { "name": "fluffy", "weight": 15 }, { "name": "spike", "weight": 20 }, { "name": "spot", "weight": 10 }]
```

And if we want to sort on multiple fields, we can do that too:

```
    List<Dog> dogs = new ArrayList<>();
    dogs.add(new Dog("spike", 20));
    dogs.add(new Dog("fluffy", 15));
    dogs.add(new Dog("spot", 10));
    dogs.add(new Dog("comet", 30));
    dogs.add(new Dog("biscuit", 15));
    Collections.sort(dogs, Comparator.comparing(Dog::getWeight).thenComparing(Dog::getName));
    System.out.println(dogs);
...
[{ "name": "spot", "weight": 10 }, { "name": "biscuit", "weight": 15 }, { "name": "fluffy", "weight": 15 }, { "name": "spike", "weight": 20 }, { "name": "comet", "weight": 30 }]
```

