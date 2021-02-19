### 所有二叉树的套路都是一样的，一看二叉树直接先把下面的框架写出来：

```Java
//traverse函数可以是任何函数定义
void traverse(TreeNode root) {
    // 前序遍历
    traverse(root.left)
    // 中序遍历
    traverse(root.right)
    // 后序遍历
}
```


## 题目链接
[二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)
