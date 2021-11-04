## [404. 左叶子之和](https://leetcode-cn.com/problems/sum-of-left-leaves/)

计算给定二叉树的所有左叶子之和。

**示例：**

```
    3
   / \
  9  20
    /  \
   15   7

在这个二叉树中，有两个左叶子，分别是 9 和 15，所以返回 24
```

## 一、DFS递归

找到所有的左节点，如图：

![image-20211104192005979](https://gitee.com/hqinglau/img/raw/master/img/20211104192006.png)

![image-20211104192153070](https://gitee.com/hqinglau/img/raw/master/img/20211104192153.png)

代码如下：

```java
public int sumOfLeftLeaves(TreeNode root) {
    if(root == null) {
        return 0;
    }
    if(root.left!=null && root.left.left==null && root.left.right==null) {
        return root.left.val + sumOfLeftLeaves(root.right);
    }
    return sumOfLeftLeaves(root.left) + sumOfLeftLeaves(root.right);
}
```

## 二、DFS迭代

```java
public int sumOfLeftLeaves(TreeNode root) {
    if(root == null) {
        return 0;
    }
    Stack<TreeNode> stack = new Stack<>();
    stack.push(root);
    TreeNode cur = root;
    int result = 0;
    while(true) {
        if(cur.left!=null) {
            if(cur.left.left==null && cur.left.right==null) {
                result+=cur.left.val;
            }
            stack.push(cur.left);
            cur = cur.left;
        } else {
            do {
                if(stack.isEmpty()) {
                    return result;
                }
                cur = stack.pop().right;
            } while(cur==null);
            stack.push(cur);
        }
    }
}
```

## 三、BFS迭代

```java
public int sumOfLeftLeaves(TreeNode root) {
    if(root == null) {
        return 0;
    }
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    TreeNode cur;
    int result = 0;
    while(!queue.isEmpty()) {
        cur = queue.poll();
        if(cur.left!=null) {
            if(cur.left.left==null && cur.left.right==null) {
                result+=cur.left.val;
            } else {
                queue.offer(cur.left);
            }
        }
        if(cur.right!=null) {
            queue.offer(cur.right);
        }
    }
    return result;
}
```

## 四、更多

类似问题：[199. 二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view/)

【树】博客：[数据结构-树](https://orzlinux.cn/blog/tree-20211020.html)