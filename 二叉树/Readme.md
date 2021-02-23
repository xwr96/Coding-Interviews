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

### 二叉树序列化
现在，明确了要用后序遍历，那应该怎么描述一棵二叉树的模样呢？我们前文 序列化和反序列化二叉树 其实写过了，二叉树的前序/中序/后序遍历结果可以描述二叉树的结构。

所以，我们可以通过**拼接字符串的方式**把二叉树序列化，看下代码：
```Java
String traverse(TreeNode root) {
    // 对于空节点，可以用一个特殊字符表示
    if (root == null) {
        return "#";
    }
    // 将左右子树序列化成字符串
    String left = traverse(root.left);
    String right = traverse(root.right);
    /* 后序遍历代码位置 */
    // 左右子树加上自己，就是以自己为根的二叉树序列化结果
    String subTree = left + "," + right + "," + root.val;
    return subTree;
}
```
我们用非数字的特殊符#表示空指针，并且用字符,分隔每个二叉树节点值，这属于**序列化二叉树的套路**了，不多说。

### 二叉搜索树
首先，BST 的特性大家应该都很熟悉了：

1、对于 BST 的每一个节点node，左子树节点的值都比node的值要小，右子树节点的值都比node的值大。

2、对于 BST 的每一个节点node，它的左侧子树和右侧子树都是 BST。

从做算法题的角度来看 BST，除了它的定义，还有一个重要的性质：BST 的**中序遍历结果是有序的（升序）。**

## 题目链接
[二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)
