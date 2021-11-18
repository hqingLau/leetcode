## [563. 二叉树的坡度](https://leetcode-cn.com/problems/binary-tree-tilt/)

给定一个二叉树，计算 **整个树** 的坡度 。

一个树的 **节点的坡度** 定义即为，该节点左子树的节点之和和右子树节点之和的 **差的绝对值** 。如果没有左子树的话，左子树的节点之和为 0 ；没有右子树的话也是一样。空结点的坡度是 0 。

**整个树** 的坡度就是其所有节点的坡度之和。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/20/tilt1.jpg)

```
输入：root = [1,2,3]
输出：1
解释：
节点 2 的坡度：|0-0| = 0（没有子节点）
节点 3 的坡度：|0-0| = 0（没有子节点）
节点 1 的坡度：|2-3| = 1（左子树就是左子节点，所以和是 2 ；右子树就是右子节点，所以和是 3 ）
坡度总和：0 + 0 + 1 = 1
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/10/20/tilt2.jpg)

```
输入：root = [4,2,9,3,5,null,7]
输出：15
解释：
节点 3 的坡度：|0-0| = 0（没有子节点）
节点 5 的坡度：|0-0| = 0（没有子节点）
节点 7 的坡度：|0-0| = 0（没有子节点）
节点 2 的坡度：|3-5| = 2（左子树就是左子节点，所以和是 3 ；右子树就是右子节点，所以和是 5 ）
节点 9 的坡度：|0-7| = 7（没有左子树，所以和是 0 ；右子树正好是右子节点，所以和是 7 ）
节点 4 的坡度：|(3+5+2)-(9+7)| = |10-16| = 6（左子树值为 3、5 和 2 ，和是 10 ；右子树值为 9 和 7 ，和是 16 ）
坡度总和：0 + 0 + 0 + 2 + 7 + 6 = 15
```

**示例 3：**

![img](https://assets.leetcode.com/uploads/2020/10/20/tilt3.jpg)

```
输入：root = [21,7,14,1,1,2,2,3,3]
输出：9
```

 

**提示：**

- 树中节点数目的范围在 `[0, 104]` 内
- `-1000 <= Node.val <= 1000`

## 题解

首先，用map记录每个结点的左右子树之和是必须的，减少重复计算。

![image-20211118100850695](https://gitee.com/hqinglau/img/raw/master/img/20211118100850.png)

然后就可以随便遍历结点，求取所有结点左右子树之差。

![image-20211118100922956](https://gitee.com/hqinglau/img/raw/master/img/20211118100922.png)

这里可以用前中后序或者层序遍历都可以。例如递归前续遍历：

```java
// 记录每个结点的有用信息，也就是当前结点所有子树的和
private Map<TreeNode,Integer> map = new HashMap<>();

public int findTilt(TreeNode root) {
    if (root == null) { return 0; }
    int dif = Math.abs(subTreeSum(root.left) - subTreeSum(root.right));
    int left = findTilt(root.left);
    int right = findTilt(root.right);
    return dif + left + right;
}

private int subTreeSum(TreeNode root) {
    if(root == null) return 0;
    if(map.containsKey(root)) {
        return map.get(root);
    }
    int ret = root.val + subTreeSum(root.left) + subTreeSum(root.right);
    map.put(root,ret);
    return ret;
}
```

