---
title: LeetCode-15-三数之和
author: bellick
copyright: true
top: 0
date: 2020-03-28 17:42:41
categories:
- Algorithms
- LeetCode
tags:
mathjax:
---

# LeetCode-15-三数之和

## 思路



* 先确定一个，然后找剩下两个
* 剩下两个通过双指针寻找

* 三个元素一定有一个是最小的
* 先确定最小的

* 为了避免重复，先排序

* 如果第一个元素就>0, 那就没必要找了，break
* 如果当前元素和前一个元素一样，则跳过

* 因为不能重复，所以 left == right 时就得结束


## 代码

```java

class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        int len = nums.length;
        if(nums==null || len <3)return res;
        
        Arrays.sort(nums);
        
        for(int i = 0;i<len;i++) {
        	if(nums[i] > 0)break;//如果当前元素都大于0，就提前结束
        	if(i > 0 && nums[i] == nums[i-1])continue;
        	int left = i+1;
        	int right = len-1;
        	while(left < right) {
        		int sum = nums[i] + nums[left] + nums[right];
        		if(sum == 0) {
        			//符合条件
        			res.add(Arrays.asList(nums[i],nums[left],nums[right]));
        			while(left < right && nums[left] == nums[left+1])left++;
        			while(left < right && nums[right] == nums[right-1])right--;
        			left ++;
        			right--;
        		}else if(sum > 0) {
        			right --;
        		}else if(sum < 0) {
        			left ++;
        		}
        	}
        }
        return res;
    }
}

```

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<>();
        for(int i = 0;i<nums.length-2;i++){
            if(i>0&&nums[i]==nums[i-1])continue;//避免重复三元组；
            int left = i+1,right = nums.length-1,target = 0-nums[i];
            while (left<right){
                if(target == nums[left]+nums[right]){
                    res.add(Arrays.asList(nums[i],nums[left],nums[right]));
                    while(left<right&&nums[left]==nums[left+1])left++;
                    while(left<right&&nums[right] == nums[right-1])right--;
                    left++;
                    right--;
                }else if(target < nums[left]+nums[right]){
                    right--;
                }else{
                    left++;
                }

            }
        }
        return res;
    }
}

```
