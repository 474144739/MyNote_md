# Map概述
`Map`集合和`Collection`集合的存储方式不同：
![image](https://raw.githubusercontent.com/474144739/image/master/YnoteImg/Collection%E4%B8%8EMap.bmp)

* `Collection`中的集合，元素是孤立存在的（理解为单身），向集合中存储元素采用一个个元素的方式存储。
* `Map`中的集合，元素是成对存在的(理解为夫妻)。每个元素由键与值两部分组成，通过键可以找对所对应的值。
* `Collection`中的集合称为单列集合，`Map`中的集合称为双列集合。
* 需要注意的是，`Map`中的集合不能包含重复的键，值可以重复；每个键只能对应一个值。

# Map常用的子类

* **HashMap<K,V>**：存储数据采用的哈希表结构，元素的存取顺序不能保证一致。由于要保证键的唯一、不重复，需要重写键的hashCode()方法、equals()方法。
* **LinkedHashMap<K,V>**：HashMap下有个子类LinkedHashMap，存储数据采用的哈希表结构+链表结构。通过链表结构可以保证元素的存取顺序一致；通过哈希表结构可以保证的键的唯一、不重复，需要重写键的hashCode()方法、equals()方法。

>tips：Map接口中的集合都有两个泛型变量<K,V>,在使用时，要为两个泛型变量赋予数据类型。两个泛型变量<K,V>的数据类型可以相同，也可以不同。

`java.util.HashMap<k, v>`集合继承了`Map<k, v>`接口

**HashMap特点**:
1. HashMap集合底层是哈希表：查询速度特别快。JDK1.8之前是数组和单向链表组成，JDK1.8以后由数组和单向链表/红黑树组成(链表长度超过8)，提高查询速度。
2. HashMap集合是一个无序的集合，存储元素和取出元素的顺序有可能不一致

`java.util.LinkedHashMap<k, v>`集合继承了`Map<k, v>`接口

**LinkedHashMap特点**：
1. LinkedHashMap集合底层是哈希表和链表(保证了元素有序)
2. LinkedHashMap集合是一个有序的集合，存储元素和取出元素的顺序是一致的

# Map接口中常用的方法

方法|描述|返回值
---|---|---
V put(K key, V value)|把指定的键和值添加到Map集合中|返回值:v 存储键值对的时候。key不重复，返回值v是null。key重复则替换map中的的value，返回被替换的value值。
V remove(Object key)|把指定的键所对应的键值对元素，在Map集合中删除，返回被删除元素的值|返回值:v 存储键值对的时候。key不重复，返回值是null。key重复，会使用新的value替换map中重复的value，返回被替换的value值。
V get(Object key)|根据指定的键，在Map集合中获取对应的值|返回值:v key存在，返回对应的值。key不存在，返回null。
boolean containsKey(Object key)|判断集合中是否包含指定的键|包含返回true，不包含返回false。
```java
package com.yuer.demo05.Map;

import java.util.HashMap;
import java.util.Map;

public class Demo01Map {
    public static void main(String[] args) {
        show01();
        show02();
        show03();
        show04();
    }

    private static void show01() {
        Map<String, String> map = new HashMap<>();
        String v1 = map.put("李晨", "范冰冰1");
        System.out.println("v1:" + v1);//null

        String v2 = map.put("李晨", "范冰冰2");
        System.out.println("v2:" + v2);//范冰冰1

        System.out.println(map);//{李晨=范冰冰2}

        map.put("冷锋", "龙小云");
        map.put("杨过", "小龙女");
        map.put("尹志平", "小龙女");
        System.out.println(map);//{杨过=小龙女, 尹志平=小龙女, 李晨=范冰冰2, 冷锋=龙小云}
        System.out.println("------------");
    }

    private static void show02() {
        Map<String, Integer> map = new HashMap<>();
        map.put("赵丽颖", 168);
        map.put("杨颖", 165);
        map.put("林志玲", 178);
        System.out.println(map);//{林志玲=178, 赵丽颖=168, 杨颖=165}

        Integer v1 = map.remove("林志玲");
        System.out.println(v1);//178
        System.out.println(map);//{赵丽颖=168, 杨颖=165}

        Integer v2 = map.remove("林志颖");
        System.out.println(v2);//null
        System.out.println(map);//{赵丽颖=168, 杨颖=165}
        System.out.println("------------");
    }

    private static void show03() {
        Map<String, Integer> map = new HashMap<>();
        map.put("赵丽颖", 168);
        map.put("杨颖", 165);
        map.put("林志玲", 178);

        Integer v1 = map.get("杨颖");
        System.out.println(v1);//165
        Integer v2 = map.get("林志颖");
        System.out.println(v2);//null
        System.out.println("------------");
    }

    private static void show04(){
        Map<String, Integer> map = new HashMap<>();
        map.put("赵丽颖", 168);
        map.put("杨颖", 165);
        map.put("林志玲", 178);

        boolean b1 = map.containsKey("赵丽颖");
        System.out.println(b1);//true

        boolean b2 = map.containsKey("林志颖");
        System.out.println(b2);//false
    }
}

```
# Map集合通过键找值的方式遍历Map集合

实现步骤：
1. 使用Map集合中的方法keySet()，把Map集合中所有key取出来，存储到一个Set集合中
2. 遍历Set集合，获取Map集合中的每一个key，
3. 通过Map集合中的方法get(key)，通过key找到value

方法|描述
---|---
Set<k> keySet()|返回此映射中包含的键的Set视图

```java
package com.yuer.demo05.Map;

import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;

public class Demo02KeySet {
    public static void main(String[] args) {
        //创建Map集合对象
        Map<String, Integer> map = new HashMap<>();
        map.put("赵丽颖", 168);
        map.put("杨颖", 178);
        map.put("林志玲", 158);
        //1. 使用Map集合中的方法keySet(),把集合中所有的key取出来，存储到一个Set集合中
        Set<String> set = map.keySet();
        //遍历set集合
        Iterator<String> it = set.iterator();
        while (it.hasNext()){
            String key = it.next();
            //通过Map中每一个key找到value
            Integer value = map.get(key);
            System.out.println(key + ":" + value);

        }
    }
}
```

# Entry键值对对象与Map集合的遍历
`Map.Entry<K, v>`：在Map接口中有一个内部接口Entry。当Map集合一创建，那么就会在Map集合中创建一个Entry对象，用来记录键与值(键值对对象，键与值的映射关系)

方法|描述
---|---
Set<Map.Entry<K, V>>entrySet()|返回此映射中包含的映射关系的Set视图
实现步骤：
1. 使用Map集合中的方法entrySet()，把Map集合中的多个Entry对象取出来，存储到一个Set集合中
2. 遍历Set集合，获取每一个Entry对象
3. 使用Entry对象中的方法getKey()和getValue()获取键与值

```java
package com.yuer.demo05.Map;

import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;

public class Demo03EntrySet {
    public static void main(String[] args) {
        //创建Map集合对象
        Map<String, Integer> map = new HashMap<>();
        map.put("赵丽颖", 168);
        map.put("杨颖", 178);
        map.put("林志玲", 158);
        //1. 使用Map集合中的方法entrySet()，把Map集合中的多个Entry对象取出来，存储到一个Set集合中
        Set<Map.Entry<String, Integer>> set = map.entrySet();

        //2. 遍历Set集合，获取每一个Entry对象
        Iterator<Map.Entry<String, Integer>> it = set.iterator();
        while (it.hasNext()) {
            Map.Entry<String, Integer> entry = it.next();
            //3. 使用Entry对象中的方法getKey()和getValue()获取键与值
            String key = entry.getKey();
            Integer value = entry.getValue();
            System.out.println(key + ":" + value);
        }

    }
}
```


