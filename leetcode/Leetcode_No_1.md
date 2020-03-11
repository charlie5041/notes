---
title: Leetcode No. 1
tags: 
notebook: Leetcode
---

<!-- TOC -->

- [Problem](#problem)
- [Solving](#solving)
- [Code (C++ or python3)](#code-c-or-python3)

<!-- /TOC -->

# Problem
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

**示例:**

给定 nums = [2, 7, 11, 15], target = 9，
因为 nums[0] + nums[1] = 2 + 7 = 9，
所以返回 [0, 1]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/two-sum
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

# Solving



# Code (C++ or python3)

- 一次hash table
```cpp
class Solution {
 public:
  vector<int> twoSum(vector<int>& nums, int target) {
    unordered_map<int,int> m;
    for(int i=0; i<nums.size(); i++) {
      if(m.find(target-nums[i])!=m.end())
        return {m[target-nums[i]],i};
      m[nums[i]]=i;
    }
  }
};
```

- 暴力法
```cpp
class Solution {
 public:
  vector<int> twoSum(vector<int>& nums, int target) {
    vector<int> twotarget;
    for(int i = 0; i < nums.size(); ++i){
      for(int j = 0; j < nums.size(); ++j){
        if(i==j){break;}
        else{
          if(nums[i] + nums[j] == target){
            twotarget.push_back(i);
            twotarget.push_back(j);
          } else{continue;}
            std::cout << "i: " << i << ", j:" << j << std::endl;
        }
      }
      if(twotarget.size() == 2){
        std::cout << "nums[i]: " << twotarget[0] << ", nums[j]:" << twotarget[1]<< std::endl;
        break;
      }
    }
    return twotarget;
  }
};
```