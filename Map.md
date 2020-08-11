# 图

## 有效数字 - 65 *

验证给定的字符串是否可以解释为十进制数字

例如

`"0"` => `true`
`" 0.1 "` => `true`
`"abc"` => `false`
`"1 a"` => `false`
`"2e10"` =>`true`
`" -90e3   "` => `true`
`" 1e"` => `false`
`"e3"` => `false`
`" 6e-1"` => `true`
`" 99e2.5 "` => `false`
`"53.5e93"` => `true`
`" --6 "` => `false`
`"-+3"` => `false`
`"95a54e53"` => `false`

说明: 我们有意将问题陈述地比较模糊。在实现代码之前，你应当事先思考所有可能的情况。这里给出一份可能存在于有效十进制数字中的字符列表：

数字 0-9
指数 - "e"
正/负号 - "+"/"-"
小数点 - "."
当然，在输入中，这些字符的上下文也很重要



***解法***

<img src="D:\Learning\Note\Algorithm\Map.assets\image-20200803103723057.png" alt="image-20200803103723057" style="zoom:67%;" />

* 根据状态图编写邻接表
* 对字符串中的每个字符进行状态的比较，最后状态在3, 5, 6的字符串可解释为十进制数字

```js
/**
 * @param {string} s
 * @return {boolean}
 */
var isNumber = function(s) {
  const graph = {
    0: {"blank": 0, "signs" : 1, "nums": 6, ".": 2},
    1: {"nums": 6, ".": 2},
    2: {"nums": 3},
    3: {"nums": 3, "e": 4},
    4: {"nums": 5, "signs": 7},
    5: {"nums": 5},
    6: {"nums": 6, ".": 3, "e": 4},
    7: {"nums": 5}
  }
  let state = 0;
  for( c of s.trim() ) {
    if(c >= "0" && c <= "9") {
      c = "nums";
    } else if(c === "+" || c === "-") {
      c = "signs";
    } else if(c === " ") {
      c = "blank";
    }
    state = graph[state][c];
    if(state === undefined) return false;
  }
  if(state === 3 || state === 5 || state === 6) return true;
  return false;
};
```



---

## 太平洋大西洋水流问题 - 417 ***

给定一个 `m x n` 的非负整数矩阵来表示一片大陆上各个单元格的高度。“太平洋”处于大陆的左边界和上边界，而“大西洋”处于大陆的右边界和下边界。

规定水流只能按照上、下、左、右四个方向流动，且只能从高到低或者在同等高度上流动。

请找出那些水流既可以流动到“太平洋”，又能流动到“大西洋”的陆地单元的坐标。

**提示：**

1. 输出坐标的顺序不重要
2. *m* 和 *n* 都小于150

 

**示例：**

```
给定下面的 5x5 矩阵:

  太平洋 ~   ~   ~   ~   ~ 
       ~  1   2   2   3  (5) *
       ~  3   2   3  (4) (4) *
       ~  2   4  (5)  3   1  *
       ~ (6) (7)  1   4   5  *
       ~ (5)  1   1   2   4  *
          *   *   *   *   * 大西洋

返回:

[[0, 4], [1, 3], [1, 4], [2, 2], [3, 0], [3, 1], [4, 0]] (上图中带括号的单元).
```



***解法***

* 新建两个矩阵，分别记录能够流到两大洋的坐标
* 从海岸线逆流而上，深度优先遍历图，同时记录上述矩阵

```js
/**
 * @param {number[][]} matrix
 * @return {number[][]}
 */
var pacificAtlantic = function(matrix) {
  if(!matrix || !matrix[0]) return [];
  const m = matrix.length;
  const n = matrix[0].length;
  // 初始化两个 m x n 的二维数组分别代表能够流向太平洋和大西洋的坐标的矩阵
  const flow1 = Array.from({ length: m }, () => new Array(n).fill(false));
  const flow2 = Array.from({ length: m }, () => new Array(n).fill(false));
  // 深度优先遍历
  const dfs = (r, c, flow) => {
    flow[r][c] = true;
    // 对于当前坐标相邻的四个坐标进行有效性判断
    // 1. 相邻坐标在有效矩阵中
    // 2. 下一个坐标未经过判断 (防止死循环)
    // 3. 下一个坐标符合“逆流而上”，即下一个坐标大于或者等于当前坐标
    [[r+1, c], [r-1, c], [r, c+1], [r, c-1]].forEach(([nr, nc]) => {
      if( nr >= 0 && nr < m &&
          nc >= 0 && nc < n &&
          !flow[nr][nc] &&
          matrix[nr][nc] >= matrix[r][c]
      ) {
          dfs(nr, nc, flow); 
      }
    })
  }
  for( let r = 0; r < m; r++) {
    // 从第0列和最后一列开始判断能否流向太平洋/大西洋
    dfs(r, 0, flow1);
    dfs(r, n-1, flow2);
  }
  for( let c = 0; c < n; c++) {
    // 从第0行和最后一行开始判断能否流向太平洋/大西洋
    dfs(0, c, flow1);
    dfs(m-1, c, flow2);
  }
  const res = [];
  // 两标记矩阵中同时为 true 的坐标即为即可以流向太平洋，也可以流向大西洋的坐标
  for (let r = 0; r < m; r++) {
    for (let c = 0; c < n; c++) {
      if(flow1[r][c] && flow2[r][c]) {
        res.push([r, c]);
      }
    }
  }
  return res;
};
```



---

## 克隆图 - 133

给你无向 **[连通](https://baike.baidu.com/item/连通图/6460995?fr=aladdin)** 图中一个节点的引用，请你返回该图的 [**深拷贝**](https://baike.baidu.com/item/深拷贝/22785317?fr=aladdin)（克隆）。

图中的每个节点都包含它的值 `val`（`int`） 和其邻居的列表（`list[Node]`）。

```
class Node {
    public int val;
    public List<Node> neighbors;
}
```

**测试用例格式：**

简单起见，每个节点的值都和它的索引相同。例如，第一个节点值为 1（`val = 1`），第二个节点值为 2（`val = 2`），以此类推。该图在测试用例中使用邻接列表表示。

**邻接列表** 是用于表示有限图的无序列表的集合。每个列表都描述了图中节点的邻居集。

给定节点将始终是图中的第一个节点（值为 1）。你必须将 **给定节点的拷贝** 作为对克隆图的引用返回。

**示例 1：**

<img src="D:\Learning\Note\Algorithm\Map.assets\image-20200803174644973.png" alt="image-20200803174644973" style="zoom:67%;" />

```
输入：adjList = [[2,4],[1,3],[2,4],[1,3]]
输出：[[2,4],[1,3],[2,4],[1,3]]
解释：
图中有 4 个节点。
节点 1 的值是 1，它有两个邻居：节点 2 和 4 。
节点 2 的值是 2，它有两个邻居：节点 1 和 3 。
节点 3 的值是 3，它有两个邻居：节点 2 和 4 。
节点 4 的值是 4，它有两个邻居：节点 1 和 3 。
```

**示例 2：**

<img src="D:\Learning\Note\Algorithm\Map.assets\image-20200803174750477.png" alt="image-20200803174750477" style="zoom:67%;" />

```
输入：adjList = [[]]
输出：[[]]
解释：输入包含一个空列表。该图仅仅只有一个值为 1 的节点，它没有任何邻居。
```



***思路***

* 拷贝所有节点，拷贝所有边
* 深度或广度优先遍历所有节点
* 拷贝所有的节点，存储起来
* 将拷贝的节点，按照原图的连接方法进行连接

***解法 - 深度优先遍历***

```js
/**
 * // Definition for a Node.
 * function Node(val, neighbors) {
 *    this.val = val === undefined ? 0 : val;
 *    this.neighbors = neighbors === undefined ? [] : neighbors;
 * };
 */

/**
 * @param {Node} node
 * @return {Node}
 */
var cloneGraph = function(node) {
    if(!node) return;
    // 定义一个 map 将原节点与克隆节点对应起来
    const visited = new Map();
    const dfs = (n) => {
      visited.set(n, new Node(n.val));
      // 遍历相邻节点时，防止相邻节点为空
      (n.neighbors || []).forEach((ne) => {
        if(!visited.has(ne)) {
          dfs(ne);
        }
        // 深度遍历之后，相邻节点一定已经存入 map 中，可以开始克隆节点 n 的相邻节点 ne
        visited.get(n).neighbors.push(visited.get(ne));
      })
    }
    dfs(node);
    return visited.get(node);
};
```

***解法 - 广度优先遍历***

```js
/**
 * // Definition for a Node.
 * function Node(val, neighbors) {
 *    this.val = val === undefined ? 0 : val;
 *    this.neighbors = neighbors === undefined ? [] : neighbors;
 * };
 */

/**
 * @param {Node} node
 * @return {Node}
 */
var cloneGraph = function(node) {
    if(!node) return;
    // 定义一个 map 将原节点与克隆节点对应起来
    const visited = new Map();
    // 广度优先遍历
    const q = [node];
    visited.set(node, new Node(node.val));
    while(q.length) {
      const n = q.shift();
      (n.neighbors || []).forEach((ne) => {
        if(!visited.has(ne)) {
          q.push(ne);
          visited.set(ne, new Node(ne.val));
        }
        visited.get(n).neighbors.push(visited.get(ne));
      });
    }
    return visited.get(node);
};
```

