## [391. 完美矩形](https://leetcode-cn.com/problems/perfect-rectangle/)

给你一个数组 `rectangles` ，其中 `rectangles[i] = [xi, yi, ai, bi]` 表示一个坐标轴平行的矩形。这个矩形的左下顶点是 `(xi, yi)` ，右上顶点是 `(ai, bi)` 。

如果所有矩形一起精确覆盖了某个矩形区域，则返回 `true` ；否则，返回 `false` 。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/27/perectrec1-plane.jpg)

```
输入：rectangles = [[1,1,3,3],[3,1,4,2],[3,2,4,4],[1,3,2,4],[2,3,3,4]]
输出：true
解释：5 个矩形一起可以精确地覆盖一个矩形区域。 
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/03/27/perfectrec2-plane.jpg)

```
输入：rectangles = [[1,1,2,3],[1,3,2,4],[3,1,4,2],[3,2,4,4]]
输出：false
解释：两个矩形之间有间隔，无法覆盖成一个矩形。
```

**示例 3：**

![img](https://assets.leetcode.com/uploads/2021/03/27/perfectrec3-plane.jpg)

```
输入：rectangles = [[1,1,3,3],[3,1,4,2],[1,3,2,4],[3,2,4,4]]
输出：false
解释：图形顶端留有空缺，无法覆盖成一个矩形。
```

**示例 4：**

![img](https://assets.leetcode.com/uploads/2021/03/27/perfecrrec4-plane.jpg)

```
输入：rectangles = [[1,1,3,3],[3,1,4,2],[1,3,2,4],[2,2,4,4]]
输出：false
解释：因为中间有相交区域，虽然形成了矩形，但不是精确覆盖。
```



**提示：**

- `1 <= rectangles.length <= 2 * 104`
- `rectangles[i].length == 4`
- `-105 <= xi, yi, ai, bi <= 105`

## 题解：

查角点。

何为角点：

![image-20211116110151676](https://gitee.com/hqinglau/img/raw/master/img/20211116110151.png)

怎么查？



![image-20211116102837222](https://gitee.com/hqinglau/img/raw/master/img/20211116102837.png)



特殊情况可以用面积进行额外验证。

代码：

```java
class Point {
    int x;
    int y;

    Point(int i, int j) {
        x = i;
        y = j;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Point point = (Point) o;
        return x == point.x && y == point.y;
    }

    @Override
    public int hashCode() {
        return Objects.hash(x, y);
    }
}

class Solution {
    private void putMap(Map<Point, Integer> map, int[] rect) {
        Point point;
        int[][] pos = {{0, 1}, {2, 3}, {0, 3}, {2, 1}};
        for (int i = 0; i < 4; i++) {
            point = new Point(rect[pos[i][0]], rect[pos[i][1]]);
            map.put(point, map.getOrDefault(point, 0) + 1);
        }
    }

    public boolean isRectangleCover(int[][] rectangles) {
        Map<Point, Integer> map = new HashMap<>();
        for (int i = 0; i < rectangles.length; i++) {
            putMap(map, rectangles[i]);
        }
        int result = 0;
        // 最后大矩形的角点
        int minX = Integer.MAX_VALUE, minY = Integer.MAX_VALUE;
        int maxX = 0, maxY = 0;
        for (Point p : map.keySet()) {
            Integer integer = map.get(p);
            if (integer % 2 == 1) {
                result++;
                if(result > 4 ) {
                    return false;
                }
                if(p.x < minX) minX = p.x;
                if(p.x > maxX) maxX = p.x;
                if(p.y < minY) minY = p.y;
                if(p.y > maxY) maxY = p.y;
            }
        }
        
        // 验证是否是意外情况，验证面积
        int area = 0;
        for (int i = 0; i < rectangles.length; i++) {
            area += (rectangles[i][2]-rectangles[i][0]) * (rectangles[i][3]-rectangles[i][1]);
        }
        return area == (maxX - minX)*(maxY - minY);
    }
}
```

