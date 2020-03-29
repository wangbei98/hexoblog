---
title: LeetCode-18-四数之和
author: bellick
copyright: true
top: 0
date: 2020-03-28 17:50:50
categories:
- Algorithms
- LeetCode
tags:
mathjax:
---

# LeetCode-18-四数之和

## 思路

类比三数之和

## 代码

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> res = new ArrayList<>();
        if(nums == null || nums.length < 4){
            return res;
        }
        Arrays.sort(nums);
        int len = nums.length;
        // i , j, k, l
        for (int i = 0;i < len-3;i++){
            if(i > 0 && nums[i] == nums[i-1]){
                continue;
            }

            for(int j = i + 1; j < len - 2; j++){
                if(j > i+1 && nums[j] == nums[j-1]){
                    continue;
                }
                int k = j + 1;
                int l = len - 1;
                while(k < l){
                    int cur = nums[i] + nums[j] + nums[k] + nums[l];
                    if(cur == target){
                        res.add(Arrays.asList(nums[i],nums[j],nums[k],nums[l]));
                        k++;
                        while (k < l && nums[k] == nums[k-1]){
                            k++;
                        }
                        l--;
                        while (k < l && nums[l] == nums[l+1]){
                            l--;
                        }
                    }else if(cur > target){
                        l--;
                    }else{
                        k++;
                    }
                }
            }
        }
        return res;
    }
}
```
