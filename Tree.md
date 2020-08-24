# 树

## 先序遍历

* 递归版

```js
const preOrder = function(root) {
    if(!root) return;
    console.log(root.val);
    preOrder(root.left);
    preOrder(root.right);
}
```

* 迭代版

```js
const preOrder = function(root) {
    if(!root) return;
    const stack = [root];
    while(stack.length) {
        const node = stack.pop();
        console.log(node.val);
        if(node.right) stack.pop(node.right);
        if(node.left) stack.pop(node.left);
    }
}
```



## 中序遍历

* 递归版

```js
const inOrder = function(root) {
    if(!root) return;
    inOrder(root.left);
    console.log(root.val);
    inOrder(root.right);
}
```

* 迭代版

```js
const inOrder = function(root) {
    if(!root) return;
    const stack = [];
    let p = root;
    while(stack.length || p) {
        while(n) {
            // 将左子树压栈
            stack.push(p.left);
            p = p.left;
        }
        const node = stack.pop();
        console.log(node.val);
        p = node.right;
    }
}
```



## 后序遍历

* 递归版

```js
const postOrder = function(root) {
    if(!root) return;
    postOrder(root.right);
    postOrder(root.left);
    console.log(root.val);
}
```

* 迭代版

```js
const postOrder = function(root) {
    if(!root) return;
    const stack = [root];
    const outputStack = [];
    while(stack.length) {
        const node = stack.pop();
        outputStack.push(node);
        if(node.right) stack.push(node.right);
        if(node.left) stack.push(node.left);
    }
    while(outputStack.length) {
        const node = outputStack.pop();
        console.log(node.val);
    }
}
```



## 验证二叉搜索树 - 98

给定一个二叉树，判断其是否是一个有效的二叉搜索树。

假设一个二叉搜索树具有如下特征：

* 节点的左子树只包含**小于**当前节点的数。
* 节点的右子树只包含**大于**当前节点的数。
* 所有左子树和右子树自身必须也是二叉搜索树。

**示例1: **

```
输入:
    2
   / \
  1   3
输出: true
```

**示例2: **

```
输入:
    5
   / \
  1   4
     / \
    3   6
输出: false
解释: 输入为: [5,1,4,null,null,3,6]。
     根节点的值为 5 ，但是其右子节点值为 4 。
```



***思路***

* 使用中序遍历，树的左节点小于根节点小于右键点，遍历



***解法 - 中序遍历***

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
var isValidBST = function(root) {
  if(!root) return true;
  let pre = -Infinity;
  const inOrder = function(node) {
    if(!node) return true;
    if(!inOrder(node.left)) return false;
    // 前一个节点大于等于当前节点，不符合二叉搜索树
    if(pre >= node.val) return false;
    // 记录当前节点值，用以比较下一个节点
    pre = node.val; 
    return inOrder(node.right);
  }
  return inOrder(root);
};
```



---

## 二叉搜索树中的插入操作 - 701

给定二叉搜索树（BST）的根节点和要插入树中的值，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。 保证原始二叉搜索树中不存在新值。

注意，可能存在多种有效的插入方式，只要树在插入后仍保持为二叉搜索树即可。 你可以返回任意有效的结果。

例如, 

```
给定二叉搜索树:

        4
       / \
      2   7
     / \
    1   3

和 插入的值: 5
```

你可以返回这个二叉搜索树:

```
         4
       /   \
      2     7
     / \   /
    1   3 5
```

或者这个树也是有效的:

```
         5
       /   \
      2     7
     / \   
    1   3
         \
          4
```



***解法*** - 根据搜索二叉树特性

* 插入值小于当前节点，往左子树寻找合适位置
* 插入值大于当期节点，往右子树寻找合适位置

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
 * @param {TreeNode} root
 * @param {number} val
 * @return {TreeNode}
 */
var insertIntoBST = function(root, val) {
    if(!root) return new TreeNode(val);
    let node = root;
    while(node) {
      if(val < node.val) {
        // val 小于当前节点值，插入到左子树
        if(node.left == null) {
          // 左子树为空，直接插入
          node.left = new TreeNode(val);
          return root;
        } else {
          // 左子树不为空，继续寻找合适位置
          node = node.left;
        }
      } else {
        // val 大于当前节点值，插入到右子树
        if(node.right == null) {
          // 右子树为空，直接插入
          node.right = new TreeNode(val);
          return root;
        } else {
          // 右子树不为空，继续寻找合适位置
          node = node.right;
        }
      }
    }
};
```



---

## 最小高度树 - 面试题 04.02

给定一个有序整数数组，元素各不相同且按升序排列，编写一个算法，创建一棵高度最小的二叉搜索树。

**示例:**

```
给定有序数组: [-10,-3,0,5,9],

一个可能的答案是：[0,-3,9,-10,null,5]，它可以表示下面这个高度平衡二叉搜索树：

          0 
         / \ 
       -3   9 
       /   / 
     -10  5 
```



***解法 - 递归***

* 高度最小，即左右子树高度差尽可能小，选取有序数组的中间值进行递归

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {number[]} nums
 * @return {TreeNode}
 */
var sortedArrayToBST = function(nums) {
  if(!nums) return [];
  const dfs = function(nums, l, r) {
    if(l > r) return null;
    // 向右移一位，相当去取中间值
    const mid = (l + r) >> 1;
    const node = new TreeNode(nums[mid]);
    node.left = dfs(nums, l, mid - 1);
    node.right = dfs(nums, mid + 1, r);
    return node;
  }
  const root = dfs(nums, 0, nums.length - 1);
  return root;
};
```



---

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

## 二叉树的层次遍历 II - 107

给定一个二叉树，返回其节点值自底向上的层次遍历。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）

例如：
给定二叉树 `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

返回其自底向上的层次遍历为：

```
[
  [15,7],
  [9,20],
  [3]
]
```



***解法 - 广度优先遍历***

* 使用 len = q.length 确认每一层的节点数
* .unshift() 在数组头部插入新的数组实现自底向上的层序遍历

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
var levelOrderBottom = function(root) {
  if(!root) return [];
  const q = [root];
  const res = [];
  while(q.length) {
    let len = q.length;
    res.unshift([]);
    while(len--) {
      const n = q.shift();
      res[0].push(n.val);
      if(n.left) q.push(n.left);
      if(n.right) q.push(n.right);
    }
  }
  return res;
};
```



---

## 二叉树的锯齿形层次遍历 - 103

给定一个二叉树，返回其节点值的锯齿形层次遍历。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。

例如：
给定二叉树 `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

返回锯齿形层次遍历如下：

```
[
  [3],
  [20,9],
  [15,7]
]
```



***解法***

* 定义 ltr 判断插入当前层节点值的方向 (push or unshift)

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
var zigzagLevelOrder = function(root) {
  if(!root) return [];
  // 初始化方向为左往右(left to right)
  let ltr = true;
  const q = [root];
  const res = [];
  while(q.length) {
    let len = q.length;
    res.push([]);
    while(len--) {
      const n = q.shift();
      // 根据 ltr 判断遍历方向 (即使用push插入末尾还是使用unshift插入头部)
      if(ltr) {
        res[res.length - 1].push(n.val);
      } else {
        res[res.length - 1].unshift(n.val);
      }
      if(n.left) q.push(n.left);
      if(n.right) q.push(n.right);
    }
    ltr = !ltr;
  }
  return res;
};
```



---

## 填充每个节点的下一个右侧节点指针 - 116

给定一个**完美二叉树**，其所有叶子节点都在同一层，每个父节点都有两个子节点。二叉树定义如下：

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 `NULL`。

初始状态下，所有 next 指针都被设置为 `NULL`。

**示例：**

<img src="D:\Learning\Note\Algorithm\Tree.assets\116_sample.png" alt="img" style="zoom: 50%;" />

```
输入：{"$id":"1","left":{"$id":"2","left":{"$id":"3","left":null,"next":null,"right":null,"val":4},"next":null,"right":{"$id":"4","left":null,"next":null,"right":null,"val":5},"val":2},"next":null,"right":{"$id":"5","left":{"$id":"6","left":null,"next":null,"right":null,"val":6},"next":null,"right":{"$id":"7","left":null,"next":null,"right":null,"val":7},"val":3},"val":1}

输出：{"$id":"1","left":{"$id":"2","left":{"$id":"3","left":null,"next":{"$id":"4","left":null,"next":{"$id":"5","left":null,"next":{"$id":"6","left":null,"next":null,"right":null,"val":7},"right":null,"val":6},"right":null,"val":5},"right":null,"val":4},"next":{"$id":"7","left":{"$ref":"5"},"next":null,"right":{"$ref":"6"},"val":3},"right":{"$ref":"4"},"val":2},"next":null,"right":{"$ref":"7"},"val":1}

解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。
```

**提示：**

- 你只能使用常量级额外空间。
- 使用递归解题也符合要求，本题中递归程序占用的栈空间不算做额外的空间复杂度。

***思路***

* 广度优先遍历，记录每一层节点数
* 当遍历到每层最后一个节点，next指向null；其余节点next指向堆中的第0个

***解法 - 广度优先遍历***

```js
/**
 * // Definition for a Node.
 * function Node(val, left, right, next) {
 *    this.val = val === undefined ? null : val;
 *    this.left = left === undefined ? null : left;
 *    this.right = right === undefined ? null : right;
 *    this.next = next === undefined ? null : next;
 * };
 */

/**
 * @param {Node} root
 * @return {Node}
 */
var connect = function(root) {
    if(!root) return root;
    const q = [root];
    while(q.length) {
      let len = q.length;
      while(len--) {
        const node = q.shift();
        if(len > 0) {
          node.next = q[0];
        } else {
          // 若len为0时，即为当前行最后一个节点
          node.next = null;
        }
        if(node.left) q.push(node.left);
        if(node.right) q.push(node.right);
      }
    }
    return root;
};
```



---

## 填充每个节点的下一个右侧节点指针II - 117

给定一个二叉树

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 `NULL`。

初始状态下，所有 next 指针都被设置为 `NULL`

**进阶：**

- 你只能使用常量级额外空间。
- 使用递归解题也符合要求，本题中递归程序占用的栈空间不算做额外的空间复杂度

**示例：**

<img src="D:\Learning\Note\Algorithm\Tree.assets\117_sample.png" alt="img" style="zoom: 50%;" />

```
输入：root = [1,2,3,4,5,null,7]
输出：[1,#,2,3,#,4,5,7,#]
解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。
```

**提示：**

- 树中的节点数小于 `6000`
- `-100 <= node.val <= 100`



***解法 - 广度优先遍历***

```js
/**
 * // Definition for a Node.
 * function Node(val, left, right, next) {
 *    this.val = val === undefined ? null : val;
 *    this.left = left === undefined ? null : left;
 *    this.right = right === undefined ? null : right;
 *    this.next = next === undefined ? null : next;
 * };
 */

/**
 * @param {Node} root
 * @return {Node}
 */
var connect = function(root) {
    if(!root) return root;
    const q = [root];
    while(q.length) {
      let len = q.length;
      while(len--) {
        const node = q.shift();
        if(len > 0) {
          node.next = q[0];
        } else {
          node.next = null;
        }
        if(node.left) q.push(node.left);
        if(node.right) q.push(node.right);
      }
    }
    return root;
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
    //  详解：若当前节点为 p 或 q，而且其他子树返回结果为 null，这剩下的 q 或 p 必定在当前节点的子树内
    if(root == null || root == p || root == q) return root;
    const left = lowestCommonAncestor(root.left, p, q);
    const right = lowestCommonAncestor(root.right, p, q);
    // 以下为 
    // left 为 null, right 不为 null;
    // left 不为 null, right 为 null; 
    // left, right均为 null的合并
    if(left == null) return right;
    if(right == null) return left;
    // 若 left, right 均不为 null，当前节点为最近公共节点
    return root;
};
```



---

## 合并二叉树 - 617

给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。

你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则**不为** NULL 的节点将直接作为新二叉树的节点。

**示例 1:**

```
输入: 
	Tree 1                     Tree 2                  
          1                         2                             
         / \                       / \                            
        3   2                     1   3                        
       /                           \   \                      
      5                             4   7                  
输出: 
合并后的树:
	     3
	    / \
	   4   5
	  / \   \ 
	 5   4   7
```

**注意:** 合并必须从两个树的根节点开始



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
 * @param {TreeNode} t1
 * @param {TreeNode} t2
 * @return {TreeNode}
 */
var mergeTrees = function(t1, t2) {
  // 将 t2 合并到 t1 中
  // t1 没有节点，直接返回t2
  if(!t1) return t2;
  // t2 没有节点，直接返回t1
  if(!t2) return t1;
  // t1, t2 均存在节点，将节点值相加
  t1.val += t2.val;
  // t1, t2 的左右子树递归
  t1.left = mergeTrees(t1.left, t2.left);
  t1.right = mergeTrees(t1.right, t2.right);
  return t1;
};
```



---

## 对称二叉树 - 101

给定一个二叉树，检查它是否是镜像对称的。

 

例如，二叉树 `[1,2,2,3,4,4,3]` 是对称的。

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
      if(l && r && l.val === r.val && isMirror(l.left, r.right) && isMirror(l.right, r.left)) {
        return true;
      } 
      return false;
    }
    return isMirror(root.left, root.right);
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

## 路径总和 - 112

给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。

说明: 叶子节点是指没有子节点的节点。

示例: 
给定如下二叉树，以及目标和 `sum = 22`

```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
```

返回 `true`, 因为存在目标和为 22 的根节点到叶子节点的路径 `5->4->11->2`



***解法一 - 先序遍历***

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
 * @return {boolean}
 */
var hasPathSum = function(root, sum) {
  if(!root) return false;
  sum = sum - root.val;
  if(!root.left && !root.right) {
    if(sum === 0) {
      return true;
    }
    return false;
  }
  return hasPathSum(root.left, sum) || hasPathSum(root.right, sum);
};
```

***解法二 - 深度优先遍历***

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
 * @return {boolean}
 */
var hasPathSum = function(root, sum) {
  if(!root) return false;
  let find = false;
  const dfs = (n, s) => {
    if(!n.left && !n.right && s === sum) find = true;
    if(n.left) dfs(n.left, s + n.left.val);
    if(n.right) dfs(n.right, s + n.right.val);
  }
  dfs(root, root.val);
  return find;
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



---

## 二叉树中的最大路径和 - 124 ***

给定一个非空二叉树，返回其最大路径和。

本题中，路径被定义为一条从树中任意节点出发，达到任意节点的序列。该路径至少包含一个节点，且不一定经过根节点。

**示例 1:**

```
输入: [1,2,3]

       1
      / \
     2   3

输出: 6
```

**示例 2:**

```
输入: [-10,9,20,null,null,15,7]

   -10
   / \
  9  20
    /  \
   15   7

输出: 42
```



***思路***

* 最大路径和的可能
  * b + a + c
  * b + a + a 的父节点
  * c + a + a 的父节点

```
   a
  / \
 b   c
```



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
 * @return {number}
 */
var maxPathSum = function(root) {
  let maxPath = -2147483648;
  const dfs = function(node) {
    if(!node) return 0;
    const leftPath = dfs(node.left);
    const rightPath = dfs(node.right);
    // 若左右子树的路径和为负数，则舍去
    const leftPath_max = leftPath < 0 ? 0 : leftPath;
    const rightPath_max = rightPath < 0 ? 0 : rightPath;
    // 最大路径和情况一: left -> node -> right (lnr)
    const lnr = leftPath_max + node.val + rightPath_max;
    // 更新最大路径
    maxPath = Math.max(maxPath, lnr);
    // 最大路径和情况二, 三: 单边最大路径和 + 当前节点的父节点
    return node.val + Math.max(leftPath_max, rightPath_max);
  }

  dfs(root);
  return maxPath;
};
```



---

## 从中序与后序遍历序列构造二叉树 - 106

根据一棵树的中序遍历与后序遍历构造二叉树。

**注意:**
你可以假设树中没有重复的元素。

例如，给出

```
中序遍历 inorder = [9,3,15,20,7]
后序遍历 postorder = [9,15,7,20,3]
```

返回如下的二叉树：

```
    3
   / \
  9  20
    /  \
   15   7
```

***思路***

* 后序遍历的最后一个节点为当前的根节点
* 中序遍历中根节点(位置为index)左边为当前根节点的左子树(0, index-1)，数量为左子树中节点的总数(index-1-0+1)；右边为右子树
* 根据中序遍历中左子树节点总数可以得到后序遍历中左子树的部分(前index个)，然后是右子树节点
* 依次遍历，返回每次递归的根节点

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
 * @param {number[]} inorder
 * @param {number[]} postorder
 * @return {TreeNode}
 */
var buildTree = function(inorder, postorder) {
  const post_end = postorder[postorder.length - 1]; // 根节点
  if(!post_end && post_end !== 0) return null;
  const root = new TreeNode(post_end);
  const index = inorder.indexOf(post_end);
  // 当前根节点位置为 index, 则根节点的左子树为中序遍历中的 index 左侧(0, index-1)
  // 左子树节点数量为 index - 1 - 0 + 1 = index 个
  // 因为节点数量一致，根节点的左子树为后序遍历中的 前index个 (0, index - 1)
  const inorder_left = inorder.slice(0, index);
  const postorder_left = postorder.slice(0, index);
  // 右子树同理
  const inorder_right = inorder.slice(index + 1);
  const postorder_right = postorder.slice(index, -1);

  root.left = buildTree(inorder_left, postorder_left);
  root.right = buildTree(inorder_right, postorder_right);

  return root;
};
```



---

## 从前序与中序遍历序列构造二叉树 - 105

根据一棵树的前序遍历与中序遍历构造二叉树。

**注意:**
你可以假设树中没有重复的元素。

例如，给出

```
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
```

返回如下的二叉树：

```
    3
   / \
  9  20
    /  \
   15   7
```

***思路***

* 由前序遍历获取当前根节点
* 从中序遍历中找出左右子树
* 递归

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
 * @param {number[]} preorder
 * @param {number[]} inorder
 * @return {TreeNode}
 */
var buildTree = function(preorder, inorder) {
  const pre_start = preorder[0];
  if(!pre_start && pre_start !== 0) return null;
  const index = inorder.indexOf(pre_start);
  const root = new TreeNode(pre_start);
  // 前序遍历第一个元素为当前的根节点
  // 中序遍历根节点左侧为左子树，右侧为右子树
  const inorder_left = inorder.slice(0,index);
  // 获取左子树节点总数
  const num_left = inorder_left.length;
  const preorder_left = preorder.slice(1, num_left + 1);
  
  const inorder_right = inorder.slice(index + 1);
  const preorder_right = preorder.slice(num_left + 1);

  root.left = buildTree(preorder_left,inorder_left);
  root.right = buildTree(preorder_right, inorder_right);

  return root;
};
```



---

## 根据前序和后序遍历构造二叉树 - 889

返回与给定的前序和后序遍历匹配的任何二叉树。

 `pre` 和 `post` 遍历中的值是不同的正整数。

 

**示例：**

```
输入：pre = [1,2,4,5,3,6,7], post = [4,5,2,6,7,3,1]
输出：[1,2,3,4,5,6,7]
```

**提示：**

* `1 <= pre.length == post.length <= 30`
* `pre[]` 和 `post[]` 都是 `1, 2, ..., pre.length` 的排列
* 每个输入保证至少有一个答案。如果有多个答案，可以返回其中一个。



***思路***

* 根据前序遍历获取根节点及左子树的根节点
* 根据左子树的根节点判断后序遍历中属于当前节点左子树的部分
* 接下来的思路与前面两题(105,106)类似

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
 * @param {number[]} pre
 * @param {number[]} post
 * @return {TreeNode}
 */
var constructFromPrePost = function(pre, post) {
  const pre_start = pre[0];
  if(!pre_start && pre_start !== 0) return null;
  const root = new TreeNode(pre_start);
  // 当前左子树的根节点
  const pre_left_root = pre[1];
  // post_left_index 的左侧为所有左子树节点
  const post_left_index = post.indexOf(pre_left_root);

  const post_left = post.slice(0, post_left_index + 1);
  const post_left_length = post_left.length;
  const pre_left = pre.slice(1, 1 + post_left_length);

  const post_right = post.slice(post_left_index + 1);
  const pre_right = pre.slice(1 + post_left_length);

  root.left = constructFromPrePost(pre_left, post_left);
  root.right = constructFromPrePost(pre_right, post_right);

  return root;

};
```



---

## 二叉树的镜像 - 剑指 Offer 27

请完成一个函数，输入一个二叉树，该函数输出它的镜像。

例如输入：

```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

镜像输出：

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

**示例 1：**

```
输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
```



***解法 - 递归交换左右子树***

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
var mirrorTree = function(root) {
  if(root){
    const temp = root.left;
    root.left = root.right;
    root.right = temp;
    mirrorTree(root.left);
    mirrorTree(root.right);
  }
  return root;
};
```

