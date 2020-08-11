# 字典

## 两个数组的交集 - 394

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

***解法二 - 字典***

```js
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var intersection = function(nums1, nums2) {
  const map = new Map();
  const res = [];
  nums1.forEach(n => {
    map.set(n, true);
  });

  nums2.forEach(n => {
    if(map.get(n)) {
      res.push(n);
      map.delete(n);
    }
  })

  return res;
};
```



---

## 有效的括号 - 20

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

**示例 1:**

```
输入: "()"
输出: true
```

**示例 2:**

```
输入: "()[]{}"
输出: true
```

**示例 3:**

```
输入: "(]"
输出: false
```

***解法***

* 栈 + 字典

```js
/**
 * @param {string} s
 * @return {boolean}
 */
var isValid = function(s) {
  if(s.length % 2 === 1) return false;
  let stack = [];
  const map = new Map();
  map.set('(', ')');
  map.set('[', ']');
  map.set('{', '}');
  for(let i = 0; i < s.length; i++) {
    const c = s[i];
    if(map.has(c)) {
      stack.push(c);
    } else {
        const t = stack[stack.length - 1];
        if (map.get(t) === c) {
            stack.pop();
          } else {
            return false;
        }
    }
  }
  return stack.length === 0;
};
```



----

## 两数之和 - 1

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

 

**示例:**

```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```



***解法一 - 字典***

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    const map = new Map();
    for(let i = 0; i < nums.length; i++) {
      const match = target - nums[i];  // 当前元素匹配的另一元素值
      if(map.has(match)) {
        // 查找map中是否存在匹配的元素，存在则返回两个元素的index
        return [map.get(match), i];
      } else {
        // map存储当前元素及其index值
        map.set(nums[i], i);
      }
    }
};
```



---

## 【难】最小覆盖子串 - 76

给你一个字符串 S、一个字符串 T，请在字符串 S 里面找出：包含 T 所有字符的最小子串。

**示例：**

```
输入: S = "ADOBECODEBANC", T = "ABC"
输出: "BANC"
```

**说明：**

- 如果 S 中不存这样的子串，则返回空字符串 `""`。
- 如果 S 中存在这样的子串，我们保证它是唯一的答案

***解法***

```js
/**
 * @param {string} s
 * @param {string} t
 * @return {string}
 */
var minWindow = function(s, t) {
  let r = 0;
  let l = 0;
  let res = "";
  const tMap = new Map();
  for(let i = 0; i < t.length; i++) {
    // tMap包含T内的所有字符串及其个数
    tMap.set(t[i], tMap.get(t[i]) ? tMap.get(t[i]) + 1 : 1);
  }
  let tType = tMap.size;
  while(r < s.length) {
    // 右指针寻找直到找到包含T内所有字符的字符串
    if(tMap.has(s[r])) {
      tMap.set(s[r], tMap.get(s[r]) - 1);
      if(tMap.get(s[r]) === 0) tType--;
    }
    while(tType === 0) {
      // 存储当前字符串,substring(start, end)包含start不包含end
      const str = s.substring(l, r + 1);
      if(!res || str.length < res.length) res = str;
      // 当右指针移动至找到包含T内所有字符的字符串时，移动左指针缩小子串长度
      if(tMap.has(s[l])) {
        tMap.set(s[l], tMap.get(s[l]) + 1);
        if(tMap.get(s[l]) === 1) tType++;
      }
      l++;
    }
    r++;
  }
  return res;
};
```

