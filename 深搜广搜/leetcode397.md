## [397. 整数替换](https://leetcode-cn.com/problems/integer-replacement/)

给定一个正整数 `n` ，你可以做如下操作：

1. 如果 `n` 是偶数，则用 `n / 2`替换 `n` 。
2. 如果 `n` 是奇数，则可以用 `n + 1`或`n - 1`替换 `n` 。

`n` 变为 `1` 所需的最小替换次数是多少？

 

**示例 1：**

```
输入：n = 8
输出：3
解释：8 -> 4 -> 2 -> 1
```

**示例 2：**

```
输入：n = 7
输出：4
解释：7 -> 8 -> 4 -> 2 -> 1
或 7 -> 6 -> 3 -> 2 -> 1
```

**示例 3：**

```
输入：n = 4
输出：2
```

 

**提示：**

- `1 <= n <= 231 - 1`



## 题解

### 一、递归硬算

递归的思路比较容易想，模拟题中的说法，n为偶数就继续求n/2的变换次数加上当前的1次变换；奇数就是当前的1次变换加上+1或者-1之后的最小次数。

```java
public int integerReplacement(int n) {
    // 递归
    // 偶数：一条路
    // 奇数：加一或者减一
    if(n==1) return 0;
    if(n%2==0) {
        return 1+integerReplacement(n/2);
    } else {
        return 1+Math.min(integerReplacement(n+1),integerReplacement(n-1));
    }
}
```

**最后会栈溢出**。`2147483647`成了过不去的坎。

### 二、原来是越界

想一下会发现，测试用例`2147483647`就是整数的最大值，在`integerReplacement(n+1)`中越界了啊。修改函数参数类型为`long`。

```java
class Solution {
    public int integerReplacement(long n) {
        // 递归
        // 偶数：一条路
        // 奇数：加一或者减一
        if(n==1) return 0;
        if(n%2==0) {
            return 1+integerReplacement(n/2);
        } else {
            return 1+Math.min(integerReplacement(n+1),integerReplacement(n-1));
        }
    }
}
```

![image-20211119100126905](https://gitee.com/hqinglau/img/raw/master/img/20211119100126.png)

目前能怎么优化呢？加个map来记录一个数变换到1的次数是很显然的事，这样就可以避免大量重复计算。然后我们就可以写出如下代码。

### 三、Map记录变换次数

```java
private Map<Long,Integer> map = new HashMap<>();

public int integerReplacement(long n) {
    // 递归
    // 偶数：一条路，奇数：加一或者减一
    if(n==1) return 0;
    if(map.containsKey(n)) return map.get(n);
    int cur;
    if(n%2==0) {
        cur = 1 + integerReplacement(n/2);
    } else {
        cur =  1 + Math.min(integerReplacement(n+1),integerReplacement(n-1));
    }
    map.put(n,cur);
    return cur;
}
```

结果好了不少：

![image-20211119100243630](https://gitee.com/hqinglau/img/raw/master/img/20211119100243.png)



可是`leetcoder`仍然不满意，这种简单的递归配叫中等题吗？

### 四、位运算

> 参考链接：[谢尔盖·塔切诺夫](https://leetcode.com/problems/integer-replacement/discuss/87920/A-couple-of-Java-solutions-with-explanations)

从 n 的二进制角度分析：就是把 n 变成`000...001`的最小变换次数。

偶数无需多想，自然是右移一位就完了，次数+1。重点是奇数。

- 奇数-1的话，最后一位直接置0。
- 奇数+1的话，最后一位变0，向前进位。如果前一位为0，则置1，结束；如果前一位为1，置0，接着进位，重复。

很明显，偶数最好，直接折半，那么**奇数要如何选择**呢？要**尽量去掉1**吗？这样移位之后就有尽可能过的偶数了。**但是是有问题的**。看下例子：

```shell
111011 -> 111010 -> 11101 -> 11100 -> 1110 -> 111 -> 1000 -> 100 -> 10 -> 1
```

但是，这个并不是最理想的方法，有更快的：

```shell
111011 -> 111100 -> 11110 -> 1111 -> 10000 -> 1000 -> 100 -> 10 -> 1
```

双方`111011 -> 111010`并`111011 -> 111100`删除相同数量的1，而第二种方式是更好的（+1）。

所以我们需要**删除尽可能多的 1**，在平局的情况下做 +1？并不全是。 `n=3` 不符合该策略，因为`11 -> 10 -> 1`它优于`11 -> 100 -> 10 -> 1`。幸运的是，据测试这应该是唯一的例外。

所以最后的思路就是：奇数如果`n==3`或者`n-1`的1比`n+1`的1少，才选择-1，其它奇数都+1。

代码：

```java
class Solution {
    // 这里n为int是因为最大值111..11，明显应该+1，然后一路/2，次数最少
    // 其最左边有个符号位0，+1之后是1000000000，是最大的负数，仍可继续移位
    public int integerReplacement(int n) {
        int result = 0;
        while(n!=1) {
            if((n&1)==0) {
                n = n >> 1;
            } else if((n==3) || (Integer.bitCount(n+1)>Integer.bitCount(n-1))) {
                n--;
            } else{
                n++;
            }
            result++;
        }
        return result;
    }
}
```

结果：

![image-20211119104504770](https://gitee.com/hqinglau/img/raw/master/img/20211119104504.png)

![image-20211119105915192](https://gitee.com/hqinglau/img/raw/master/img/20211119105915.png)