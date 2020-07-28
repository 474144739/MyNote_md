# Collection集合的方法

java.util.Colection接口是所有单列集合最顶层的接口，里面定义了所有单列集合共性的方法，任意的单列集合都可以使用Collection接口中的方法。

方法 | 描述
---|---
boolean add(E e) | 向集合中添加元素
boolean remove(E e) | 删除集合中的元素（成功返回true失败返回false)
void clear() | 清空集合的所有元素(无返回值)
boolean contains(E e) | 判断集合是否包含某元素(有则返回true，无则返回false)
boolean isEmapty() | 判断集合是否为空(为空则返回true,不为空返回false)
int size() | 获取集合的长度并返回集合长度
Object[] toArray() | 将集合返回为一个数组

collection.toArray()的用法:
```java
Object[] arr = collection.toArray();
// 遍历数组
for(int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
```
# 迭代器

**迭代**：即Collection集合元素的通用获取方式。在取出元素之前要判断集合中有没有元素，如果有，就把这个元素取出来，继续再判断，如果还有就继续取出来。一直把集合中的元素全部取出。这种取出方式叫做迭代。

### Iterator迭代器

java.util.Iterator接口：迭代器(对集合进行遍历)。


Iterator是一个接口，我么无法直接使用，需要使用Iterator接口的实现类对象，获取实现类的方式比较特殊。

Collection接口中有一个方法，叫iterator()，这个方法返回的就是迭代器的实现类对象。

Iterator<E> iterator() 返回在此collection的元素上进行迭代的迭代器。

方法|描述
---|---
boolean hasNext() | 如果有元素可以迭代返回true
E next() | 返回迭代的下一个元素
void remove() | 从迭代器指向的collection 中移除迭代器返回的最后一个元素(可选操作，不常用)

**迭代器的使用步骤**
1. 使用集合中的方法iterator()获取迭代器的实现类对象，使用Iterator接口接收(多态)。
2. 使用Iterator接口中的方法hasNext判断还有没有下一个元素。
3. 使用Iterator接口中的方法next取出集合中下一个元素。

#### 代码实现
```java
public class IteratorDemo {
  	public static void main(String[] args) {
        // 使用多态方式创建对象
        Collection<String> coll = new ArrayList<String>();

        // 添加元素到集合
        coll.add("串串星人");
        coll.add("吐槽星人");
        coll.add("汪星人");
        
        //遍历
        //使用迭代器遍历每个集合对象都有自己的迭代器
        Iterator<String> it = coll.iterator();
        
        //  泛型指的是迭代出元素的数据类型
        while(it.hasNext()){ //判断是否有迭代元素
            String s = it.next();//获取迭代出的元素
            System.out.println(s);
        }
  	}
}
```
当it.hasNext()为false时再次取值会抛出NoSuchElementException异常。

### for each

增强for循环:底层使用的是迭代器，使用for循环的格式，简化了迭代器的书写Collection<E>:所有的单列集合都可以使用增强for。

**格式**

```java
for (/*集合/数组的数据类型 变量名:集合名/数组名*/){
    sout(/*变量名*/);
}
```
#### 代码实现

**遍历数组**

```java
public class NBForDemo1 {
    public static void main(String[] args) {
		int[] arr = {3,5,6,87};
       	//使用增强for遍历数组
		for(int a : arr){//a代表数组中的每个元素
			System.out.println(a);
		}
	}
}
```
**遍历集合**
```java
public class NBFor {
    public static void main(String[] args) {        
    	Collection<String> coll = new ArrayList<String>();
    	coll.add("小河神");
    	coll.add("老河神");
    	coll.add("神婆");
    	//使用增强for遍历
    	for(String s :coll){//接收变量s代表 代表被遍历到的集合元素
    		System.out.println(s);
    	}
	}
}
```
# 泛型

**概念**

是一种未知的数据类型，当我们不知道使用什么数据类型的时候，可以使用泛型。

E e:Element ·元素

T t: Type 类型。


- **泛型** 可以在类中或方法中预支使用未知的类型。
>tips:一般在创建对象时，将未知的类型确定具体的类型。当没有指定泛型时，默认类型为Object类型。

### 泛型的好处

##### 创建集合对象不使用泛型
集合不使用泛型，默认是Object类型，可以存储任意类型的数据。但是不安全，容易引发异常。

##### 泛型带来了哪些好处呢？

- 将运行时期的ClassCastException，转移到了编译时期变成了编译失败。
- 避免了类型转换的麻烦。
- **弊端**：泛型是什么类型就只能存储什么类型的数据。

> tips:泛型是数据类型的一部分，我们将类名与泛型合并一起看做数据类型。

### 泛型的定义和使用

#### 定义带有泛型的类
定义格式：
```java
修饰符 class 类名<代表泛型的变量> {  }
```
例如API中的ArrayList集合：
```java
class ArrayList<E>{ 
    public boolean add(E e){ }

    public E get(int index){ }
   	....
}
```
自定义泛型类：
```java
package com.yuer;

public class MyGenericClass<MVP> {
    //没有MVP类型，在这里代表 未知的一种数据类型 未来传递什么就是什么类型
    private MVP mvp;

    public void setMVP(MVP mvp) {
        this.mvp = mvp;
    }

    public MVP getMVP() {
        return mvp;
    }
}

```
自定义泛型类的使用格式：
```java
package com.yuer.Test;

import com.yuer.MyGenericClass;

public class Main01 {
    public static void main(String[] args) {
        MyGenericClass<Integer> mvp = new MyGenericClass<>();
        mvp.setMVP(22);
        System.out.println(mvp.getMVP());
    }
}

```
#### 定义带有泛型的方法

定义格式：
```java
修饰符 <代表泛型的变量> 返回值类型 方法名(参数){  }
```
例如：
```java
public class MyGenericMethod {	  
    public <MVP> void show(MVP mvp) {
    	System.out.println(mvp.getClass());
    }
    public <MVP> MVP show2(MVP mvp) {	
    	return mvp;
    }
}
```
使用格式：**调用方法时，确定泛型的类型**
```java
package com.yuer.Test;

import com.yuer.GenericMethod;

public class GenericMethodDemo {
    public static void main(String[] args) {
        GenericMethod gg = new GenericMethod();
        gg.show(123);
        gg.show("abc");
    }
}
```
#### 定义带有泛型的接口

定义格式：
```java
修饰符 interface接口名<代表泛型的变量> {  }
```
例如：
```java
public interface MyGenericInterface<E>{
	public abstract void add(E e);
	
	public abstract E getE();  
}
```

**1. 定义类时确定泛型的类型**
```java
public class MyImp1 implements MyGenericInterface<String> {
	@Override
    public void add(String e) {
        // 省略...
    }

	@Override
	public String getE() {
		return null;
	}
}
```

**2.定义类时不确定泛型的类型，直到创建对象时，确定泛型的类型**
```java
public class MyImp2<E> implements MyGenericInterface<E> {
	@Override
	public void add(E e) {
       	 // 省略...
	}

	@Override
	public E getE() {
		return null;
	}
}
```
确定泛型时：
```java
//使用时
public class GenericInterface {
    public static void main(String[] args) {
        MyImp2<String>  my = new MyImp2<String>();  
        my.add("aa");
    }
}
```
### 泛型通配符
当使用泛型类或者接口的时候，泛型的类型不确定的时候，可以使用通配符<?>表示。但是一旦使用通配符以后，只能使用Obeject的共性方法，集合中元素类型的方法无法使用。
#### 通配符的基本使用
泛型的通配符：**不知道使用什么类型来接收的时候,此时可以使用?,?表示未知通配符。**
此时只能接受数据,不能往该集合中存储数据。

举个例子大家理解使用即可：

```java
public static void main(String[] args) {
    Collection<Intger> list1 = new ArrayList<Integer>();
    getElement(list1);
    Collection<String> list2 = new ArrayList<String>();
    getElement(list2);
}
public static void getElement(Collection<?> coll){}
//？代表可以接收任意类型
```

> tips:泛型不存在继承关系 Collection<Object> list = new ArrayList<String>();这种是错误的。

#### 通配符高级使用----受限泛型
之前设置泛型的时候，实际上是可以任意设置的，只要是类就可以设置。但是在JAVA的泛型中可以指定一个泛型的**上限**和**下限**。

**泛型的上限**：

- **格式**： `类型名称 <? extends 类 > 对象名称`
- **意义**： `只能接收该类型及其子类`

**泛型的下限**：

- **格式**： 类型名称 <? super 类 > 对象名称
- **意义**： 只能接收该类型及其父类型

比如：现已知Object类，String 类，Number类，Integer类，其中Number是Integer的父类
```java
public static void main(String[] args) {
    Collection<Integer> list1 = new ArrayList<Integer>();
    Collection<String> list2 = new ArrayList<String>();
    Collection<Number> list3 = new ArrayList<Number>();
    Collection<Object> list4 = new ArrayList<Object>();
    
    getElement(list1);
    getElement(list2);//报错
    getElement(list3);
    getElement(list4);//报错
  
    getElement2(list1);//报错
    getElement2(list2);//报错
    getElement2(list3);
    getElement2(list4);
  
}
// 泛型的上限：此时的泛型?，必须是Number类型或者Number类型的子类
public static void getElement1(Collection<? extends Number> coll){}
// 泛型的下限：此时的泛型?，必须是Number类型或者Number类型的父类
public static void getElement2(Collection<? super Number> coll){}
```