## [1218. 最长定差子序列](https://leetcode-cn.com/problems/longest-arithmetic-subsequence-of-given-difference/)

给你一个整数数组 `arr` 和一个整数 `difference`，请你找出并返回 `arr` 中最长等差子序列的长度，该子序列中相邻元素之间的差等于 `difference` 。

**子序列** 是指在不改变其余元素顺序的情况下，通过删除一些元素或不删除任何元素而从 `arr` 派生出来的序列。

**示例 1：**

```
输入：arr = [1,2,3,4], difference = 1
输出：4
解释：最长的等差子序列是 [1,2,3,4]。
```

**示例 2：**

```
输入：arr = [1,3,5,7], difference = 1
输出：1
解释：最长的等差子序列是任意单个元素。
```

**示例 3：**

```
输入：arr = [1,5,7,8,5,3,4,2,1], difference = -2
输出：4
解释：最长的等差子序列是 [7,5,3,1]。
```

**提示：**

- `1 <= arr.length <= 105`
- `-104 <= arr[i], difference <= 104`

## 

思路：**动态规划**。

`dp[i]`认为是数组`0~i`等差序列的最大长度。如果`arr[i]-arr[j]=difference`，则有`dp[i]=max(dp[i],dp[j]+1)`。

一个会超时的方法为如下，先遍历，找到之前的符合条件的值，在它的长度上加1.

```java
public int longestSubsequence(int[] arr, int difference) {
    int[] dp = new int[arr.length];
    for(int i=0;i<arr.length;i++) {
        dp[i] = 1;
    }
    for(int i=1; i<arr.length; i++) {
        for(int j=0; j<i; j++) {
            if(arr[i]-arr[j] == difference) {
                dp[i] = Math.max(dp[i],dp[j]+1);
            }
        }
    }
    int result = 0;
    for(int i=0;i<arr.length;i++) {
        result = Math.max(result,dp[i]);
    }
    return result;
}
```

代码写出来就会发现，不用遍历所有的，记住这个数长度最大时的位置就行，没有出现过就记录长度为1，作为开始位置。看图：

![image-20211105002420201](https://gitee.com/hqinglau/img/raw/master/img/20211105002420.png)

![image-20211105004747461](https://gitee.com/hqinglau/img/raw/master/img/20211105004747.png)

![image-20211105004814455](https://gitee.com/hqinglau/img/raw/master/img/20211105004814.png)

![image-20211105004910509](https://gitee.com/hqinglau/img/raw/master/img/20211105004910.png)



![image-20211105004942061](https://gitee.com/hqinglau/img/raw/master/img/20211105004942.png)



![image-20211105005052940](https://gitee.com/hqinglau/img/raw/master/img/20211105005052.png)

![image-20211105005542064](https://gitee.com/hqinglau/img/raw/master/img/20211105005542.png)

![image-20211105005610917](https://gitee.com/hqinglau/img/raw/master/img/20211105005610.png)

![image-20211105005649469](https://gitee.com/hqinglau/img/raw/master/img/20211105005649.png)

![image-20211105005728656](https://gitee.com/hqinglau/img/raw/master/img/20211105005728.png)







代码：

```java
public int longestSubsequence(int[] arr, int difference) {
    HashMap<Integer, Integer> dp = new HashMap<>();
    int result = 0;
    for(int i=0; i<arr.length; i++) {
        dp.put(arr[i], dp.getOrDefault(arr[i] - difference, 0) + 1);
        result = Math.max(result, dp.get(arr[i]));
    }
    return result;
}
```

