# 分而治之

## 翻转二叉树 - 226

翻转一棵二叉树。

**示例：**

输入：

```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

输出：

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```



***解法***

* 分：获取左右子树
* 解：递归地翻转左右子树
* 合：将翻转后的左右子树换位置放到根节点上

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {TreeNode}
 */
var invertTree = function(root) {
  if(root) {
    invertTree(root.left);
    invertTree(root.right);
    const temp = root.left;
    root.left = root.right;
    root.right = temp;
  }
  return root;
};
```



----

## 相同的树 - 100

给定两个二叉树，编写一个函数来检验它们是否相同。

如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。

**示例 1:**

```
输入:       1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]

输出: true
```

**示例 2:**

```
输入:      1          1
          /           \
         2             2

        [1,2],     [1,null,2]

输出: false
```

**示例 3:**

```
输入:       1         1
          / \       / \
         2   1     1   2

        [1,2,1],   [1,1,2]

输出: false
```



***解法***

* 时间复杂度：O(n)
* 空间复杂度：O(n)

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {boolean}
 */
var isSameTree = function(p, q) {
    if(!p && !q) return true;
    if(p && q && p.val === q.val &&
      isSameTree(p.left, q.left) && 
      isSameTree(p.right, q.right)
    ) {
      return true;
    }
    return false;
};
```



----

## 对称二叉树 - 101

给定一个二叉树，检查它是否是镜像对称的。

 

例如，二叉树 `[1,2,2,3,4,4,3]` 是对称的

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

但是下面这个 `[1,2,2,null,3,null,3]` 则不是镜像对称的:

```
    1
   / \
  2   2
   \   \
   3    3
```



***解法***

* 时间复杂度：O(n)
* 空间复杂度：O(n)

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {boolean}
 */
var isSymmetric = function(root) {
  if(!root) return true;
  const isMirror = function(l, r) {
    if(!l && !r) return true;
    if(l && r && l.val === r.val &&
        isMirror(l.left, r.right) &&
        isMirror(l.right, r.left)
    ) {
      return true;
    }
    return false;
  }
  return isMirror(root.left, root.right);
};
```

