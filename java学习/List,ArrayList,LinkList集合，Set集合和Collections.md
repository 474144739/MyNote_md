# List集合(继承了Collection接口)
java.util.List接口 extends Collection接口

List接口的特点：
1. 有序集合，存储元素和取出元素的顺序是一致的
2. 有索引，包含了一些带索引的方法
3. 允许存储重复的元素

List接口中带索引的方法（特有）：

方法|描述
---|---
void add(int index, E element)|将指定元素，添加到该集合中指定位置上
E get(int index)|返回集合中指定位置的元素
E remove(int index)|移除列表中指定位置的元素，返回的是被移除的元素
E set(int index, E element)|用指定元素替换集合中指定位置的元素，返回值是更新前的元素

>操作索引时一定注意防止越界异常。

```java
package com.yuer;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class Demo01List {
    public static void main(String[] args) {
    
        //创建一个List集合对象，多态
        List<String> list = new ArrayList<>();
        //使用add方法向集合中添加元素
        list.add("a");
        list.add("b");
        list.add("c");
        list.add("d");
        list.add("e");
        
        //打印集合
        System.out.println(list);//[a, b, c, d, e]
        
        //在c和d之间添加一个元素 void add(int index, E element)
        list.add(3, "lxy");
        System.out.println(list);//[a, b, c, lxy, d, e]
        
        //移除元素 E remove(int index)
        String removeE = list.remove(2);
        System.out.println("被移除元素：" + removeE);//被移除元素：c
        System.out.println(list);//[a, b, lxy, d, e]
        
        //把第一个a替换为A   E set(int index, E element)
        String setE = list.set(0, "A");
        System.out.println("被替换的元素：" + setE);//被替换的元素：a
        System.out.println(list);//[A, b, lxy, d, e]

        //List集合遍历有三种方式
        //使用普通for循环
        for (int i = 0; i < list.size(); i ++){
            //E get(int index) 返回集合中指定位置的元素
            System.out.println(list.get(i));
        }
        System.out.println("----------------");
        //使用迭代器遍历List
        Iterator<String> it = list.iterator();
        while (it.hasNext()){
            System.out.println(it.next());
        }
        System.out.println("----------------");
        //使用增强for循环
        for (String s : list) {
            System.out.println(s);
        }
    }
}

```

# List的子类

## 1. ArrayList集合

`java.util.ArrayList`集合数据存储结构是数组结构。元素增删慢，查找快。由于日常开发中是用最多的功能为查询数据、遍历数组、所以`ArrayList`是最常使用的集合。但是不提倡随意使用ArrayList完成任何需求。（不同步，多线程）

## 2. LinkedList集合
`java.util.LinkList`集合数据存储结构是链表结构。元素的查询慢，增删快。里面包含了大量操作首尾元素的方法，使用LinkList集合特有的方法，不能使用多态。


方法|描述
---|---
void addFirst(E e)|将指定元素插入此列表的开头
void addLast(E e)|将指定元素添加到此列表的结尾
void push(E e)|将元素推入此列表所表示的堆栈
E getFirst()|返回该列表的第一个元素
E getLast()|返回该列表的最后一个元素
E removeFirst()|移除并返回该列表的第一个元素
E removeLast()|移除并返回此列表的最后一个元素
boolean isEmpty()|如果列表中不包含元素，则返回true

LinkedList是List的子类，List中的方法LinkedList都是可以使用，我们只需要了解LinkedList的特有方法即可。在开发时，LinkedList集合也可以作为堆栈，队列的结构使用。(了解)
>addFirst和push方法一样，都是向列表头添加元素。addLast和add方法一样，都是向列表尾部添加元素。removeFirdt和pop方法一样，都是移除并返回列表的第一个元素。
```java
package com.yuer;

import java.util.LinkedList;

public class Demo02LinkList {
    public static void main(String[] args) {
        show01();
        show02();
        show03();
    }

    /*
     * void addFirst(E e)
     * void addLast(E e)
     * void push(E e)
     * */
    public static void show01() {
        //创建LinkList集合对象
        LinkedList<String> linked = new LinkedList<>();
        //使用add方法添加元素
        linked.add("a");
        linked.add("b");
        linked.add("c");
        System.out.println(linked);//[a, b, c]

        //addFirst
        linked.addFirst("x");
        System.out.println(linked);//[x, a, b, c]
        //addLast
        linked.addLast("y");
        System.out.println(linked);//[x, a, b, c, y]
        System.out.println("------------");
    }

    public static void show02() {
        //创建LinkList集合对象
        LinkedList<String> linked = new LinkedList<>();
        //使用add方法添加元素
        linked.add("a");
        linked.add("b");
        linked.add("c");

        String first = linked.getFirst();
        System.out.println(first);//a
        String last = linked.getLast();
        System.out.println(last);//c
        System.out.println("-----------");
    }

    public static void show03() {
        //创建LinkList集合对象
        LinkedList<String> linked = new LinkedList<>();
        //使用add方法添加元素
        linked.add("a");
        linked.add("b");
        linked.add("c");
        System.out.println(linked);//[a, b, c]

        String first = linked.removeFirst();
        System.out.println(first);
        String last = linked.removeLast();
        System.out.println(last);
        System.out.println(linked);
    }
}

```
## 3. Vector集合(了解)

也实现了List接口，和ArrayList集合一样，存储结构是数组结构。与新的Collection实现不同Vector是同步的(单线程,速度慢)，JDK 1.2版本后被淘汰。

# set接口(继承了Collection)
**特点：**
1. 不允许存储重复元素
2. 没有索引，没有带有索引的方法，也不能使用普通的for循环遍历

## HashSet
`java.util.HashSet`集合是`Set`接口的一个实现类

**特点:**
1. 不允许存储重复元素
2. 没有索引，没有带有索引的方法，也不能使用普通的for循环遍历
3. 是一个无序集合，存储元素和取出元素的顺序有可能不一致
4. 底层是一个哈希表结构(查询速度非常快)
```java
package com.yuer;

import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;

public class Demo01Set {
    public static void main(String[] args) {
        Set<Integer> set = new HashSet<>();
        //使用add方法向集合中添加元素
        set.add(1);
        set.add(3);
        set.add(2);
        set.add(1);
        System.out.println(set);//[1, 2, 3]
        //使用迭代器遍历set集合
        Iterator<Integer> it = set.iterator();
        while (it.hasNext()){
            System.out.println(it.next());
        }
        System.out.println("---------------");
        //使用增强for遍历set集合
        for (Integer integer : set) {
            System.out.println(integer);
        }
    }
}
```
## HashSet集合存储数据的结构(哈希表)
**哈希值**：是一个十进制的整数，由系统随机给出(就是对象的地址值，是一个逻辑地址，是模拟出来得到的地址，不是数据实际存储的物理地址)，在Object类中有一个方法，可以获取对象的哈希值。

**Object类**：

`public native int hashCode();`

`native`代表该方法调用的是本地操作系统的方法
方法|描述
---|---
int hashCode()|返回该对象的哈希码值

`Demo01HashCode.class`:
```java
package com.yuer.demo03.hashCode;

public class Demo01HashCode {
    public static void main(String[] args) {
        //Person类继承了Object类，所以可以使用Object类的hashCode方法
        Person p1 = new Person();
        int h1 = p1.hashCode();
        System.out.println(h1);//460141958

        Person p2 = new Person();
        int h2 = p2.hashCode();
        System.out.println(h2);//1163157884

        /*
           toString()方法的源码：
                 return getClass().getName + "@" + Integer.toHexString(hashCode());
        */
        System.out.println(p1);//com.yuer.demo03.hashCode.Person@1b6d3586
        System.out.println(p2);//com.yuer.demo03.hashCode.Person@4554617c
        System.out.println(p2 == p1);

        /*String的哈希值
        *     String类重写的hashCode方法
        * */

        /*
        *   String 类的哈希值
        *      String 类重写了Object类的hashCode方法
        * */
        String s1 = new String("abc");
        String s2 = new String("abc");
        System.out.println(s1.hashCode());
        System.out.println(s2.hashCode());
    }
}

```
`Person.class`:
```java
package com.yuer.demo03.hashCode;

public class Person {
    //重写hashCode方法
    @Override
    public int hashCode() {
        return 1;
    }
}
```

>**HashSet存储自定义类型元素**给HashSet中存放自定义类型元素时，需要重写对象中的hashCode和equals方法，建立自己的比较方式，才能保证HashSet集合中的对象唯一

## LinkedHashSet
`java.util.LinkedHashSet`集合 extends HashSet集合

LinkedHashSet集合特点：
底层是一个哈希表(数组+链表/红黑树)+链表：多了一条链表(记录元素的存储顺序)，保证元素有序，但不重复
```java
package com.yuer.demo02.Set;

import java.util.HashSet;
import java.util.LinkedHashSet;

public class Demo04LinkedHashSet {
    public static void main(String[] args) {
        HashSet<String> set = new HashSet<>();
        set.add("www");
        set.add("abc");
        set.add("it cast");
        System.out.println(set);//[abc, it cast, www]
        System.out.println("---------");
        LinkedHashSet<String> linked = new LinkedHashSet<>();
        linked.add("www");
        linked.add("abc");
        linked.add("it cast");
        System.out.println(linked);//[www, abc, it cast]
    }
}

```
## 可变参数


使用前提：当方法的参数列表数据类型已经确定，但是参数的个数不确定，就可以使用可变参数。

使用格式：定义方法时使用

`修饰符 返回值类型 方法名(数据类型...变量名){}`

可变参数的原理：

可变参数的底层就是一个数组，根据传递参数个数不同，会创建不同长度的数组，来存储这些参数
传递的参数的个数，可以是0个（不传递），1, 2, 3...多个。

可变参数的注意事项：
1. 一个方法的参数列表，只能有一个可变参数
2. 如果方法的参数有多个，那么可变参数必须写在参数列表的末尾

# Collections

`java.util.Collections`是集合工具类，用来对集合进行操作。

方法|描述
---|---
static <T> boolean addAll(Collection<T> c, T...elements)|向集合中添加一些元素。
static void shuffle(List<?> list)|打乱集合顺序

```java
package com.yuer.demo04.Collections;

import java.util.ArrayList;
import java.util.Collections;

public class Demo01Collections {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();

        //static <T> boolean addAll(Collection<T>, T...elements)向集合中添加一些元素。
        Collections.addAll(list, "a", "b", "c", "d", "e");
        System.out.println(list);//[a, b, c, d, e]

        //static void shuffle(List<?> list)打乱集合顺序
        Collections.shuffle(list);
        System.out.println(list);//[d, e, a, b, c]

    }
}
```
方法|描述
---|---
static <T> void sort (List <T> list)|将集合中的元素按照默认规则排序

```java
package com.yuer.demo04.Collections;

import java.util.ArrayList;
import java.util.Collections;

public class Demo02Sort {
    public static void main(String[] args) {
        ArrayList<Integer> list01 = new ArrayList<>();
        list01.add(1);
        list01.add(3);
        list01.add(2);
        System.out.println(list01);//[1, 3, 2]

        //static <T> void sort(List<T> list):将集合中的元素按照默认规则排序。
        Collections.sort(list01);//默认升序
        System.out.println(list01);//[1, 2, 3]

        ArrayList<String> list02 = new ArrayList<>();
        list02.add("a");
        list02.add("c");
        list02.add("b");
        System.out.println(list02);//[a, c, b]

        Collections.sort(list02);
        System.out.println(list02);//[a, b, c]

    }
}

```

注意：sort(List<T> list)适用前提是被排序的集合里边存储元素，必须实现Comparable，重写接口中的方法

**Comparable接口的排序规则**：自己(this) - 参数：升序；

`Person.class`
```java
public int compareTo(Person o){
    //return 0; //认为元素都是相同的
    //return this.getAge() - o.getAge();//升序
    return o.getAge() - this.getAge();//降序
}
```
方法|描述
---|---
static <T> void sort(List<T> list, Comparator <? super T> )|将集合中的元素按照指定规则排序

Comparator和Comparable的区别：
- Comparable:自己(this)和别人(参数)比较，自己需要实现Comparable接口，重写比较的规则compareTo方法
- Comparator:相当于找一个第三方裁判，比较两个

**Comparator排序规则**：o2 - o1 升序
