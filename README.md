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

