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

Display the elements of a list as a space-separated String:

```
$ cat Demo0002.java 
import java.util.stream.Collectors;
import java.util.*;

public class Demo0002 {
  public static void main(String[] args) {
    List<Integer> values = new ArrayList<>();
    values.add(2);
    values.add(4);
    values.add(6);
    values.add(8);
    System.out.println("Using default toString():");
    System.out.println(values);
    System.out.println("Using stream with Collectors.joining:");
    System.out.println(values.stream().map(Object::toString).collect(Collectors.joining(" ")));
  }
}
$ java Demo0002
Using default toString():
[2, 4, 6, 8]
Using stream with Collectors.joining:
2 4 6 8
```

Count the number of characters in a String:

```
$ cat Demo0003.java 
public class Demo0003 {
  public static void main(String[] args) {
    if (args.length != 2) {
      System.out.println("Usage: java Demo0003 <string> <char>");
      System.exit(0);
    }
    String s = args[0];
    char c = args[1].charAt(0);
    long i = s.chars().filter(ch -> ch == c).count();
    System.out.printf("There are %d occurences of the character '%s' in the string \"%s\"\n", i, c, s);
  }
}
$ java Demo0003 "This is a dog. Woof!" o
There are 3 occurences of the character 'o' in the string "This is a dog. Woof!"
```

Count the number of all characters in a String and store the results in a Map:

```
$ cat Demo0004.java 
import java.util.Map;
import java.util.stream.Collectors;
import java.util.function.Function;

public class Demo0004 {
  public static void main(String[] args) {
    if (args.length != 1) {
      System.out.println("Usage: java Demo0003 <string>");
      System.exit(0);
    }
    String s = args[0];
    Map<Character,Long> charMap = s.chars().mapToObj(c -> (char) c).collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    charMap.forEach((k, v) -> System.out.printf("%s: %d\n", k, v));
  }
}
$ java Demo0004 "This is a dog. Woof!"
 : 4
!: 1
a: 1
s: 2
d: 1
T: 1
f: 1
W: 1
g: 1
h: 1
i: 2
.: 1
o: 3
```

Summing up all the numbers in a Set (or List):

```
Set<Integer> set = new HashSet<>();
set.add(5);
// ...add more numbers...
set.add(25);
int sum = set.stream().mapToInt(Integer::intValue).sum();
```

Convert a long to String:

```
long l = 12345L;
String s = Long.toString(l);
```

Convert a character to the number it represents (NOT convert it to its char value):

```
String s = "12345";
int i = Character.getNumericValue(s.charAt(1)); // i=2
```

Convert double to long:

```
double d = 1234.5678;
long l = Double.valueOf(d).longValue();
```

The way to count the occurences of `int`s in an `int` array:
```
public static Map<Integer, Long> getFrequencyMap(int[] numbers) {
        return Arrays.stream(numbers) // Use Java 8 stream
                .boxed() // convert IntStream to Stream<Integer>
                .collect(Collectors.groupingBy(
                        Function.identity(), // Key - the number
                        Collectors.counting() // Value - occurrences of the number
                ));
    }
```

Get the key of the max value in a `Map`:
```
Map<Integer,Long> map = new HashMap<>();
...
Collections.max(map.entrySet(), Map.Entry.comparingByValue()).getKey();
```

Convert a `List` of `Integer`s into a `String`:
```
List<Integer> list = new ArrayList<>();
...
String s = list.stream().map(String::valueOf).collect(Collectors.joining());
```

Convert a binary String to the `int` equivalent:
```
String s = "01011101";
int i = Integer.parseInt(s, 2);
```

Remove all non-alphanumeric characters from String:
```
String s = "...";
s = s.replaceAll("[^a-zA-Z0-9]", "");
```

Sorting by multiple fields:
```
class Student {
  int id;
  String name;
  double gpa;
  public int getId() { return id; }
  public String getName() { return name; }
  publib double getGpa() { return id; }
}
...
  public static void main(String[] args) {
    List<Student> students = new ArrayList<>();
    ...
    Collections.sort(students, Comparator.comparing(Student::getGpa, Comparator.reverseOrder()).thenComparing(Comparator.comparing(Student::getName)));
    ...
  }
...
```

Convert `int` to bit string, then reverse it, and convert back to `int`:
```
int i = ...;
String s = Integer.toBinaryString(i);
StringBuilder sb = new StringBuilder(s);
sb = sb.reverse();
i = Integer.parseInt(sb.toString(), 2);
```

Zero pad a binary string:
```
int i = ...;
BigInteger bi = new BigInteger(Integer.toBinaryString(i));
String s = String.format("%032d", bi); // display value as 32-bits
```
