算法的关键点是：**始终从个数最多的球里面逐层挑选**，来最大限度提升利润。

例如，A球有8个，B球有6个，那么获取最大利润的方法就是尽可能的**贪婪**，先从A里面抓一个，得到8分，A球剩7个，仍然从A球里面抓一个，得到7分。

这里面不需要一个一个算，可以用等差数列计算：

```
Sn= a1*n+[n*(n-1)*d]/2
```

图解：

![image-20211029000559356](https://gitee.com/hqinglau/img/raw/master/img/20211029000559.png)![image-20211029000657380](https://gitee.com/hqinglau/img/raw/master/img/20211029000657.png)![image-20211029000954013](https://gitee.com/hqinglau/img/raw/master/img/20211029000954.png)![image-20211029001055623](https://gitee.com/hqinglau/img/raw/master/img/20211029001055.png)![image-20211029001318445](https://gitee.com/hqinglau/img/raw/master/img/20211029001318.png)

![image-20211029001308255](https://gitee.com/hqinglau/img/raw/master/img/20211029001308.png)![image.png](https://pic.leetcode-cn.com/1635437878-vNZpfx-image.png)





java代码实现如下：

```java
public static int maxProfit(int[] inventory, int orders) {
    // 很明显，应该先卖个数最多的球
    int mod = 1000000007;
    Arrays.sort(inventory);
    int curIndex = inventory.length-1;
    int curValue = inventory[curIndex];

    long profit = 0;
    while(orders>0) {
        while(curIndex>=0 && inventory[curIndex]==curValue) {
            curIndex--;
        }
        // 令最少的为0球，防止越界
        int nextValue = curIndex<0?0:inventory[curIndex];
        // 目前球相等的个数
        int numSameColor = inventory.length-1-curIndex;
        // 将要买的球的个数
        int nums = (curValue-nextValue)*numSameColor;

        if(orders>nums) {
            // 如果还可以买的个数较多，就直接全买
            profit += (long)(curValue + nextValue + 1) * (curValue - nextValue) / 2 * numSameColor;
        } else {
            // 不能全买
            int numFullRow = orders/numSameColor;
            int remainder = orders%numSameColor;
            profit += (long)(curValue + curValue - numFullRow + 1) * numFullRow / 2 * numSameColor;
            profit += (long)(curValue - numFullRow) * remainder;
        }
        orders -= nums;
        profit = profit % mod;
        curValue = nextValue;
    }
    return (int)profit;
}
```



如果本文对您有帮助的话，不妨点个赞，也欢迎关注我的[公众号](https://gitee.com/hqinglau/img/raw/master/img/20211028215948.png)，不定时抽奖发布送书活动。本题解同步发布到我的[github题解](https://github.com/hqingLau/leetcode/wiki/index#hq%E7%9A%84%E5%88%B7%E9%A2%98%E6%97%A5%E8%AE%B0)，欢迎star，一起变强吧！