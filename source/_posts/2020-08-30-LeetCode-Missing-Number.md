---
title: LeetCode Missing Number
date: 2020-08-30 10:46:16
tags: LeetCode
category: Program
---
# Missing Number

> https://leetcode.com/problems/missing-number/

> Given an array containing n distinct numbers taken from 0, 1, 2, ..., n, find the one that is missing from the array.
> 
> Example 1:
> 
> Input: [3,0,1]
> Output: 2
> 
> Example 2:
> 
> Input: [9,6,4,2,3,5,7,0,1]
> Output: 8
> 
> Note:
> Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?

````c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int ret;
        int n = nums.size();
        int checkout[n + 1];
        for (int i = 0; i < n + 1; ++i) {
            checkout[i] = 0;
        }
        for (int i = 0; i < n; ++i) {
            checkout[nums[i]] = 1;
        }
        for (int i = 0; i < n + 1; ++i) {
            if (!checkout[i]) {
                ret = i;
                break;
            }
        }
        return ret;
    }
};
````

```java
class Solution {
    public int missingNumber(int[] nums) {
        int ret = 0;
        int n = nums.length;
        int[] checkout = new int[n + 1];
        for (int i = 0; i < n + 1; ++i) {
            checkout[i] = 0;
        }
        for (int i = 0; i < n; ++i) {
            checkout[nums[i]] = 1;
        }
        for (int i = 0; i < n + 1; ++i) {
            if (checkout[i] == 0) {
                ret = i;
                break;
            }
        }
        return ret;
    }
}
```
