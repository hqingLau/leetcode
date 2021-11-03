## [367. 有效的完全平方数](https://leetcode-cn.com/problems/valid-perfect-square/)

给定一个 **正整数** `num` ，编写一个函数，如果 `num` 是一个完全平方数，则返回 `true` ，否则返回 `false` 。

**进阶：不要** 使用任何内置的库函数，如 `sqrt` 。

**示例 1：**

```
输入：num = 16
输出：true
```

**示例 2：**

```
输入：num = 14
输出：false
```

**提示：**

- `1 <= num <= 2^31 - 1`



## 二分

看图很容易懂：

![image-20211104002402831](https://gitee.com/hqinglau/img/raw/master/img/20211104002402.png)

java代码如下：

```java
class Solution {
    public boolean isPerfectSquare(int num) {
        // 二分
        if(num == 1) {
            return true;
        }
        long left = 0;
        long right = num;
        long mid = (left+right)/2;
        long tmp;
        while(left <= right) {
            tmp = mid * mid;
            if(tmp == num) {
                return true;
            } else if(tmp > num) {
                right = mid-1;
            } else {
                left = mid+1;
            }
            mid = (left+right)/2;
        }
        return false;
    }
}
```

- 时间复杂度：`O(log(n))`
- 空间复杂度：`O(1)`

![image-20211104001352717](https://gitee.com/hqinglau/img/raw/master/img/20211104001352.png)

## 1+3+5+7解法

一个有趣的解法

找规律：

```
1: 1*1
1+3: 2*2
1+3+5: 3*3
1+3+5+7: 4*4
1+3+5+7+9: 5*5
```

Java代码：

```java
public boolean isPerfectSquare(int num) {
    int i = 1;
    while (num > 0) {
        num -= i;
        i += 2;
    }
    return num == 0;
}
```

- 时间复杂度：`O(sqrt(n))`
- 空间复杂度：`O(1)`

`O(sqrt(n))`效率不如二分法的`O(log(n))`好。

<img src="https://gitee.com/hqinglau/img/raw/master/img/20211104003107.png" alt="image-20211104003106951" style="zoom:67%;" />