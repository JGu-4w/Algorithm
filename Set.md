# 集合

## 两个数组的交集 - 349


给定两个数组，编写一个函数来计算它们的交集。

 

**示例 1：**

```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]
```

**示例 2：**

```
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[9,4]
```

 

**说明：**

- 输出结果中的每个元素一定是唯一的。
- 我们可以不考虑输出结果的顺序。

***解法一 - 集合***

* 使用Set去重，再用filter遍历num2数组获取交集

```js
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var intersection = function(nums1, nums2) {
  const set1 = new Set(nums1);
  const set3 = new Set(nums2.filter(item => set1.has(item)));
  return [...set3];
};

var intersection = function(nums1, nums2) {
  return [...new Set(nums1)].filter(item => nums2.includes(item));
};
```

***解法二 - 字典***



-----

## 无重复字符的最长子串 - 3

给定一个字符串，请你找出其中不含有重复字符的 **最长子串** 的长度。

**示例 1:**

```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

**示例 2:**

```
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例 3:**

```
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```



***解法一 - Set + 滑动窗口***

```js
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
    let list = new Set();
    let l = 0; // left pointer
    let r = 0;  // rigth pointer
    let max = -1;
    while (r < s.length) {
        if(!list.has(s[r])) {
            list.add(s[r++]);
            max = max < r - l ? r - l : max;
        } else {
            list.delete(s[l]);
            l++;
        }
    }
    return max == -1 ? 0 : max;
};
```

***解法二 - 字典 + 滑动窗口***

* map存储字符及index
* 遇到重复字符，左指针指向map内重复字符的下一个字符

```js
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
  let l = 0;
  const map = new Map();
  let max = 0;
  for(let r = 0; r < s.length; r++) {
    if(map.has(s[r]) && map.get(s[r]) >= l) {
      l = map.get(s[r]) + 1;
    }
    max = Math.max(max, r - l + 1);
    map.set(s[r], r);
  }
  return max;
};
```

