## [268. 丢失的数字](https://leetcode-cn.com/problems/missing-number/)

难度简单456

给定一个包含 `[0, n]` 中 `n` 个数的数组 `nums` ，找出 `[0, n]` 这个范围内没有出现在数组中的那个数。

**示例 1：**

```
输入：nums = [3,0,1]
输出：2
解释：n = 3，因为有 3 个数字，所以所有的数字都在范围 [0,3] 内。2 是丢失的数字，因为它没有出现在 nums 中。
```

**示例 2：**

```
输入：nums = [0,1]
输出：2
解释：n = 2，因为有 2 个数字，所以所有的数字都在范围 [0,2] 内。2 是丢失的数字，因为它没有出现在 nums 中。
```

**示例 3：**

```
输入：nums = [9,6,4,2,3,5,7,0,1]
输出：8
解释：n = 9，因为有 9 个数字，所以所有的数字都在范围 [0,9] 内。8 是丢失的数字，因为它没有出现在 nums 中。
```

**示例 4：**

```
输入：nums = [0]
输出：1
解释：n = 1，因为有 1 个数字，所以所有的数字都在范围 [0,1] 内。1 是丢失的数字，因为它没有出现在 nums 中。
```

 

**提示：**

- `n == nums.length`
- `1 <= n <= 104`
- `0 <= nums[i] <= n`
- `nums` 中的所有数字都 **独一无二**

 

**进阶：**你能否实现线性时间复杂度、仅使用额外常数空间的算法解决此问题?

## 一、数组标志位

新建一个数组标记访问。最后寻找为被标记的数。

```java
public int missingNumber(int[] nums) {
    boolean[] visited = new boolean[nums.length+1];
    for(int i=0;i<nums.length;i++) {
        visited[nums[i]] = true;
    }
    for(int i=0;i<nums.length+1;i++) {
        if(visited[i]==false) {
            return i;
        }
    }
    return -1;
}
```

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

## 二、排序

要实现常数空间，一个简单的做法是原地排序，然后遍历数组。

```java
public int missingNumber(int[] nums) {
    Arrays.sort(nums);
    if(nums[0]!=0) {
        return 0;
    }
    for(int i=1;i<nums.length;i++) {
        // 非递增，如0 2
        // 1的位置为2，说明1没了
        if(nums[i]-nums[i-1]!=1) {
            return i;
        }
    }
    return nums.length;
}
```

- 时间复杂度：`O(nlog(n))`
- 空间复杂度：`O(1)`

## 三、归位

题意：所有数字独一无二，只有一个缺的。

![image-20211106012855588](https://gitee.com/hqinglau/img/raw/master/img/20211106012855.png)

![image-20211106012928824](https://gitee.com/hqinglau/img/raw/master/img/20211106012928.png)

![image-20211106013000357](https://gitee.com/hqinglau/img/raw/master/img/20211106013000.png)

![image-20211106013021758](https://gitee.com/hqinglau/img/raw/master/img/20211106013021.png)

![image-20211106013045461](https://gitee.com/hqinglau/img/raw/master/img/20211106013045.png)

![image-20211106013112627](https://gitee.com/hqinglau/img/raw/master/img/20211106013112.png)

![image-20211106013149185](https://gitee.com/hqinglau/img/raw/master/img/20211106013149.png)

![image-20211106013212995](https://gitee.com/hqinglau/img/raw/master/img/20211106013213.png)

![image-20211106015613897](https://gitee.com/hqinglau/img/raw/master/img/20211106015613.png)

![image-20211106012757547](https://gitee.com/hqinglau/img/raw/master/img/20211106012757.png)

代码：

```java
public int missingNumber(int[] nums) {
    int cur = nums[0];
    int p;
    for(int i=0;i<nums.length;i++) {
        if(i==nums[i]) {
            continue;
        }
        p = nums[i];
        while(p!=nums.length&&p!=nums[p]) {
            cur = nums[p];
            nums[p] = p;
            p = cur;
        }
    }
    for(int i=0;i<nums.length;i++) {
        if(i!=nums[i]) {
            return i;
        }
    }
    return nums.length;
}
```

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`





