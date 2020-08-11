# 回溯算法

## 全排列 - 46

给定一个 **没有重复** 数字的序列，返回其所有可能的全排列。

**示例:**

```
输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```



***解法***

* concat 不会改变原数组，会返回一个新的组合后的数组
* 时间复杂度：O(n!)
* 空间复杂度：O(n)

```js
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var permute = function(nums) {
  const res = [];
  const backTrack = function(path) {
    if(path.length === nums.length) {
      res.push(path);
      return;
    }
    nums.forEach(n => {
      if(path.includes(n)) return;
      backTrack(path.concat(n));
    })
  }
  backTrack([]);
  return res;
};
```



----

## 子集 - 78

给定一组**不含重复元素**的整数数组 *nums*，返回该数组所有可能的子集（幂集）。

**说明：**解集不能包含重复的子集。

**示例:**

```
输入: nums = [1,2,3]
输出:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```



***解法***

```js
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var subsets = function(nums) {
  const res = [];
  const backtrack = (path, l, start) => {
    if(start === l) {
      res.push(path);
      return;
    }
    for(let i = start; i <= l; i++) {
      backtrack(path.concat(nums[i]), l, i + 1);
    }
  }
  for(let i = 0; i <= nums.length; i++) {
    backtrack([], i, 0);
  }
  return res;
};
```

