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
### 二叉树的出度和入度
- 入度：有多少个节点指向它；
- 出度：它指向多少个节点。

我们知道在树（甚至图）中，**所有节点的入度之和等于出度之和**。可以根据这个特点判断输入序列是否为有效的！
### 二叉树的层级遍历
```Java
void traverse(TreeNode root) {
    if (root == null) return;
    // 初始化队列，将 root 加入队列
    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);

    while (!q.isEmpty()) {
        TreeNode cur = q.poll();

        /* 层级遍历代码位置 */
        System.out.println(root.val);
        /*****************/

        if (cur.left != null) {
            q.offer(cur.left);
        }

        if (cur.right != null) {
            q.offer(cur.right);
        }
    }
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

简单总结下吧，BST 相关的问题，要么利用 BST 左小右大的特性提升算法效率，要么利用中序遍历的特性满足题目的要求，也就这么些事儿吧。

### 二叉搜索树的遍历框架
```Java
void BST(TreeNode root, int target) {
    if (root.val == target)
        // 找到目标，做点什么
    if (root.val < target) 
        BST(root.right, target);
    if (root.val > target)
        BST(root.left, target);
}
```


## 题目链接
[二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)
