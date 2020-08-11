# 排列和搜索

## 合并两个有序链表 - 21

将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

 

**示例：**

```
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```



***思路***

* 新建一个新链表，作为返回结果
* 用指针遍历两个有序链表，比较两个链表的当前节点，较小者接入链表，指针后移
* 链表遍历结束，返回新链表

***解法***

* 时间复杂度O(n)
* 空间复杂度O(1)

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var mergeTwoLists = function(l1, l2) {
    const l = new ListNode(0);
    let p = l;
    let p1 = l1;
    let p2 = l2;
   	while(p1 && p2) {
        if(p1.val < p2.val) {
            p.next = p1;
            p1 = p1.next;
        } else {
            p.next = p2;
            p2 = p2.next;
        }
        p = p.next;
    }
    if(p1) {
        p.next = p1;
    }
    if(p2) {
        p.next = p2;
    }
    return l.next;
}
```



---

## 猜数字大小 - 374

猜数字游戏的规则如下：

每轮游戏，系统都会从 **1** 到 **n** 随机选择一个数字。 请你猜选出的是哪个数字。
如果你猜错了，系统会告诉你这个数字比系统选出的数字是大了还是小了。
你可以通过调用一个预先定义好的接口 `guess(int num)` 来获取猜测结果，返回值一共有 3 种可能的情况（`-1`，`1` 或 `0`）：

```
-1 : 系统选出的数字比你猜测的数字小
 1 : 系统选出的数字比你猜测的数字大
 0 : 恭喜！你猜对了！
```

**示例 :**

```
输入: n = 10, pick = 6
输出: 6
```



***解法 - 二分搜索***

* 时间复杂度：O(logn)
* 空间复杂度：O(1)

```js
/** 
 * Forward declaration of guess API.
 * @param {number} num   your guess
 * @return 	            -1 if num is lower than the guess number
 *			             1 if num is higher than the guess number
 *                       otherwise return 0
 * var guess = function(num) {}
 */

/**
 * @param {number} n
 * @return {number}
 */
var guessNumber = function(n) {
    let low = 1;
    let high = n;
    while(low <= high) {
      let mid = Math.floor((low + high) / 2);
      const res = guess(mid);
      if(res === -1) {
        high = mid - 1;
      } else if(res === 1) {
        low = mid + 1;
      } else if(res === 0) {
        return mid;
      }
    }
};
```



***解法 - 分而治之***

* 时间复杂度：O(logn)
* 空间复杂度：O(logn)

```js
/** 
 * Forward declaration of guess API.
 * @param {number} num   your guess
 * @return 	            -1 if num is lower than the guess number
 *			             1 if num is higher than the guess number
 *                       otherwise return 0
 * var guess = function(num) {}
 */

/**
 * @param {number} n
 * @return {number}
 */
var guessNumber = function(n) {
    const rec = (low, high) => {
        const mid = Math.floor((low + high) / 2);
        const res = guess(mid);
        if(res === 0) {
            return mid;
        } else if(res === -1) {
            return rec(low, mid - 1);
        } else if(res === 1) {
            return rec(mid + 1, high);
        }
    };
    return rec(1, n);
};
```

