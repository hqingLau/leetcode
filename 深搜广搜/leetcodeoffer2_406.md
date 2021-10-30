## [剑指 Offer II 046. 二叉树的右侧视图](https://leetcode-cn.com/problems/WNC0Lk/)题目

给定一个二叉树的 **根节点** `root`，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

**示例 1:**

![img](https://assets.leetcode.com/uploads/2021/02/14/tree.jpg)

```
输入: [1,2,3,null,5,null,4]
输出: [1,3,4]
```

**示例 2:**

```
输入: [1,null,3]
输出: [1,3]
```

**示例 3:**

```
输入: []
输出: []
```

**提示:**

- 二叉树的节点个数的范围是 `[0,100]`
- `-100 <= Node.val <= 100` 

## 一、层序遍历

![image-20211030174213323](https://gitee.com/hqinglau/img/raw/master/img/20211030174213.png)

其中，上一层队列的结束可以通过计数，也可以通过两个队列轮换的形式。

java 代码：

```java
public List<Integer> rightSideView(TreeNode root) {
    List<Integer> result = new LinkedList<Integer>();
    Queue<TreeNode> queue = new LinkedList<>();
    if (root == null) return result;
    queue.offer(root);
    int curLayerNode = 0;
    int lastLayerNode = 1;
    TreeNode p;
    while (!queue.isEmpty()) {
        p = queue.poll();
        lastLayerNode -= 1;
        if (p.left != null) {
            queue.offer(p.left);
            curLayerNode++;
        }
        if (p.right != null) {
            queue.offer(p.right);
            curLayerNode++;
        }
        if (lastLayerNode == 0) {
            result.add(p.val);
            lastLayerNode = curLayerNode;
            curLayerNode = 0;
        }
    }
    return result;
}
```

- 时间复杂度：`O(n)`



## 二、深度搜索

先右后左，最先访问某一层的肯定是最右边。条件：首次访问第x层，就加入结果数组。

![image-20211030175632320](https://gitee.com/hqinglau/img/raw/master/img/20211030175632.png)

![image-20211030175748909](https://gitee.com/hqinglau/img/raw/master/img/20211030175749.png)

![image-20211030175832322](https://gitee.com/hqinglau/img/raw/master/img/20211030175832.png)

![image-20211030175900911](https://gitee.com/hqinglau/img/raw/master/img/20211030175901.png)

![image-20211030175953671](https://gitee.com/hqinglau/img/raw/master/img/20211030175953.png)

![image-20211030180154637](https://gitee.com/hqinglau/img/raw/master/img/20211030180154.png)

![image-20211030180301327](https://gitee.com/hqinglau/img/raw/master/img/20211030180301.png)



java代码：

```java
class Solution {
    public void dfs(List<Integer> result,int depth,TreeNode root) {
        if(root==null) {
            return;
        }
        if(result.size()==depth) {
            result.add(root.val);
        }
        dfs(result,depth+1,root.right);
        dfs(result,depth+1,root.left);
    }

    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> result = new LinkedList<Integer>();
        dfs(result,0,root);
        return result;
    }
}
```

- 时间复杂度：`O(n)`

