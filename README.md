## 目录

### 面试经典150题

 [[1-24]数组 : 字符串.md](面试经典150题/[1-24]数组 : 字符串.md) 

 [[25-29]双指针.md](面试经典150题/[25-29]双指针.md) 

### 剑指Offer

> 题库参考这里
>
> https://blog.csdn.net/weixin_43840280/article/details/119447204

### 宝藏算法指导

 [Hello算法C语言.pdf](Notes/Hello算法C语言.pdf) 

> 上面记录刷题目录，下面记录Java零零散散的知识点，关于Java基础见Java笔记部分。对于算法，跟着一个作者即可，不要东看看西看看。务必！要的是沉浸和深入，减少些现代社会的浮躁吧。

## 1.复制数组

`Arrays.copyOfRange(nums3, 0, m + n)` 和 `System.arraycopy(nums3, 0, nums1, 0, m + n)` 都是用来复制数组的方法，但它们有一些区别：

1. **返回类型**:
   - `Arrays.copyOfRange(nums3, 0, m + n)` 返回一个新的数组。
   - `System.arraycopy(nums3, 0, nums1, 0, m + n)` 是一个 void 方法，它将元素复制到已经存在的数组 `nums1`中。
2. **数组初始化**:
   - `Arrays.copyOfRange(nums3, 0, m + n)` 会创建一个新的数组并返回。
   - `System.arraycopy(nums3, 0, nums1, 0, m + n)` 需要目标数组 `nums1`已经被初始化。
3. **使用场景**:
   - 如果你需要一个新的数组，可以使用 `Arrays.copyOfRange`。
   - 如果你需要将元素复制到一个已经存在的数组中，可以使用 `System.arraycopy`。

> 其具体使用多多尝试



## 2.Map的遍历

`Map`接口提供了两种常见的遍历形式：

1. **通过`entrySet()`遍历**： 这种方式可以同时获取键和值。`entrySet()`方法返回一个**包含所有键值对**的集合，可以使用增强的`for`循环进行遍历。

   ```java
   Map<Integer, String> map = new HashMap<>();
   map.put(1, "one");
   map.put(2, "two");
   
   for (Map.Entry<Integer, String> entry : map.entrySet()) {
     System.out.println("Key: " + entry.getKey() + ", Value: " + entry.getValue());
   }
   ```

2. **通过`keySet()`遍历**： 这种方式只获取键，然后通过键来获取对应的值。`keySet()`方法返回一个**包含所有键**的集合，可以使用增强的`for`循环进行遍历。

   ```java
   Map<Integer, String> map = new HashMap<>();
   map.put(1, "one");
   map.put(2, "two");
   
   for (Integer key : map.keySet()) {
     System.out.println("Key: " + key + ", Value: " + map.get(key));
   }
   ```

**性能差异**：

- `entrySet()`遍历在性能上通常优于`keySet()`遍历，因为`entrySet()`直接访问键和值，而`keySet()`遍历需要通过键再次访问值，这会增加额外的查找开销。



## 3.Map的初始化

1. **使用 `HashMap` 构造函数**：

   ```java
   Map<String, Integer> map = new HashMap<>();
   map.put("a", 1);
   map.put("b", 2);
   map.put("c", 3);
   
   // 同时注意下面这种形式
   Map<String, Integer> map = new HashMap<String, Integer>(){{
   	put("a", 1);
   	put("b", 2);
   	put("c", 3);
   }}
   // 匿名内部类的一个实例初始值设定项 （也称为一种"双大括号初始化"）,其他部分实现类也能通过这种方式初始化
   ```

2. **使用 `Map.of`**：

   - 适用于最多 10 个键值对的不可变映射。

   ```java
   Map<String, Integer> map = Map.of("a", 1, "b", 2, "c", 3);
   ```

   **使用 `Map.ofEntries`**：

   - 适用于任意数量键值对的不可变映射。

   ```java
   Map<String, Integer> map = Map.ofEntries(
     Map.entry("a", 1),
     Map.entry("b", 2),
     Map.entry("c", 3),
     Map.entry("d", 4),
     Map.entry("e", 5)
   )；
   ```

3. **使用 `Collections.singletonMap`**：

   - 适用于只有一个键值对的不可变映射。

   ```java
   Map<String, Integer> map = Collections.singletonMap("a", 1);
   ```

4. **使用 `Stream` 和 `Collectors.toMap`**：

   - 适用于动态生成键值对的映射。

   ```java
   Map<String, Integer> map = Stream.of(new Object[][] {
     { "a", 1 },
     { "b", 2 },
     { "c", 3 },
   }).collect(Collectors.toMap(data -> (String) data[0], data -> (Integer) data[1]));
   ```

   > 利用stream和lombda表达式



## 4.Map的各大实现类排序机制

1. **`HashMap`**：

   - **排序机制**：`HashMap` 不保证键值对的顺序。键值对的顺序可能会随着插入和删除操作而变化。

   - **特性**：基于哈希表实现，允许 `null` 键和 `null` 值，提供快速的查找、插入和删除操作。

   - > 顾名思义

2. **`LinkedHashMap`**：

   - **排序机制**：`LinkedHashMap` 保证键值对的插入顺序（即按键值对插入的顺序进行排序）。还可以按访问顺序排序（通过构造函数参数指定）。
   - **特性**：基于哈希表和双向链表实现，允许 `null` 键和 `null` 值，维护插入顺序或访问顺序。

3. **`TreeMap`**：

   - **排序机制**：`TreeMap` 按键的自然顺序（`Comparable`）或自定义比较器（`Comparator`）进行排序。
   - **特性**：基于红黑树实现，不允许 `null` 键，提供有序的键值对视图。

4. **`Hashtable`**：

   - **排序机制**：`Hashtable` 不保证键值对的顺序。类似于 `HashMap`，但它是线程安全的。
   - **特性**：基于哈希表实现，不允许 `null` 键和 `null` 值，线程安全。

```java
Map<Integer, String> romanMap = new LinkedHashMap<>(sortedMap);
```

- `HashMap` 和 `Hashtable` 不支持排序。
- `LinkedHashMap` 可以保持插入顺序，但不自动排序。
- `TreeMap` 支持自动排序。

> Map的排序时灵活且好用的，按需求使用就好



## 5.String操作汇总

> String的操作是重中之重，故特意整理出来

1. 基本操作方法
2. 字符串比较
3. 字符串与其他数据类型之间的转换
4. 字符与字符串的查找
5. 字符串的截取与拆分
6. 字符串的替换与修改
7. 匹配开始

### 1.基本操作

1. 获取字符串长度方法 `length()`

   格式：int length = str.length();

2. 获取字符串中的第 i 个字符方法 `charAt(i)`

   格式：char ch = str.charAt(i);

3. 获取指定位置的字符方法 `getChars(4个参数)`

   格式：char array[] = new char[80];

   str.getChars(indexBegin,indexEnd,array,arrayBegin);


### 2.字符串比较

1. **使用 `==` 运算符**：
   - 这种方式比较的是两个字符串对象的引用是否相同，而不是它们的内容是否相同。
2. **使用 `equals()` 方法**：
   - 这种方式比较的是两个字符串的内容是否相同。
3. **使用 `compareTo()` 方法**：
   - 这种方式按字典顺序比较两个字符串。返回值为负数、零或正数，分别表示此字符串小于、等于或大于指定的字符串。

### 3.字符串与其他数据类型之间的转换

| 数据类型 | 字符串转换为其他数据类型的方法 | 其它数据类型转换为字符串的方法1 | 其他数据类型转换为字符串的方法2 |
| -------- | ------------------------------ | ------------------------------- | ------------------------------- |
| byte     | Byte.parseByte(str)            | String.valueOf([byte] bt)       | Byte.toString([byte] bt)        |
| int      | Integer.parseInt(str)          | String.valueOf([int] i)         | Int.toString([int] i)           |
| long     | Long.parseLong(str)            | String.valueOf([long] l)        | Long.toString([long] l)         |
| float    | Float.parseFloat(str)          | String.valueOf([float] f)       | Float.toString([float] f)       |
| double   | double.parseDouble(str)        | String.valueOf([double] d)      | Double.toString([double] b)     |
| char     | str.charAt()                   | String.valueOf([char] c)        | Character.toString([char] c)    |
| boolean  | Boolean.getBoolean(str)        | String.valueOf([boolean] b)     | Boolean.toString([boolean] b)   |

### 4.字符串查找

1. indexOf()方法 
2. lastIndexOf()方法

### 5.截取与拆分

**`substring()` 方法**

- 格式1：`String result = str.substring(index);`
- 格式2：`String result = str.substring(beginIndex, EndIndex);` // 实际索引号 `[beginIndex, EndIndex-1]`

**`split()` 方法**

- 格式1：`String strArray[] = str.split(正则表达式);`
- 格式2：`String strArray[] = str.split(正则表达式, limit);`

```java
String str = "apple,banana,orange";
String[] fruits = str.split(",");

String str = "apple;banana,orange";
String[] fruits = str.split("[,;]");
// 输出：
// apple
// banana
// orange

String str = "apple,banana,orange";
String[] fruits = str.split(",", 2);
// 输出：
// apple
// banana,orange

String str = "hello  world"; // 两个空格
str.split(" ");
// 输出
// "hello"
// “”
// “world”

str.split("\\s");
// 输出
// "hello"
// “world”
```
>
> 这里稍微注意了

### 6.替换或修改

1. **`concat()` 方法：合并字符串**
   - 格式：`String result = str1.concat(str2);` 
2. **`toLowerCase()` 方法：将字符全部转化为小写**
   - 格式：`String result = str.toLowerCase();`
3. **`toUpperCase()` 方法：将字符全部转化为大写**
   - 格式：`String result = str.toUpperCase();`
4. **`replaceAll()`、`replaceFirst()` 方法：需要匹配正则表达式**

### 7.匹配开始

```java
str.startWith(otherStr);
// 返回boolean对象
```

## 6.String数组的排序

1. 默认字典序

   ```java
   String[] strs = {"banana", "apple", "orange"};
   Arrays.sort(strs);
   ```

2. 自定义为长度排序

   ```java
   Arrays.sort(strs, new Comparator<String>() {
       @Override
       public int compare(String s1, String s2) {
           return Integer.compare(s1.length(), s2.length());
       }
   });
   
   Arrays.sort(strs, (s1, s2) -> Integer.compare(s1.length(), s2.length()));
   ```

## 7,List<? >初始化

> 关于List非常详细的介绍看下列博文，这里不做没有意义的重复搬运
>
> https://juejin.cn/post/7290741484365545472

### 1.实现类初始化

```java
List<String> test = new ArrayList<String>();
test.add("aaa");
test.add("bbb");
test.add("ccc");
System.out.println(test.size());
```

### 2.利用Arrays工具包

```java
List<String> test1 = Arrays.asList("xxx","yyy","zzz");
//test1.add("aaa");//wrong  java.lang.UnsupportedOperationException
//test1.add("bbb");//wrong  java.lang.UnsupportedOperationException
//test1.add("ccc");//wrong  java.lang.UnsupportedOperationException
System.out.println(test1.size());
```

### 3.不完全利用Array工具包

```java
List<String> test2 =new ArrayList<String>(Arrays.asList("xxx","yyy","zzz"));
test2.add("aaa");
test2.add("bbb");
test2.add("ccc");
System.out.println(test2.size());
```

### 4.匿名内部类

```java
List<String> test3 = new ArrayList<String>(){
	{  
	add("aaa");  
	add("bbb");  
	add("ccc");  
	}
}; 
```



## 8.List<List<`Integer`>>

> 有点套娃的感觉，这里特意稍微记录下，其本质和C++里面的vector<vector<`int`>>差别不大

### 1.初始化

```java
List<List<Integer>> list = new ArrayList<>();
```

或者

```java
List<List<Integer>> list = new ArrayList<List<Integer>>();
```

这里能够对Java的接口和实现类有更多的了解和认识，注意看下面使用，就能大致发现脉络

### 2.比较/去重

这种对象类显然不是使用==来比较，可以使用contains来进行比较

#### 1.使用contains去重

> 单独的两个List用equal来比较即可

```java
for(int i=0;i<ans.size();i++){
    List<Integer> temp = ans.get(i);
    Collections.sort(temp);
    if(!ret.contains(temp)){
        System.out.println(temp);
        ret.add(temp);
    }
}
```

> 用于单个List对象内部去重，但是开辟了额外的数组空间，空间复杂度高

#### 2.使用remove去重

> 双重for循环，j=i+1即可

```java
if(list.get(i).equals(list.get(j))){
    list.remove(j);
}
```

#### 3.使用LinkedHashSet实现List去重(无序)

> 使用了其他对象，空间复杂度高，不过少了排序的步骤

```java
List<String> list = new ArrayList<>(Arrays.asList("apple", "banana", "apple", "orange", "banana"));
Set<String> set = new LinkedHashSet<>(list);
List<String> newList = new ArrayList<>(set);
```



## 9.LinkedList

`LinkedList` 是 Java 集合框架中的一个类，基于双向链表实现。以下是一些关键知识点：

1. **实现的接口**：
   - `List`：提供列表的基本操作，如添加、删除、访问元素等。
   - `Deque`：提供双端队列的操作，如在头部和尾部添加、删除元素。
   - `Queue`：提供队列的基本操作，如添加、删除、查看队首元素等。
2. **特点**：
   - **双向链表**：每个节点包含指向前一个和后一个节点的指针。
   - **动态大小**：不需要预先指定大小，插入和删除操作不会引起内存的重新分配。
   - **插入和删除效率高**：在链表的任意位置插入或删除元素的时间复杂度为 O(1)。
   - **访问效率低**：访问任意位置的元素需要从头或尾遍历，时间复杂度为 O(n)。
3. **常用方法**：
   - `add(E e)`：在链表的尾部添加元素。
   - `addFirst(E e)`：在链表的头部添加元素。
   - `addLast(E e)`：在链表的尾部添加元素。
   - `remove()`：删除并返回链表的头部元素。
   - `removeFirst()`：删除并返回链表的头部元素。
   - `removeLast()`：删除并返回链表的尾部元素。
   - `get(int index)`：返回指定位置的元素。
   - `size()`：返回链表的元素个数。
4. **使用场景**：
   - 适用于频繁插入和删除操作的场景。
   - 不适用于频繁随机访问的场景。

> 在大量插入，删除（尤其是头和尾）的情况下非常推荐使用LinkedList

