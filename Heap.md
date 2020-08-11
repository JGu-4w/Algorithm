# 堆

## 数组中的第 K 个最大元素 - 215

在未排序的数组中找到第 **k** 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

**示例 1:**

```
输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
```

**示例 2:**

```
输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
```

**说明:**

你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度。



***解法 - 构建小顶堆***

```js
// 构建小顶堆
class MiniHeap {
  constructor() {
    this.heap = [];
  }
  getParentIndex(i) {
    // 二进制计算方式求商
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
    // 若子节点的值小于父节点的值，则交换，一直递归到根节点
    if(index === 0) { return; }
    const parentIndex = this.getParentIndex(index);
    if(this.heap[parentIndex] > this.heap[index]) {
      this.swap(parentIndex, index);
      this.shiftUp(parentIndex);
    }
  }
  shiftDown(index) {
    // 若父节点的值大于左或右子节点的值，则交换，直至叶子节点末尾
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
    // 删除堆顶元素，具体为将堆末尾的元素替换堆顶元素，再递归与子节点比较大小且交换直至到达合适位置
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

/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
var findKthLargest = function(nums, k) {
  const h = new MiniHeap();
  nums.forEach(n => {
    h.insert(n);
    if(h.size() > k) {
      h.pop();
    }
  })
  return h.peek();
};
```



---

## 前 K 个高频元素 - 347

给定一个非空的整数数组，返回其中出现频率前 ***k\*** 高的元素。

 

**示例 1:**

```
输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]
```

**示例 2:**

```
输入: nums = [1], k = 1
输出: [1]
```

**提示：**

* 你可以假设给定的 k 总是合理的，且 1 ≤ k ≤ 数组中不相同的元素的个数。
* 你的算法的时间复杂度**必须**优于 O(n log n) , n 是数组的大小。
* 题目数据保证答案唯一，换句话说，数组中前 k 个高频元素的集合是唯一的。
* 你可以按任意顺序返回答案。



***解法 - 小顶堆***

* 每当 h.size() 大于 K 时，将最后一个元素 (当前最大值) 替换堆顶元素 (当前最小值)，实现保留前 K 个最大值

```js
class MinHeap {
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
    const parentIndex = this.getParentIndex(index);
    if(this.heap[index].value < (this.heap[parentIndex] && this.heap[parentIndex].value)) {
      this.swap(index, parentIndex);
      this.shiftUp(parentIndex);
    }
  }
  shiftDown(index) {
    const leftIndex = this.getLeftChildIndex(index);
    const rightIndex = this.getRightChildIndex(index);
    if(this.heap[index].value > (this.heap[leftIndex] && this.heap[leftIndex].value)) {
      this.swap(index, leftIndex);
      this.shiftDown(leftIndex);
    }
    if(this.heap[index].value > (this.heap[rightIndex] && this.heap[rightIndex].value)) {
      this.swap(index, rightIndex);
      this.shiftDown(rightIndex);
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

/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */
var topKFrequent = function(nums, k) {
  const map = new Map();
  const h = new MiniHeap();
  nums.forEach(n => {
    map.set(n, map.has(n) ? map.get(n) + 1 : 1);
  });
  map.forEach((value, key) => {
    h.insert({value, key});
    if(h.size() > k) {
      h.pop();
    }
  });
  return h.heap.map(n => n.key);
};
```



----

## 合并 K 个排序链表 - 23 ***

合并 *k* 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。

**示例:**

```
输入:
[
  1->4->5,
  1->3->4,
  2->6
]
输出: 1->1->2->3->4->4->5->6
```



***解法***

```js
class MinHeap {
  constructor() {
    this.heap = [];
  }
  getParentIndex(i) {
    return (i -1) >> 1;
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
    if(index == 0) { return; }
    const parentIndex = this.getParentIndex(index);
    if(this.heap[index].val < (this.heap[parentIndex] && this.heap[parentIndex].val)) {
      this.swap(index, parentIndex);
      this.shiftUp(parentIndex);
    }
  }
  shiftDown(index) {
    const leftIndex = this.getLeftChildIndex(index);
    const rightIndex = this.getRightChildIndex(index);
    if(this.heap[index].val > (this.heap[leftIndex] && this.heap[leftIndex].val)) {
      this.swap(leftIndex, index);
      this.shiftDown(leftIndex);
    }
    if(this.heap[index].val > (this.heap[rightIndex] && this.heap[rightIndex].val)) {
      this.swap(rightIndex, index);
      this.shiftDown(rightIndex);
    }
  }
  insert(value) {
    this.heap.push(value);
    this.shiftUp(this.heap.length - 1);
  }
  pop() {
    if(this.size() === 1) return this.heap.shift();
    const top = this.heap[0];
    this.heap[0] = this.heap.pop();
    this.shiftDown(0);
    return top;
  }
  peek() {
    return this.heap[0];
  }
  size() {
    return this.heap.length;
  }
}

/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode[]} lists
 * @return {ListNode}
 */
var mergeKLists = function(lists) {
  const res = new ListNode(0);
  let p = res;
  const h = new MinHeap();
  lists.forEach(l => {
    if(l) h.insert(l);
  })
  while(h.size()) {
    const n = h.pop();
    p.next = n;
    p = p.next;
    if(n.next) h.insert(n.next);
  }
  return res.next;
};
```

