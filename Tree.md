# 树

## 二叉树的最大深度 - 104

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

**说明: **叶子节点是指没有子节点的节点。

**示例：**
给定二叉树 <code>[3,9,20,null,null,15,7]</code>，

```
    3
   / \
  9  20
    /  \
   15   7
```

返回它的最大深度 3 。

***解法一 - 深度优先遍历 DFS***

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
 * @return {number}
 */
var maxDepth = function(root) {
	if(!root) return 0;
    let res = 0;
    function dfs(n, l) {
        // l为当前层级
        res = res > l ? res : l;
        if(n.left) dfs(n.left, l + 1);
        if(n.right) dfs(n.right, l + 1);
    }
    dfs(root, 1);
    return res;
}
```

***解法二 - 广度优先遍历 BFS***

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
 * @return {number}
 */
var maxDepth = function(root) {
  if(!root) return 0;
  let res = 0;
  function bfs(n) {
    const q = [n];
    while(q.length) {
      // 记录当前层级的节点总数，确定将当前层级节点的子节点放入队列需要的最大循环的次数
      const length = q.length
      for(let i = 0; i < length; i++) {
        // 将当前层级的节点的子节点放入队列中
        const nd = q.shift();
        if(nd.left) q.push(nd.left);
        if(nd.right) q.push(nd.right);
      }
      res++;
    }
  }
  bfs(root);
  return res;
};
```



---

## 二叉树的最小深度 - 111

给定一个二叉树，找出其最小深度。

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

**说明: **叶子节点是指没有子节点的节点。

**示例:**

给定二叉树 <code>[3,9,20,null,null,15,7]</code>,

```
    3
   / \
  9  20
    /  \
   15   7
```

返回它的最小深度  2.



***解法 - 广度遍历 BFS***

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
 * @return {number}
 */
var minDepth = function(root) {
  if(!root) return 0;
  let res = 1;
  const q = [root];
  while(q.length) {
    const length = q.length;
    for(let i = 0; i < length; i++) {
      // 广度遍历当前层级的节点，若发现叶子节点(没有子节点)，直接返回最小深度
      const n = q.shift();
      if(!n.left && !n.right) return res;
      if(n.left) q.push(n.left);
      if(n.right) q.push(n.right);
    }
    res++;
  }
  return res;
};

// 改进版
var minDepth = function(root) {
  if(!root) return 0;
  const q = [[root, 1]];
  while(q.length) {
    const [n, l] = q.shift();
    if(!n.left && !n.right) return l;
    if(n.left) q.push([n.left, l + 1]);
    if(n.right) q.push([n.right, l + 1]);
  }
};
```



---

## 二叉树的层序遍历 - 102

给你一个二叉树，请你返回其按 层序遍历 得到的节点值。 （即逐层地，从左到右访问所有节点）。

 

**示例：**
二叉树：<code>[3,9,20,null,null,15,7]</code>。

```
    3
   / \
  9  20
    /  \
   15   7
```

返回其层次遍历结果:

```
[
  [3],
  [9,20],
  [15,7]
]
```

***解法 - 广度优先遍历 BFS***

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
 * @return {number[][]}
 */
// 解法一
var levelOrder = function(root) {
  if(!root) return [];

  const res = [];
  let mark = 0;   // 设置记录当前层级
  const q = [[root, 1]];

  while(q.length) {
    const [n, l] = q.shift();
    if(mark < l) {
      // 若遍历至下一层级，新开一个数组存放当前层级的节点
      mark++;
      res[mark - 1] = [];
    } 
    res[mark - 1].push(n.val);
    if(n.left) q.push([n.left, l + 1]);
    if(n.right) q.push([n.right, l + 1]);
  }
  return res;
};

// 解法二
var levelOrder = function(root) {
  if(!root) return [];
  const q = [root];
  const res = [];
  while(q.length) {
    let len = q.length;
    res.push([]);
    while(len--) {
      // len为当前行节点的剩余个数，保证此次循环弹出的节点均为同一行的节点
      const n = q.shift();
      res[res.length - 1].push(n.val);
      if(n.left) q.push(n.left);      
      if(n.right) q.push(n.right);      
    }
  }
  return res;
};
```



---

## 二叉树的最近公共祖先 - 236 ***

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]

![](D:\Learning\Note\Algorithm\Tree.assets\binarytree.png)

**示例 1：**

```
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出: 3
解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
```

**示例 2：**

```
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出: 5
解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。
```

**说明:**

- 所有节点的值都是唯一的。
- p、q 为不同节点且均存在于给定的二叉树中。



***解法***

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
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {TreeNode}
 */
var lowestCommonAncestor = function(root, p, q) {
    // 1.在节点的左右子树中分别找到p, q，该节点为最近公共祖先
    // 2.该节点为p或q, 剩余另一个节点在该节点的子树中，该节点为最近公共祖先
    if(root == null || root == p || root == q) return root;
    const left = lowestCommonAncestor(root.left, p, q);
    const right = lowestCommonAncestor(root.right, p, q);
    if(left == null) return right;
    if(right == null) return left;
    return root;
};
```



---

## 平衡二叉树 - 110

给定一个二叉树，判断它是否是高度平衡的二叉树。

本题中，一棵高度平衡二叉树定义为：

一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过1。

**示例 1:**

给定二叉树 ``[3,9,20,null,null,15,7]``

```
    3
   / \
  9  20
    /  \
   15   7
```

返回 `true` 。

**示例 2:**

给定二叉树 `[1,2,2,3,3,null,null,4,4]`**:**

```
       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
```

返回 `false` 。



***思路 - 后序遍历***

* 自下而上，使用后序遍历
* 判断左右子树的深度差是否大于1，若大于1，则返回-1，该二叉树不是平衡二叉树，结束递归判断；若小于1，则返回当前左右子树的最大高度 + 1，作为该子树的根节点的高度继续判断



***步骤***

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
var isBalanced = function(root) {
  const postOrder = function(node) {
    // node为空时存在两种情况，root为空或到达二叉树叶子节点
    if(!node) return 0;
    let l = postOrder(node.left);
    if(l === -1) return -1;
    let r = postOrder(node.right);
    if(r === -1) return -1;
    if(Math.abs(l - r) > 1) {
      return -1;
    } else {
      return Math.max(l, r) + 1;
    }
  }
  return postOrder(root) !== -1;
};
```



---

## 路径总和 III - 437 **

给定一个二叉树，它的每个结点都存放着一个整数值。

找出路径和等于给定数值的路径总数。

路径不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

二叉树不超过1000个节点，且节点数值范围是 [-1000000,1000000] 的整数。

**示例：**

```
root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

返回 3。和等于 8 的路径有:

1.  5 -> 3
2.  5 -> 2 -> 1
3.  -3 -> 11
```

***解法***

* pathSum 遍历以二叉树的每个节点作为根节点
* dfs 遍历以当前节点为根节点的二叉树是否存在满足条件的路径，用res作为累加器

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
 * @param {number} sum
 * @return {number}
 */
var pathSum = function(root, sum) {
  if(!root) return 0;
  let res = 0;
  res += dfs(root, sum);
  res += pathSum(root.left, sum);
  res += pathSum(root.right, sum);
  return res;
};

var dfs = function(n,sum) {
  if(!n) return 0;
  let res = 0;
  sum -= n.val;
  if(sum === 0) res++;
  res += dfs(n.left, sum);
  res += dfs(n.right, sum);
  return res;
}
```
