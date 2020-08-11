# 1 时间复杂度

## 1.1 时间复杂度增长趋势

* 增长趋势：O(1) < O(logN) < O(n) < O(n^2)
* logN表示求2的多少次方为n

## 1.2 时间复杂度计算

* 每次运行算法，需要执行的次数
* 不同算法的时间复杂度计算
  * 两个时间复杂度的算法先后排列，时间复杂度相加，结果取增长趋势较大的时间复杂度
  * 两个时间复杂度的算法嵌套，时间复杂度相乘，结果为时间复杂度的乘积

# 2 空间复杂度

* 算法运行时临时占用存储空间的大小的度量

# 3 栈 - 后进先出(Array实现)

## 3.1 使用Array实现

* JavaScript中没有栈，使用Array实现栈的所有功能(push, pop)

## 3.2 使用场景

* 十进制转二进制，余数倒序输出
* 有效括号(判断括号是否有效闭合)
  * 左括号入栈，右括号出栈，检查最后栈是否为空
* 函数调用堆栈(函数中调用另一函数)
  * 最后调用的函数，最先执行完
* 所有递归都可以用栈数据结构模拟或者改写

# 4 队列 - 先进先出(Array实现)

## 4.1 使用Array实现

* 使用 push 和 shift 实现

## 4.2 使用场景

* 需要先进先出的场景：JS异步中的任务队列，计算最近请求次数

## 4.3 常用操作

* push(), shift(), arr[0] (队头元素)

# 5 链表

## 5.1 数组 vs 链表

* 数组：增删非首尾元素需要移动元素
* 链表：增删非首尾元素，只需改变指针

## 5.2 使用Object实现

* 使用.next指向下一个节点
* 链表也可以用来遍历JS中的原型链
* 链表指针获取JSON的节点值

```js
const json = {
    a: {b: {c: 1}}
}
const path = ['a', 'b', 'c'];
const p = json;
path.forEach(key => {
    p = p[key]
});
// p 为获取的
```

# 6 集合 - 无序且唯一

## 6.1 ES6中有集合，Set

## 6.2 使用场景

* 去重

```js
const arr = [1,1,2,3,3];
const arr2 = [...new Set(arr)];
```

* 判断某元素是否在集合中

```js
const s = new Set();
s.has();
```

* 判断交集

```js
const set1 = new Set(arr1);
const set2 = new Set(arr2);
const set3 = new Set([...set1].filter(item => set2.has(item)));  // 交集
```

* 判断差集 (set1有，set2没有)

```js
const set3 = new Set([...set1].filter(item => !set2.has(item)));  // 差集
```



## 6.3 迭代Set

* for ... of

## 6.4 Set 和 Array 互转

**Set 转 Array**

```js
const arr = [...mySet];
const arr = Array.from(mySet)
```

**Array 转 Set**

```js
const mySet = new Set(arr);
```



# 7 字典 - 键值对

## 7.1 ES6中有字典，Map

* set (增、改), get, delete, clear 时间复杂度都是O(1)

## 7.2 使用场景



# 8 树 - 分层

## 8.1 使用Object 和 Array构建树

## 8.2 使用场景

* 前端：DOM、树、级联选择、树形控件
* 深度/广度优先遍历
  * 使用深度遍历：遍历JSON所有节点值
* 先中后序遍历

## 8.3 深度/广度优先遍历

### 8.3.1 深度优先遍历 (递归)

* 尽可能深的搜素树的分支
* 口诀：
  * 访问根节点
  * 对根节点的children逐个进行深度优先遍历

```js
const dfs = function(root) {
    console.log(root.val);
    root.children.forEach(dfs);
}
```



### 8.3.2 广度优先遍历 (使用队列)

* 先访问离根节点最近的节点

* 口诀：
  * 新建一个队列，把根节点入队
  * 把队头出队并访问
  * 把队头的children逐个入队
  * 重复二、三步，直到队列为空

```js
const bfs = function(root) {
    const q = [root];
    while(q.length) {
        const node = q.shift();
        console.log(node.val);
        node.children.forEach(child => q.push(child));
    }
}
```

# 9 二叉树

## 9.1 使用Object 模拟二叉树

* 二叉树中每个节点最多只能有两个子节点

## 9.2 先序遍历

* 访问根节点
* 对根节点的左子树进行先序遍历
* 对根节点的右子树进行先序遍历

```js
// 递归版
const preOrder = function(root) {
    if(!root) return;
    console.log(root.val);
    preOrder(root.left);
    preOrder(root.right);
}

// 非递归版(栈)
const preOrder = function(root) {
    if(!root) return;
    const stack = [root];
    while(stack.length) {
        const n = stack.pop();
        console.log(n.val);
        if(n.right) stack.push(n.right);
        if(n.left) stack.push(n.left);
    }
}
```



## 9.3 中序遍历

* 对根节点的左子树进行中序遍历
* 访问根节点
* 对根节点的右子树进行中序遍历

```js
// 递归版
const inOrder = function(root) {
    if(!root) return;
    inOrder(root.left);
    console.log(root.val);
    inOrder(root.right);
}

// 非递归版
const inOrder = function(root) {
    if(!root) return;
    const stack = [];
    let p = root;
    while(stack.length || p) {
       	while(p) {
            // 先将所有左节点压栈
            stack.push(p);
            p = p.left;
        }
        n = stack.pop();
        console.log(n.val);
        p = n.right;
    }
}
```



## 9.4 后序遍历

* 对根节点的左子树进行后序遍历
* 对根节点的右子树进行后序遍历
* 访问根节点

```js
// 递归版
const postOrder = function(root) {
   	if(!root) return;
    postOrder(root.left);
    postOrder(root.right);
    console.log(root.val);
}

// 非递归版
const postOrder = function(root) {
    if(!root) return;
    const stack = [root];
    const outputStack = [];
    while(stack.length) {
        const n = stack.pop();
        outputStack.push(n);
        if(n.left) stack.push(n.left);
        if(n.right) stack.push(n.right);
    }
    while(outputStack.length) {
        const n = outputStack.pop();
        console.log(n.val);
    }
}
```



# 10 图

* 图是网络结构的抽象模型，是一组由边连接的节点

## 10.1 使用 Object 和 Array 构建图

## 10.2 图的表示法

* 邻接矩阵
* 邻接表

```js
const graph = {
    0: [1,2],
    1: [2],
    2: [0,3]
}
```

* 关联矩阵

## 10.3 图的遍历

### 10.3.1 深度优先遍历

* 尽可能深地搜索图的分支
* 步骤
  * 访问根节点
  * 对根节点的没访问过的相邻节点挨个进行深度优先遍历

```js
const visited = new Set();
const dfs = (n) => {
    console.log(n);
    visited.add(n);
    graph[n].forEach(c => {
        if(!visited.has(c)) {
            dfs(c);
        }
    });
}
```

### 10.3.2 广度优先遍历

* 先访问离根节点最近的节点

* 步骤
  * 新建一个队列，把根节点入队
  * 把队头出队并访问
  * 把队头的没访问过的相邻节点入队
  * 重复第二、三步，直到队列为空

```js
const visited = new Set();
visited.add(起始节点);
const q = [起始节点];
const bfs = (n) => {
    while(q.length) {
        const n = q.shift();
        console.log(n);
        graph[n].forEach(c => {
            if(!visited.has(c)) {
                q.push(c);
                visited.add(c);
            }
        });
    }
}
```



# 11 堆

* 是一种特殊的完全二叉树，最后一层若不是满的则只缺少右边的若干节点
* 所有节点都大于等于(最大堆)或小于等于(最小堆)它的子节点

## 11.1 使用Array表示堆

* 任意节点的左侧子节点的位置是 2 * index + 1
* 任意节点的右侧子节点的位置是 2 * index + 2
* 任意节点的父节点的位置是 ( index - 1 ) / 2

## 11.2 堆的应用

* 堆能高效、快速地找出最大值和最小值，时间复杂度：O(1)
* 找出第 K 个最大(小)元素
  * 构建最小(大)堆，并将元素依次插入堆中
  * 当堆的容量超过 K, 就删除堆顶
  * 插入结束后，堆顶就是第 K 个最大元素

## 11.3 删除堆顶

* 用数组尾部元素替换堆顶(直接删除堆顶会破坏堆结构)
* 然后将新的堆顶和它的子节点交换，直到子节点大于等于(小于等于)这个堆顶
* 大小为 k 的堆中删除堆顶的时间复杂度为O(logk)

## 11.4 获取堆顶和堆的大小 

* 获取堆顶： 返回数组的头部
* 获取堆的大小： 返回数组的长度

```js
// 小顶堆
class MiniHeap {
  constructor() {
    this.heap = [];
  }
  getParentIndex(i) {
    return (i - 1) >> 1;
  }
  getLeftChildIndex(i) {
    return i * 2 + 1;
  }
  getRightChildIndex(i) {
    return i * 2 + 2;
  }
  swap(i1, i2) {
    const temp = this.heap[i1];
    this.heap[i1] = this.heap[i2];
    this.heap[i2] = temp;
  }
  shiftUp(index) {
    if(index === 0) { return; }
    const parentIndex = this.getParentIndex(index);
    if(this.heap[parentIndex] > this.heap[index]) {
      this.swap(parentIndex, index);
      this.shiftUp(parentIndex);
    }
  }
  shiftDown(index) {
    const left = this.getLeftChildIndex(index);
    const right = this.getRightChildIndex(index);
    if(this.heap[index] > this.heap[left]) {
      this.swap(index, left);
      this.shiftDown(left);
    }
    if(this.heap[index] > this.heap[right]) {
      this.swap(index, right);
      this.shiftDown(right);
    }
  }
  insert(value) {
    this.heap.push(value);
    this.shiftUp(this.heap.length - 1);
  }
  pop() {
    this.heap[0] = this.heap.pop();
    this.shiftDown(0);
  }
  peek() {
    return this.heap[0];
  }
  size() {
    return this.heap.length;    
  }
}
```



# 12 排序和搜索

* JS 中的排序：数组的 sort 方法
* JS 中的搜索：数组的 indexOf 方法

## 12.1 排序方法

### 12.1.1 冒泡排序法

* 比较所有相邻元素，如果第一个比第二个大，则交换
* 一轮下来，保证最后一个数是最大的
* 执行 n - 1 轮，完成排序
* 时间复杂度：O(n^2)

```js
// bubbleSort
Array.prototype.bubbleSort = function() {
    for(let i = 0; i < this.length - 1; i++) {
        for(let j = 0; j < this.length - 1 - i; j++) {
            if(this[j] > this[j+1]) {
                const temp = this[j];
                this[j] = this[j+1];
                this[j+1] = temp;
            }
        }
    }
}
```

### 12.1.2 选择排序法

* 找到数组中的最小值，选中并放到第一位
* 以此类推，直至执行 n - 1 轮
* 时间复杂度：O(n^2)

```js
Array.prototype.selectionSort = function() {
    for(let i = 0; i < this.length - 1; i++) {
        let indexMin = i;
        for(let j = i + 1; j < this.length; j++) {
            if(this[indexMin] > this[j]) {
                indexMin = j;
            }
        }
        const temp = this[i];
        this[0] = this[indexMin];
        this[indexMin] = temp;
    }
}
```

### 12.1.3 插入排序法 (往前比较)

* 每轮取出一个数往前比(从第二个数开始)，找到不大于取出数的位置，插入到其后
* 时间复杂度：O(n^2)

```js
Array.prototype.insertionSort = function() {
  for(let i = 1; i < this.length; i++) {
      const temp = this[i];
      let j = i;
      for(; j > 0; j--) {
          if(this[j - 1] > temp) {
              this[j] = this[j - 1];
          } else {
              break;
          }
      }
      this[j] = temp;
  }
};
```

### 12.1.4 归并排序法 (对半拆分合并)

* 分：对用递归对数组进行对半分的操作
* 合：把两个数合并为有序数组，再对有序数组进行合并
* 合并两个有序数组
  * 新建空数组 res，用于存放最终排序后的数组
  * 比较两个有序数组的头部，较小者出队并推入 res 中，直至两个数组都没有值
* 时间复杂度：O(nlogn)

```js
// mergeSort
Array.prototype.mergeSort = function() {
  const rec = function(arr) {
    // 拆分
    if(arr.length === 1) { return arr };
    const mid = Math.floor(arr.length / 2);
    const left = arr.slice(0, mid);
    const right = arr.slice(mid, arr.length);
    const orderLeft = rec(left);
    const orderRight = rec(right);
    // 合并
    const res = [];
    while(orderLeft.length || orderRight.length) {
      if(orderLeft.length && orderRight.length) {
        res.push(orderLeft[0] < orderRight[0] ? orderLeft.shift() : orderRight.shift());
      } else if(orderLeft.length) {
        res.push(orderLeft.shift());
      } else if(orderRight.length) {
        res.push(orderRight.shift());
      }
    }
    return res;
  }
  const res = rec(this);
  res.forEach((n, i) => this[i] = n);
}
```

### 12.1.5 快速排序法 ("基准")

* 分区：从数组中任意选择一个元素为“基准”，比基准小的放基准前面，比基准大的元素放在基准后面
* 递归：递归地对基准前后的子数组进行分区
* 时间复杂度：O(nlogn)

```js
// quickSort
Array.prototype.quickSort = function() {
  const rec = function(arr) {
    if(arr.length <= 1) { return arr; }
    const left = [];
    const right = [];
    const mid = arr[0];
    for(let i = 1; i < arr.length; i++) {
      if(mid > arr[i]) {
        left.push(arr[i]);
      } else {
        right.push(arr[i]);
      }
    }
    return [...rec(left), mid, ...rec(right)];
  }
  const res = rec(this);
  res.forEach((n, i) => this[i] = n);
}
```

## 12.2 搜索方法

### 12.2.1 顺序搜索

* 遍历数组
* 找到目标值，返回其下标
* 没有找到目标值，返回 -1
* 时间复杂度：O(n)

```js
// sequentialSearch
Array.prototype.sequentialSearch = function(value) {
    for(let i = 0; i < this.length; i++) {
        if(this[i] === value) {
            return i;
        }
    }
    return -1;
}
```

### 12.2.2 二分搜索

* 前提：有序数组
* 从数组中间元素开始，若目标大于或小于中间元素，则从大于或小于中间元素的一半数组中搜索
* 时间复杂度：O(logn)

```js
// binarySearch
Array.prototype.binarySearch = function(value) {
    let low = 0;
   	let high = this.length - 1;
    while(low <= high) {
        const mid = Math.floor((low + high) / 2);
        const midValue = this[mid];
        if(midValue < value) {
            low = mid + 1;
        } else if(midValue > value) {
            high = mid - 1;
        } else {
            return mid;
        }
    }
    return -1;
};
```



# 13 分而治之

* 算法设计中的一种思想
* 将一个问题分成多个和原来问题相似的小问题，递归解决这些小问题，进而解决大问题

## 13.1 使用场景

* 归并排序
  * 分：把数组从中间一分为二
  * 解：递归地对两个子数组进行归并排序
  * 合：合并有序子数组
* 快速排序
  * 分：选基准，按基准把数组分成两个子数组
  * 解：递归地对两个子数组进行快速排序
  * 合：对两个子数组进行合并

* 二分搜索
* 翻转二叉树



# 14 动态规划

* 算法设计中的一种思想
* 将问题分解为**相互重叠**的子问题，通过反复求解解决原来的问题

## 14.1 动态规划 vs. 分而治之

* 区别在于子问题是否相互独立，子问题相互重叠为动态规划，不重叠则为分而治之

## 14.2 解题步骤

* 定义子问题

# 15 回溯算法

* 算法设计中的一种思想
* **渐进式**寻找并构建问题解决方式的策略
* 先从一个可能的动作开始解决问题，如果不行，则回溯并选择另一个动作，直到将问题解决