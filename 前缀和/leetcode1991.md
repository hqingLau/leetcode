## [1991. 找到数组的中间位置](https://leetcode-cn.com/problems/find-the-middle-index-in-array/)

给你一个下标从 **0** 开始的整数数组 `nums` ，请你找到 **最左边** 的中间位置 `middleIndex` （也就是所有可能中间位置下标最小的一个）。

中间位置 `middleIndex` 是满足 `nums[0] + nums[1] + ... + nums[middleIndex-1] == nums[middleIndex+1] + nums[middleIndex+2] + ... + nums[nums.length-1]` 的数组下标。

如果 `middleIndex == 0` ，左边部分的和定义为 `0` 。类似的，如果 `middleIndex == nums.length - 1` ，右边部分的和定义为 `0` 。

请你返回满足上述条件 **最左边** 的 `middleIndex` ，如果不存在这样的中间位置，请你返回 `-1` 。

**示例 1：**

```
输入：nums = [2,3,-1,8,4]
输出：3
解释：
下标 3 之前的数字和为：2 + 3 + -1 = 4
下标 3 之后的数字和为：4 = 4
```

**示例 2：**

```
输入：nums = [1,-1,4]
输出：2
解释：
下标 2 之前的数字和为：1 + -1 = 0
下标 2 之后的数字和为：0
```

**示例 3：**

```
输入：nums = [2,5]
输出：-1
解释：
不存在符合要求的 middleIndex 。
```

**示例 4：**

```
输入：nums = [1]
输出：0
解释：
下标 0 之前的数字和为：0
下标 0 之后的数字和为：0
```

**提示：**

- `1 <= nums.length <= 100`
- `-1000 <= nums[i] <= 1000`

## 解答

中间位置指的是：这个位置左边所有元素的和，等于右边所有元素的和。

如果我们能知道当前元素**左边的和**和**右边的和**，那就省去了重复计算。

所有要先计算每个位置左边所有元素和、右边所有元素和，用两个数组代替。

![image-20211102120906633](https://gitee.com/hqinglau/img/raw/master/img/20211102120906.png)

java代码如下：

```java
public int findMiddleIndex(int[] nums) {
    int n = nums.length;
    if(n == 0) {
        return -1;
    }
    int[] left = new int[n];
    left[0] = nums[0];
    for(int i=1; i<nums.length; i++) {
        left[i] = nums[i] + left[i-1];
    }
    for(int i=0; i<nums.length; i++) {
        if(left[i]-nums[i] == left[n-1]-left[i]) {
            return i;
        }
    }
    return -1;
}
```

- 时间复杂度：`O(n)`

- 空间复杂度：`O(n)`

