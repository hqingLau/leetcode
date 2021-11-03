## [407. 接雨水 II](https://leetcode-cn.com/problems/trapping-rain-water-ii/)

给你一个 `m x n` 的矩阵，其中的值均为非负整数，代表二维高度图每个单元的高度，请计算图中形状最多能接多少体积的雨水。

**示例 1:**

![img](https://gitee.com/hqinglau/img/raw/master/img/20211103103908.jpeg)

```
输入: heightMap = [[1,4,3,1,3,2],[3,2,1,3,2,4],[2,3,3,2,3,1]]
输出: 4
解释: 下雨后，雨水将会被上图蓝色的方块中。总的接雨水量为1+2+1=4。
```

**示例 2:**

![img](https://gitee.com/hqinglau/img/raw/master/img/20211103103908.jpeg)

```
输入: heightMap = [[3,3,3,3,3],[3,2,2,2,3],[3,2,1,2,3],[3,2,2,2,3],[3,3,3,3,3]]
输出: 10
```

 

**提示:**

- `m == heightMap.length`
- `n == heightMap[i].length`
- `1 <= m, n <= 200`
- `0 <= heightMap[i][j] <= 2 * 104`

##  题解

总体思路：

![](https://gitee.com/hqinglau/img/raw/master/img/20211103122307.png)

详细解释：

![image-20211103105625432](https://gitee.com/hqinglau/img/raw/master/img/20211103105625.png)

![image-20211103105839458](https://gitee.com/hqinglau/img/raw/master/img/20211103105839.png)

![image-20211103110113066](https://gitee.com/hqinglau/img/raw/master/img/20211103110113.png)

![image-20211103110209394](https://gitee.com/hqinglau/img/raw/master/img/20211103110209.png)

![image-20211103110327958](https://gitee.com/hqinglau/img/raw/master/img/20211103110328.png)

![image-20211103110454672](https://gitee.com/hqinglau/img/raw/master/img/20211103110454.png)



![image-20211103110607209](https://gitee.com/hqinglau/img/raw/master/img/20211103110607.png)

![image-20211103110748526](https://gitee.com/hqinglau/img/raw/master/img/20211103110748.png)

![image-20211103114248736](https://gitee.com/hqinglau/img/raw/master/img/20211103114248.png)

![image-20211103114532592](https://gitee.com/hqinglau/img/raw/master/img/20211103114532.png)

![image-20211103114700608](https://gitee.com/hqinglau/img/raw/master/img/20211103114700.png)

![image-20211103114808822](https://gitee.com/hqinglau/img/raw/master/img/20211103114808.png)

![image-20211103115049784](https://gitee.com/hqinglau/img/raw/master/img/20211103115049.png)

![image-20211103115114838](https://gitee.com/hqinglau/img/raw/master/img/20211103115114.png)

![image-20211103115133226](https://gitee.com/hqinglau/img/raw/master/img/20211103115133.png)

![image-20211103115154623](https://gitee.com/hqinglau/img/raw/master/img/20211103115154.png)

![image-20211103115300724](https://gitee.com/hqinglau/img/raw/master/img/20211103115300.png)

![image-20211103115350510](https://gitee.com/hqinglau/img/raw/master/img/20211103115350.png)

![](https://gitee.com/hqinglau/img/raw/master/img/20211103115801.png)

![image-20211103115851887](https://gitee.com/hqinglau/img/raw/master/img/20211103115851.png)

![image-20211103115947172](https://gitee.com/hqinglau/img/raw/master/img/20211103115947.png)

![image-20211103120037463](https://gitee.com/hqinglau/img/raw/master/img/20211103120037.png)

![image-20211103120131583](https://gitee.com/hqinglau/img/raw/master/img/20211103120131.png)

![image-20211103122255597](https://gitee.com/hqinglau/img/raw/master/img/20211103122255.png)

> 素材来自[油管](https://www.youtube.com/watch?v=cJayBq38VYw&ab_channel=SamShen)。



```java
import java.util.Comparator;
import java.util.PriorityQueue;

class Solution {
    private enum COLOR {WHITE, GREEN, GRAY}

    ;

    private class Pos {
        int height;
        int i;
        int j;

        public Pos(int height, int i, int j) {
            this.height = height;
            this.i = i;
            this.j = j;
        }
    }

    public int trapRainWater(int[][] heightMap) {
        if (heightMap.length == 0) {
            return 0;
        }
        COLOR[][] state = new COLOR[heightMap.length][heightMap[0].length];
        PriorityQueue<Pos> heap = new PriorityQueue<>(new Comparator<Pos>() {
            @Override
            public int compare(Pos o1, Pos o2) {
                return o1.height - o2.height;
            }
        });
        for (int i = 0; i < heightMap.length; i++) {
            for (int j = 0; j < heightMap[0].length; j++) {
                if (i == 0 || i == heightMap.length - 1 || j == 0 || j == heightMap[0].length - 1) {
                    state[i][j] = COLOR.GREEN;
                    heap.offer(new Pos(heightMap[i][j], i, j));
                } else {
                    state[i][j] = COLOR.WHITE;
                }
            }
        }
        Pos cur;
        int maxHeight = heap.peek().height;
        int result = 0;
        int nexti, nextj;
        int[][] direction = new int[][]{{-1, 0}, {1, 0}, {0, 1}, {0, -1}};
        while (!heap.isEmpty()) {
            cur = heap.poll();
            maxHeight = maxHeight < cur.height ? cur.height : maxHeight;
            state[cur.i][cur.j] = COLOR.GRAY;
            for (int i = 0; i < direction.length; i++) {
                nexti = cur.i + direction[i][0];
                nextj = cur.j + direction[i][1];
                if (nexti < 0 || nexti >= heightMap.length || nextj < 0 || nextj >= heightMap[0].length || state[nexti][nextj] != COLOR.WHITE) {
                    continue;
                }
                state[nexti][nextj] = COLOR.GREEN;
                if (heightMap[nexti][nextj] >= maxHeight) {
                    heap.offer(new Pos(heightMap[nexti][nextj], nexti, nextj));
                } else {
                    result += maxHeight - heightMap[nexti][nextj];
                    heap.offer(new Pos(maxHeight, nexti, nextj));
                }
            }
        }
        return result;
    }
}
```