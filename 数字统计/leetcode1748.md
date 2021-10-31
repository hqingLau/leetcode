## [1748. 唯一元素的和](https://leetcode-cn.com/problems/sum-of-unique-elements/)

给你一个整数数组 `nums` 。数组中唯一元素是那些只出现 **恰好一次** 的元素。

请你返回 `nums` 中唯一元素的 **和** 。

**示例 1：**

```
输入：nums = [1,2,3,2]
输出：4
解释：唯一元素为 [1,3] ，和为 4 。
```

**示例 2：**

```
输入：nums = [1,1,1,1,1]
输出：0
解释：没有唯一元素，和为 0 。
```

**示例 3 ：**

```
输入：nums = [1,2,3,4,5]
输出：15
解释：唯一元素为 [1,2,3,4,5] ，和为 15 。
```

**提示：**

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`

## 题解

如果数组很大，但是数字限制在一定范围的话，用两次遍历比较高效。

如果数组分布较为分散，可以用`Map`。

### 一、两次遍历

数组形式：

```java
public int sumOfUnique(int[] nums) {
    int[] count = new int[101];
    int result = 0;
    for (int n : nums) {
        count[n]++;
    }
    for (int i = 1; i <= 100; i++) {
        if (count[i] == 1) {
            result += i;
        }
    }
    return result;
}
```

`Map`形式：

```java
public int sumOfUnique(int[] nums) {
    Map<Integer, Integer> map = new HashMap<>();
    int result = 0;
    for (int n : nums) {
        map.put(n, map.getOrDefault(n, 0) + 1);
    }
    for (int i = 1; i <= 100; i++) {
        if (map.getOrDefault(i, 0) == 1) {
            result += i;
        }
    }
    return result;
}
```

### 二、一次遍历

数组形式：

```java
public int sumOfUnique(int[] nums) {
    int[] count = new int[101];
    int result = 0;
    for (int n : nums) {
        // 第一次见加入，第二次见减去，其他无视
        result += ++count[n]==1?n:count[n]==2?-n:0;
    }
    return result;
}
```

`Map`形式：

```java
class Solution {
    public int sumOfUnique(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        int result = 0;
        for(int n :nums) {
            map.put(n,map.getOrDefault(n,0)+1);
            int tmp = map.get(n);
            result += tmp==1?n:tmp==2?-n:0;
        }
        return result;
    }
}
```

