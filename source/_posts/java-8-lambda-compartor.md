title: JAVA 8 LAMBDA COMPARATOR 例子
date: 2015-12-11 14:17:28
tags:
- java8
- lambda
---
在这个例子中，我们将向您展示如何使用Java 8 Lambda表达式写一个Comparator排序列表。


### 首先来看一个例子：

``` java
Comparator<Developer> byName = new Comparator<Developer>() {
    @Override
    public int compare(Developer o1, Developer o2) {
      return o1.getName().compareTo(o2.getName());
    }
  };
```



### Lambda 方式实现

``` java
Comparator<Developer> byName = (Developer o1, Developer o2)->o1.getName().compareTo(o2.getName());
```
+ <!-- more -->
举个例子来比较一下开发者的年龄对。通常情况下，我们使用Collections.sort并实现一个匿名Comparator类：

``` bash
import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;
 
public class TestSorting {
 
  public static void main(String[] args) {
 
    List&lt;Developer&gt; listDevs = getDevelopers();
 
    System.out.println("Before Sort");
    for (Developer developer : listDevs) {
      System.out.println(developer);
    }
    
    //sort by age
    Collections.sort(listDevs, new Comparator&lt;Developer&gt;() {
      @Override
      public int compare(Developer o1, Developer o2) {
        return o1.getAge() - o2.getAge();
      }
    });
  
    System.out.println("After Sort");
    for (Developer developer : listDevs) {
      System.out.println(developer);
    }
    
  }
 
  private static List&lt;Developer&gt; getDevelopers() {
 
    List&lt;Developer&gt; result = new ArrayList&lt;Developer&gt;();
 
    result.add(new Developer("mkyong", new BigDecimal("70000"), 33));
    result.add(new Developer("alvin", new BigDecimal("80000"), 20));
    result.add(new Developer("jason", new BigDecimal("100000"), 10));
    result.add(new Developer("iris", new BigDecimal("170000"), 55));
    
    return result;
 
  }
  
}
```

输出

``` bash
Before Sort
Developer [name=mkyong, salary=70000, age=33]
Developer [name=alvin, salary=80000, age=20]
Developer [name=jason, salary=100000, age=10]
Developer [name=iris, salary=170000, age=55]

After Sort
Developer [name=jason, salary=100000, age=10]
Developer [name=alvin, salary=80000, age=20]
Developer [name=mkyong, salary=70000, age=33]
Developer [name=iris, salary=170000, age=55]
```

当排序规则发生变化时，我们只能通过实现另外一个Comparator类
``` java
//sort by age
Collections.sort(listDevs, new Comparator&lt;Developer&gt;() {
  @Override
  public int compare(Developer o1, Developer o2) {
    return o1.getAge() - o2.getAge();
  }
});
 
//sort by name	
Collections.sort(listDevs, new Comparator&lt;Developer&gt;() {
  @Override
  public int compare(Developer o1, Developer o2) {
    return o1.getName().compareTo(o2.getName());
  }
});
      
//sort by salary
Collections.sort(listDevs, new Comparator&lt;Developer&gt;() {
  @Override
  public int compare(Developer o1, Developer o2) {
    return o1.getSalary().compareTo(o2.getSalary());
  }
});
```
没错，它能很好的工作，但是想一想我们为了一行代码的改变而去重新实现一个类！
### Java 8?Lambda
在Java 8 中 List 接口已经直接支持了sort方法，不在需要Collections.sort了

``` java
   //List.sort() since Java 8
listDevs.sort(new Comparator&lt;Developer&gt;() {
  @Override
  public int compare(Developer o1, Developer o2) {
    return o2.getAge() - o1.getAge();
  }
});
```

Lambda表达式的例子
``` java
import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.List;

public class TestSorting {

  public static void main(String[] args) {

    List&lt;Developer&gt; listDevs = getDevelopers();
    
    System.out.println("Before Sort");
    for (Developer developer : listDevs) {
      System.out.println(developer);
    }
    
    System.out.println("After Sort");
    
    //lambda here!
    listDevs.sort((Developer o1, Developer o2)-&gt;o1.getAge()-o2.getAge());
  
    //java 8 only, lambda also, to print the List
    listDevs.forEach((developer)-&gt;System.out.println(developer));
  }

  private static List&lt;Developer&gt; getDevelopers() {

    List&lt;Developer&gt; result = new ArrayList&lt;Developer&gt;();

    result.add(new Developer("mkyong", new BigDecimal("70000"), 33));
    result.add(new Developer("alvin", new BigDecimal("80000"), 20));
    result.add(new Developer("jason", new BigDecimal("100000"), 10));
    result.add(new Developer("iris", new BigDecimal("170000"), 55));
    
    return result;

  }
  
}
```


输出
```java
Before Sort
Developer [name=mkyong, salary=70000, age=33]
Developer [name=alvin, salary=80000, age=20]
Developer [name=jason, salary=100000, age=10]
Developer [name=iris, salary=170000, age=55]

After Sort
Developer [name=jason, salary=100000, age=10]
Developer [name=alvin, salary=80000, age=20]
Developer [name=mkyong, salary=70000, age=33]
Developer [name=iris, salary=170000, age=55]
```

### 看看其他的Lambda例子
#### 按照年龄排序
```java
//sort by age
Collections.sort(listDevs, new Comparator&lt;Developer&gt;() {
  @Override
  public int compare(Developer o1, Developer o2) {
    return o1.getAge() - o2.getAge();
  }
});

//lambda
listDevs.sort((Developer o1, Developer o2)-&gt;o1.getAge()-o2.getAge());

//lambda, valid, parameter type is optional
listDevs.sort((o1, o2)-&gt;o1.getAge()-o2.getAge());
```

#### 按照名字排序
```java
//sort by name
  Collections.sort(listDevs, new Comparator&lt;Developer&gt;() {
    @Override
    public int compare(Developer o1, Developer o2) {
      return o1.getName().compareTo(o2.getName());
    }
  });
    
  //lambda
  listDevs.sort((Developer o1, Developer o2)-&gt;o1.getName().compareTo(o2.getName()));		
  
  //lambda
  listDevs.sort((o1, o2)-&gt;o1.getName().compareTo(o2.getName()));
```

#### 按照工资排序
```java
//sort by salary
Collections.sort(listDevs, new Comparator&lt;Developer&gt;() {
  @Override
  public int compare(Developer o1, Developer o2) {
    return o1.getSalary().compareTo(o2.getSalary());
  }
});				

//lambda
listDevs.sort((Developer o1, Developer o2)-&gt;o1.getSalary().compareTo(o2.getSalary()));

//lambda
listDevs.sort((o1, o2)-&gt;o1.getSalary().compareTo(o2.getSalary()));
```

#### 反向排序
Lambda 根据工资降序
```java
Comparator&lt;Developer&gt; salaryComparator = (o1, o2)-&gt;o1.getSalary().compareTo(o2.getSalary());
  listDevs.sort(salaryComparator);
```

输出
```java
Developer [name=mkyong, salary=70000, age=33]
Developer [name=alvin, salary=80000, age=20]
Developer [name=jason, salary=100000, age=10]
Developer [name=iris, salary=170000, age=55]
```

Lambda 根据工资升序

```java
Comparator&lt;Developer&gt; salaryComparator = (o1, o2)-&gt;o1.getSalary().compareTo(o2.getSalary());
listDevs.sort(salaryComparator.reversed());
````

输出
```bash
Developer [name=iris, salary=170000, age=55]
Developer [name=jason, salary=100000, age=10]
Developer [name=alvin, salary=80000, age=20]
Developer [name=mkyong, salary=70000, age=33]
```




