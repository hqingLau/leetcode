一个 2D 网格中的 顶峰元素 是指那些 严格大于 其相邻格子(上、下、左、右)的元素。

给你一个 从 0 开始编号 的 `m x n` 矩阵 `mat` ，其中任意两个相邻格子的值都 不相同 。找出 任意一个 顶峰元素 `mat[i][j]` 并 返回其位置 `[i,j]` 。

你可以假设整个矩阵周边环绕着一圈值为 -1 的格子。

要求必须写出时间复杂度为 `O(m log(n))` 或 `O(n log(m))` 的算法

示例 1:

![image-20211030143632079](https://gitee.com/hqinglau/img/raw/master/img/20211030143632.png)

```shell
输入: mat = [[1,4],[3,2]]
输出: [0,1]
解释: 3和4都是顶峰元素，所以[1,0]和[0,1]都是可接受的答案。
```

示例 2:

![image-20211030143547833](https://gitee.com/hqinglau/img/raw/master/img/20211030143618.png)

```shell
输入: mat = [[10,20,15],[21,30,14],[7,16,32]]
输出: [1,1]
解释: 30和32都是顶峰元素，所以[1,1]和[2,2]都是可接受的答案。
```

提示：

```shell
m == mat.length
n == mat[i].length
1 <= m, n <= 500
1 <= mat[i][j] <= 105
任意两个相邻元素均不相等.
```

题目要求：**时间复杂度为 `O(m log(n))` 或 `O(n log(m))` 的算法**

## 一、暴力（不符合复杂度要求）

先写出`O(mn)`复杂度的算法，逐项遍历，这里面要注意的就是边界条件的判断：

```java
class Solution {
    // 判断矩阵中的一个元素是否属于顶峰元素
    boolean isPeek(int[][] mat, int i, int j) {
        if ((i >= 1 && mat[i][j] < mat[i - 1][j]) || 
                (i < mat.length - 1 && mat[i][j] < mat[i + 1][j]) || 
                (j >= 1 && mat[i][j] < mat[i][j - 1]) || 
                (j < mat[0].length - 1 && mat[i][j] < mat[i][j + 1])) {
            return false;
        }
        return true;
    }

    public int[] findPeakGrid(int[][] mat) {
        for (int i = 0; i < mat.length; i++) {
            for (int j = 0; j < mat[0].length; j++) {
                if (isPeek(mat, i, j)) {
                    return new int[]{i, j};
                }
            }
        }
        return null;
    }
}
```

- 时间复杂度：`O(mn)`

## 二、行二分

刚才的暴力遍历有一个条件没用到：相邻元素各不相同。

二分思想图解：

![image-20211030154350281](https://gitee.com/hqinglau/img/raw/master/img/20211030154350.png)

![image-20211030155145442](https://gitee.com/hqinglau/img/raw/master/img/20211030155145.png)

![image-20211030155421865](https://gitee.com/hqinglau/img/raw/master/img/20211030155421.png)

![image-20211030160920747](https://gitee.com/hqinglau/img/raw/master/img/20211030160920.png)

二分法代码：

```java
class Solution {
    // 返回行最大值的列号
    public int maxOfRow(int[][] mat, int row) {
        if (row < 0 || row >= mat.length) {
            return -1;
        }
        int col = 0;
        for (int i = 1; i < mat[row].length; i++) {
            if (mat[row][i] > mat[row][col]) {
                col = i;
            }
        }
        return col;
    }

    public int[] findPeakGrid(int[][] mat) {
        int top = 0;
        int down = mat.length - 1;
        int mid;
        // m1 mid前一行最大值列号，m2:mid最大值列号，m3:mid+1行最大值列号
        int m1, m2, m3;
        int v1, v2, v3; //中间三行对应的最大值
        while (top <= down) {
            mid = (top + down) / 2;
            //System.out.printf("%d %d %d\n",top,mid,down);
            m2 = maxOfRow(mat, mid);
            if (top == down) {
                return new int[]{mid, m2};
            }
            m1 = maxOfRow(mat, mid - 1);
            m3 = maxOfRow(mat, mid + 1);

            v1 = mid - 1 >= 0 ? mat[mid - 1][m1] : -1;
            v2 = mat[mid][m2];
            v3 = mid + 1 < mat.length ? mat[mid + 1][m3] : -1;
            // 中间行最大，直接顶峰
            if (v2 > v3 && v2 > v1) {
                return new int[]{mid, m2};
            }
            // mid-1行最大
            if (v1 > v3 && v1 >= v2) {
                down = mid - 1;
            } else {
                // mid+1行最大
                top = mid + 1;
            }
        }
        return null;
    }
}
```

结果：

![image-20211030163713413](https://gitee.com/hqinglau/img/raw/master/img/20211030163713.png)