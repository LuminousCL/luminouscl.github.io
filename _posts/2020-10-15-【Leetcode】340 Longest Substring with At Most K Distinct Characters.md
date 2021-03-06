---
title:  【Leetcode】340 Longest Substring with At Most K Distinct Characters
tags:
  - 算法
---



### 340. Longest Substring with At Most K Distinct Characters

Given a string, find the length of the longest substring T that contains at most *k* distinct characters.

**Example 1:**

```
Input: s = "eceba", k = 2
Output: 3
Explanation: T is "ece" which its length is 3.
```

**Example 2:**

```
Input: s = "aa", k = 1
Output: 2
Explanation: T is "aa" which its length is 2.
```



**Solution 1: “固定左边”的写法** 

```java
class Solution {
    public int lengthOfLongestSubstringKDistinct(String s, int k) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        
        Map<Character, Integer> map = new HashMap<>();
        int result = 0;
        int r = 0;
        for (int l = 0; l < s.length(); l++) {
            while (r < s.length() && ((map.size() < k) || map.size() == k && map.containsKey(s.charAt(r)))) {
                if (!map.containsKey(s.charAt(r))) {
                    map.put(s.charAt(r), 1);
                }
                else {
                    map.put(s.charAt(r), map.get(s.charAt(r)) + 1);
                }
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
    public int lengthOfLongestSubstringKDistinct(String s, int k) {
        if (s == null || s.length() == 0) {
            return 0;
        }
				
        Map<Character, Integer> map = new HashMap<>();
        int result = 0;
        int l = 0;
        for (int r = 0; r < s.length(); r++) {
            char c = s.charAt(r);
            if (!map.containsKey(c)) {
                map.put(c, 1);
            }
            else {
                map.put(c, map.get(c) + 1);
            }
            
            while (map.size() > k) {
                char d = s.charAt(l);
                map.put(d, map.get(d) - 1);
                if (map.get(d) == 0) {
                    map.remove(d);
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
