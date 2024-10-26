> 面试用，考研复试用，致敬热爱计算机的自己。
>
> 除了基本的解法，还按照要求尽量拓展解法，使用Java语言，深入学习Java。

## 1.[[简单]88. 合并两个有序数组](https://leetcode.cn/problems/merge-sorted-array/)

给你两个按 **非递减顺序** 排列的整数数组 `nums1` 和 `nums2`，另有两个整数 `m` 和 `n` ，分别表示 `nums1` 和 `nums2` 中的元素数目。

请你 **合并** `nums2` 到 `nums1` 中，使合并后的数组同样按 **非递减顺序** 排列。

**注意：**最终，合并后数组不应由函数返回，而是存储在数组 `nums1` 中。为了应对这种情况，`nums1` 的初始长度为 `m + n`，其中前 `m` 个元素表示应合并的元素，后 `n` 个元素为 `0` ，应忽略。`nums2` 的长度为 `n` 。

 

**示例 1：**

```
输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
输出：[1,2,2,3,5,6]
解释：需要合并 [1,2,3] 和 [2,5,6] 。
合并结果是 [1,2,2,3,5,6] ，其中斜体加粗标注的为 nums1 中的元素。
```

**示例 2：**

```
输入：nums1 = [1], m = 1, nums2 = [], n = 0
输出：[1]
解释：需要合并 [1] 和 [] 。
合并结果是 [1] 。
```

**示例 3：**

```
输入：nums1 = [0], m = 0, nums2 = [1], n = 1
输出：[1]
解释：需要合并的数组是 [] 和 [1] 。
合并结果是 [1] 。
注意，因为 m = 0 ，所以 nums1 中没有元素。nums1 中仅存的 0 仅仅是为了确保合并结果可以顺利存放到 nums1 中。
```

 

**提示：**

- `nums1.length == m + n`
- `nums2.length == n`
- `0 <= m, n <= 200`
- `1 <= m + n <= 200`
- `-109 <= nums1[i], nums2[j] <= 109`

 

**进阶：**你可以设计实现一个时间复杂度为 `O(m + n)` 的算法解决此问题吗？

> 见解法2



**解法1：**

> 先合并两个数组，然后直接快速排序

```java
class Solution {
  public void merge(int[] nums1, int m, int[] nums2, int n) {
    int cnt = 0;
    for (int i = m; i < m + n; i++) {
      nums1[i] = nums2[cnt++];
    }
    Arrays.sort(nums1);
  }
}
```

> 暴力解法
>
> Arrays.sort();



**解法2：**

> 新开一个数组，牺牲时间换空间。

```java
class Solution {
  public void merge(int[] nums1, int m, int[] nums2, int n) {
    int[] nums3=new int[m+n];
    int left=0;int right=0;
    for(int i=0;i<m+n;i++){
      if(left==m){
        nums3[i]=nums2[right++];
      }else if(right==n){
        nums3[i]=nums1[left++];
      }else if(nums1[left]<nums2[right]){
        nums3[i]=nums1[left++];
      }else{
        nums3[i]=nums2[right++];
      }
    }
    for(int i=0;i<m+n;i++){
      nums1[i]=nums3[i];
    }
    // System.arraycopy(nums3, 0, nums1, 0, m + n);
  }
} 
```

> 显然写的比较繁琐。

> if(right==n || nums1[left]<=nums2[right])
>
> 这样写会出错，会数组越界



**解法3：**

> 逆向思维

```java
class Solution {
  public void merge(int[] nums1, int m, int[] nums2, int n) {
    int i = m - 1;
    int j = n - 1;
    int k = m + n - 1;
    while (i >= 0 && j >= 0) {
      if (nums1[i] < nums2[j])
        nums1[k--] = nums2[j--];
      else
        nums1[k--] = nums1[i--];
    }
    while(j>=0) nums1[k--]=nums2[j--];
  }
}
```

> 逆向填补，然后把nums2剩下的填补



**补充：**

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



## 2.[[简单]27. 移除元素](https://leetcode.cn/problems/remove-element/)

给你一个数组 `nums` 和一个值 `val`，你需要 **[原地](https://baike.baidu.com/item/原地算法)** 移除所有数值等于 `val` 的元素。元素的顺序可能发生改变。然后返回 `nums` 中与 `val` 不同的元素的数量。

假设 `nums` 中不等于 `val` 的元素数量为 `k`，要通过此题，您需要执行以下操作：

- 更改 `nums` 数组，使 `nums` 的前 `k` 个元素包含不等于 `val` 的元素。`nums` 的其余元素和 `nums` 的大小并不重要。
- 返回 `k`。

**用户评测：**

评测机将使用以下代码测试您的解决方案：

```
int[] nums = [...]; // 输入数组
int val = ...; // 要移除的值
int[] expectedNums = [...]; // 长度正确的预期答案。
                            // 它以不等于 val 的值排序。

int k = removeElement(nums, val); // 调用你的实现

assert k == expectedNums.length;
sort(nums, 0, k); // 排序 nums 的前 k 个元素
for (int i = 0; i < actualLength; i++) {
    assert nums[i] == expectedNums[i];
}
```

如果所有的断言都通过，你的解决方案将会 **通过**。

 

**示例 1：**

```
输入：nums = [3,2,2,3], val = 3
输出：2, nums = [2,2,_,_]
解释：你的函数函数应该返回 k = 2, 并且 nums 中的前两个元素均为 2。
你在返回的 k 个元素之外留下了什么并不重要（因此它们并不计入评测）。
```

**示例 2：**

```
输入：nums = [0,1,2,2,3,0,4,2], val = 2
输出：5, nums = [0,1,4,0,3,_,_,_]
解释：你的函数应该返回 k = 5，并且 nums 中的前五个元素为 0,0,1,3,4。
注意这五个元素可以任意顺序返回。
你在返回的 k 个元素之外留下了什么并不重要（因此它们并不计入评测）。
```

 

**提示：**

- `0 <= nums.length <= 100`
- `0 <= nums[i] <= 50`
- `0 <= val <= 100`



**解法1：**

> 枚举，基于不在意后面存储了什么元素。
>
> *O*(*N*)

```java
class Solution {
  public int removeElement(int[] nums, int val) {
      int k=0;
      for(int i=0;i<nums.length;i++){
          if(nums[i]!=val){
              nums[k]=nums[i];
              k++;
          }
      }
      return k;
  }
}
```

> k++确保了k的数值代表了数组nums的长度
>
> 不必在乎剩下的数组里面存储了什么



**解法2：**

> 双指针，参考提示得出灵感。
>
> *O*(*N*)

```java
class Solution {
  public int removeElement(int[] nums, int val) {
    if (nums.length == 0)
      return 0;
    int k = nums.length;
    int l = 0, r = nums.length - 1;
    while (nums[r] == val && r >= 0) {
      r--;
      k--;
      if (k == 0)
        return 0;
    }
    while (l <= r) {
      if (nums[l] == val) {
        nums[l] = nums[r];
        nums[r] = val;
        r--;
        k--;
      }
      l++;
      while (nums[r] == val && r >= 0) {
        r--;
        k--;
        if (k == 0)
          return 0;
      }
    }
    return k;
  }
}
```

> 总是右指针匹配了就把移动右指针，把长度减少。
>
> 基于左右交换的双指针。



**题解：**

> 主要是自己的双指针写的过于繁琐

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int n = nums.length;
        int left = 0;
        for (int right = 0; right < n; right++) {
            if (nums[right] != val) {
                nums[left] = nums[right];
                left++;
            }
        }
        return left;
    }
}
```

> 基于快慢指针的双指针。
>
> 其实这里和我的解法1一致，不过我的解法1显然比题解还要好。



## 3.[[简单]26. 删除有序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)

给你一个 **非严格递增排列** 的数组 `nums` ，请你**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使每个元素 **只出现一次** ，返回删除后数组的新长度。元素的 **相对顺序** 应该保持 **一致** 。然后返回 `nums` 中唯一元素的个数。

考虑 `nums` 的唯一元素的数量为 `k` ，你需要做以下事情确保你的题解可以被通过：

- 更改数组 `nums` ，使 `nums` 的前 `k` 个元素包含唯一元素，并按照它们最初在 `nums` 中出现的顺序排列。`nums` 的其余元素与 `nums` 的大小不重要。
- 返回 `k` 。

**判题标准:**

系统会用下面的代码来测试你的题解:

```
int[] nums = [...]; // 输入数组
int[] expectedNums = [...]; // 长度正确的期望答案

int k = removeDuplicates(nums); // 调用

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
```

如果所有断言都通过，那么您的题解将被 **通过**。

 

**示例 1：**

```
输入：nums = [1,1,2]
输出：2, nums = [1,2,_]
解释：函数应该返回新的长度 2 ，并且原数组 nums 的前两个元素被修改为 1, 2 。不需要考虑数组中超出新长度后面的元素。
```

**示例 2：**

```
输入：nums = [0,0,1,1,1,2,2,3,3,4]
输出：5, nums = [0,1,2,3,4]
解释：函数应该返回新的长度 5 ， 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4 。不需要考虑数组中超出新长度后面的元素。
```

 

**提示：**

- `1 <= nums.length <= 3 * 104`
- `-104 <= nums[i] <= 104`
- `nums` 已按 **非严格递增** 排列

> 这个题和上面那个题基本类似了。
>
> 虽然元素的出现不一定是按照严格递增，但是整体出现的位置是相对的，这里出现了后面也不能再出现了。



**解法1：**

> 和上面那个题的思想一致了

```java
class Solution {
  public int removeDuplicates(int[] nums) {
    int k = 0;
    for (int i = 0; i < nums.length; i++) {
      if (i == 0 || nums[i] != nums[i - 1]) {
        nums[k++] = nums[i];
      }
    }
    return k;
  }
}
```

> 至于为什么是i == 0 || nums[i] != nums[i - 1])
>
> 提示下面那段注释解释的很清楚了



## 4.[[中等]80. 删除有序数组中的重复项 II](https://leetcode.cn/problems/remove-duplicates-from-sorted-array-ii/)

给你一个有序数组 `nums` ，请你**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使得出现次数超过两次的元素**只出现两次** ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 **[原地 ](https://baike.baidu.com/item/原地算法)修改输入数组** 并在使用 O(1) 额外空间的条件下完成。

 

**说明：**

为什么返回数值是整数，但输出的答案是数组呢？

请注意，输入数组是以**「引用」**方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

```
// nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中 该长度范围内 的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

 

**示例 1：**

```
输入：nums = [1,1,1,2,2,3]
输出：5, nums = [1,1,2,2,3]
解释：函数应返回新长度 length = 5, 并且原数组的前五个元素被修改为 1, 1, 2, 2, 3。 不需要考虑数组中超出新长度后面的元素。
```

**示例 2：**

```
输入：nums = [0,0,1,1,1,1,2,3,3]
输出：7, nums = [0,0,1,1,2,3,3]
解释：函数应返回新长度 length = 7, 并且原数组的前七个元素被修改为 0, 0, 1, 1, 2, 3, 3。不需要考虑数组中超出新长度后面的元素。
```

 

**提示：**

- `1 <= nums.length <= 3 * 104`
- `-104 <= nums[i] <= 104`
- `nums` 已按升序排列

> 只能原地操作，不让使用新的空间。出现的次数大于等于2则保存两次



**解法1：**

```java
```

