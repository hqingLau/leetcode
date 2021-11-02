## [910. 最小差值 II](https://leetcode-cn.com/problems/smallest-range-ii/)

给你一个整数数组 `A`，对于每个整数 `A[i]`，可以选择 **`x = -K` 或是 `x = K`** （`**K**` 总是非负整数），并将 `x` 加到 `A[i]` 中。

在此过程之后，得到数组 `B`。

返回 `B` 的最大值和 `B` 的最小值之间可能存在的最小差值。

**示例 1：**

```
输入：A = [1], K = 0
输出：0
解释：B = [1]
```

**示例 2：**

```
输入：A = [0,10], K = 2
输出：6
解释：B = [2,8]
```

**示例 3：**

```
输入：A = [1,3,6], K = 3
输出：3
解释：B = [4,6,3]
```

 

**提示：**

- `1 <= A.length <= 10000`
- `0 <= A[i] <= 10000`
- `0 <= K <= 10000`

## 题解

先对数字进行排序，令D=Max-Min，下一步就是找到比D更小的差值。

如果K>=D，那就不用找了，结果就是D。因为我们可以把所有项都加上K。

对于两个点，要变化，有四种情况：

![image-20211102134603293](https://gitee.com/hqinglau/img/raw/master/img/20211102134603.png)

1和2整体+k或者-k，MAX-MIN不变，情况3差更大了，只有情况4可能减小MAX-MIN。

所以解决方案就是比较所有可能的情况4的差值。

所以我们先把所有点下移k，在逐项遍历，将前面的项改为+k，找最小差。

看图：

![image-20211102133220126](https://gitee.com/hqinglau/img/raw/master/img/20211102133220.png)

要想Max-Min<D，就需要向中间聚拢，小的+k，大的-k。

```java
class Solution {
    public int smallestRangeII(int[] nums, int k) {
        if(nums.length == 0) {
            return 0;
        }
        Arrays.sort(nums);
        int result = nums[nums.length-1]-nums[0];
        for(int i=0; i<nums.length-1; i++) {
            result = Math.min(result,Math.max(nums[nums.length-1]-k,nums[i]+k)-Math.min(nums[i+1]-k,nums[0]+k));
        }
        return result;
    }
}
```







