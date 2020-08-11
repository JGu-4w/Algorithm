# 链表

## 环形链表 - 141

给定一个链表，判断链表中是否有环。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

**示例 1：**

```
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```

<img src="D:\Learning\Note\Algorithm\LinkedList.assets\circularlinkedlist.png" style="zoom: 50%;" />

**示例 2：**

```
输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。
```

<img src="D:\Learning\Note\Algorithm\LinkedList.assets\circularlinkedlist_test2.png" alt="circularlinkedlist_test2" style="zoom:67%;" />

**示例 3：**

```
输入：head = [1], pos = -1
输出：false
解释：链表中没有环。
```

<img src="D:\Learning\Note\Algorithm\LinkedList.assets\circularlinkedlist_test3.png" alt="circularlinkedlist_test3" style="zoom:80%;" />


进阶：

你能用 O(1)（即，常量）内存解决此问题吗？

***解法***

解法一：快慢指针 空间复杂度O(1)

* 若链表存在环，则快慢指针必定会相遇；快慢指针还可以用来取得链表中间的节点

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} head
 * @return {boolean}
 */
var hasCycle = function(head) {
    let p1 = head;
    let p2 = head;
    while(p1 && p2 && p2.next) {
      p1 = p1.next;
      p2 = p2.next.next;
      if(p1 == p2) {
        return true;
      }
    }
    return false;
};
```



-----

## 回文链表 - 234

请判断一个链表是否为回文链表。

**示例 1:**

```
输入: 1->2
输出: false
```

**示例2：**

```
输入: 1->2->2->1
输出: true
```

**进阶：**
你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？

***解法***

* 思路：使用快慢指针找出链表的中间位置断开，翻转后半段链表并遍历比较两链表是否完全相等(奇数偶数链表不影响，因为只要其中一条链表遍历完即表明原链表为回文链表)

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {boolean}
 */
var isPalindrome = function(head) {
  if(!head) return true;
  let firstList = head;
  let firstEnd = findHalfEnd(head);
  let secondList = firstEnd.next;
  // 先从中间断开链表，翻转后半段链表
  firstEnd.next = null;
  let secondNewList = reverseList(secondList);
  // 遍历两链表
  let p = secondNewList;
  while(firstList && p) {
    if(firstList.val !== p.val) {
      // 链表不是回文链表，还原链表并返回false
      secondList = reverseList(secondNewList);
      firstEnd.next = secondList;
      return false;
    }
    firstList = firstList.next;
    p = p.next;
  }
  secondList = reverseList(secondNewList);
  firstEnd.next = secondList;
  return true;
};

var findHalfEnd = function(head) {
  let fast = head;
  let slow = head;
  while(fast.next && fast.next.next) {
    slow = slow.next;
    fast = fast.next.next;
  }
  return slow;
}

var reverseList = function(head) {
  let p1 = head;
  let p2 = null;
  while(p1) {
    let tmp = p1.next;
    p1.next = p2;
    p2 = p1;
    p1 = tmp;   
  }
  return p2;
}
```



----

## 移除链表元素 - 203

删除链表中等于给定值 **val** 的所有节点。

**示例:**

```
输入: 1->2->6->3->4->5->6, val = 6
输出: 1->2->3->4->5
```

***解法***

* 使用哨兵节点解决链表第一个节点即为需要删除节点的情况

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} val
 * @return {ListNode}
 */
var removeElements = function(head, val) {
  // 设置哨兵节点，哨兵节点next指向head
  let p = new ListNode('first');
  p.next = head;
  let cur = head;
  let pre = p;
  while(cur) {
    // 遍历链表
    if(cur.val === val) {
      // cur指针的节点为需要删除的节点，将pre指针的next指向cur指针的next
      pre.next = cur.next;
    } else {
      // 不需要删除的节点，pre指针移动
      pre = cur;
    }
    cur = cur.next;
  }
  return p.next;
};
```



---

## 两数相加 - 2

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

**示例：**

```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

***解法***

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var addTwoNumbers = function(l1, l2) {
  const l3 = new ListNode(null);
  let p1 = l1;
  let p2 = l2;
  let p3 = l3;
  let flag = 0;
  while(p1 || p2) {
    const res = (p1 ? p1.val : 0) + (p2 ? p2.val : 0) + flag;
    flag = res >= 10 ? 1 : 0;
    p3.next = new ListNode(res % 10);
    p3 = p3.next;
    p1 && (p1 = p1.next);
    p2 && (p2 = p2.next);
  }
  if(flag) p3.next = new ListNode(1);
  return l3.next;
};
```



---

## 两两交换链表中的节点 - 24

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

**你不能只是单纯的改变节点内部的值**，而是需要实际的进行节点交换。

 

**示例:**

```
给定 1->2->3->4, 你应该返回 2->1->4->3.
```

***解法***

* 设置哨兵节点记录第一个节点的位置
* head指向每一组对调节点的首端节点
* 设置两个指针(firstNode, secondNode)进行链表交换
* 上一次对调的末端节点(prevNode)连接此次对调后的首端节点
* head移向下一组节点(firstNode.next)

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var swapPairs = function(head) {
  let dummy = new ListNode(-1);
  dummy.next = head;
  let prevNode = dummy;
  while(head && head.next) {
    let firstNode = head;
    let secondNode = head.next;
    // 交换链表节点
    firstNode.next = secondNode.next;
    secondNode.next = firstNode;
    prevNode.next = secondNode;
    // 记录prevNode
    prevNode = firstNode;
    // 移动head指针至下一对节点
    head = firstNode.next;
  }
  return dummy.next;
};
```



---

## 删除链表的倒数第N个节点 - 19

给定一个链表，删除链表的倒数第 *n* 个节点，并且返回链表的头结点。

**示例：**

```
给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5.
```

**说明：**

给定的 *n* 保证是有效的。

**进阶：**

你能尝试使用一趟扫描实现吗？

***解法一***

* 使用快慢指针，fast指针与slow指针的间隔为n，当fast指针到达链表末端时，slow指针正好是需要删除的节点的前一个节点
* 在链表前加一个伪指针处理删除表头节点的情况

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} n
 * @return {ListNode}
 */
var removeNthFromEnd = function(head, n) {
  if(n === 0) return head;
  let dummy = new ListNode(-1);
  dummy.next = head;
  let fast = dummy;
  let slow = dummy;
  while(n+1) {
    // fast指针在slow指针前n+1的位置，使得两指针距离固定为n
    fast = fast.next;
    n--;
  }
  while(fast) {
    //当fast指向链表末端null时，slow正好是需要删除的节点前一个节点
    fast = fast.next;
    slow = slow.next;
  }
  slow.next = slow.next.next;
  return dummy.next;
};
```

***解法二***

* 翻转链表，n即为正序的需要删除的节点的位置，删除后再还原链表

