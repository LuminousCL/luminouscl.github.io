---
title:  【Leetcode】159 Longest Substring with At Most Two Distinct Characters
tags:
  - 算法
---



### 159. Longest Substring with At Most Two Distinct Characters

Given a string **s** , find the length of the longest substring **t** that contains **at most** 2 distinct characters.

**Example 1:**

```
Input: "eceba"
Output: 3
Explanation: t is "ece" which its length is 3.
```

**Example 2:**

```
Input: "ccaabbb"
Output: 5
Explanation: t is "aabbb" which its length is 5.
```



**Solution 1: “固定左边”的写法** 

```java
class Solution {
    public int lengthOfLongestSubstringTwoDistinct(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        Map<Character, Integer> map = new HashMap<>();
        int r = 0;
        int result = 0;
        for (int l = 0; l < s.length(); l++) {
            while (r < s.length() && (map.containsKey(s.charAt(r)) || 
            (!map.containsKey(s.charAt(r)) && map.size() < 2))) {
                map.put(s.charAt(r), map.getOrDefault(s.charAt(r), 0) + 1);
                r++;
            }
            
            result = Math.max(result, r - l);
            
            if (map.containsKey(s.charAt(l))) {
                map.put(s.charAt(l), map.get(s.charAt(l)) - 1);
                if (map.get(s.charAt(l)) == 0) {
                    map.remove(s.charAt(l));
                }
            }
        }
        return result;
    }
}
```

| Time Complexity |  Space Complexity   |      Semantic       |
| :-------------: | :-----------------: | :-----------------: |
|      O(n)       |        O(k)         |       [l, r)        |
|  一遍for loop   | 只要用一个有限的has | 左闭右开区间, r - l |



**Solution 2: “固定右边”的写法**

```java
class Solution {
    public int lengthOfLongestSubstringTwoDistinct(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        Map<Character, Integer> map = new HashMap<>();
        int l = 0;
        int result = 0;
        for (int r = 0; r < s.length(); r++) {
            map.put(s.charAt(r), map.getOrDefault(s.charAt(r), 0) + 1);
            
            while (map.size() > 2) {
                if (map.containsKey(s.charAt(l))) {
                    map.put(s.charAt(l), map.get(s.charAt(l)) - 1);
                    if (map.get(s.charAt(l)) == 0) {
                        map.remove(s.charAt(l));
                    }
                }
                l++;
            }
            
            result = Math.max(result, r - l + 1);
        }
        return result;
    }
}
```

| Time Complexity |         Space Complexity         |        Semantic         |
| :-------------: | :------------------------------: | :---------------------: |
|      O(n)       |               O(k)               |         [l, r]          |
|  一遍for loop   | 额外的空间为HashMap，size最大为k | 左闭右闭区间, r - l + 1 |
