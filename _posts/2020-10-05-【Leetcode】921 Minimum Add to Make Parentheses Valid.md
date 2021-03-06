---
title:  【Leetcode】921 Minimum Add to Make Parentheses Valid
tags:
  - 算法
---



### Leetcode 921. Minimum Add to Make Parentheses Valid

Given a string consisting of left and right parenthesis, such as ()() ((( )). Return the minimum number of parentheses needed to add into the parenthesis, so that they become valid.



**Clarification:** The input is a string and the output should be integer.



**Assumption:** Valid input string only has left and right parenthesis, fit in the memory; the input may be null, and we should check the corner case.



**Result:** The high level idea is, how would we define the term **“valid”** in the context of parenthesis?  For each “(”, will have a “)” as a pair to it. So we can use a stack to store "(", and check two cases:

+ **case 1**: if we meet "(", we push it into stack, and start++;

+ **case 2**: if we meet ")", we should check the stack:

+ case 2.1 if stack is empty, we count the total++, and start++;
+ case 2.2 if stack is not empty, we pop the stack, and start++



**Test:** input1 =  ((),   return 1,    and the output1 =  (());  input2 =   )) (  return 3,    and the output2 = (()) ().

```java
class Solution {
    public int minAddToMakeValid(String S) {
        if (S == null || S.length() == 0) {
            return 0;
        }
        
        int total = 0; // 
        int stack = 0;
        int start = 0;
        while (start < S.length()) {
            if (S.charAt(start) == '(') {
                stack++;
            }
            else {
                if (stack > 0) {
                    stack--;
                }
                else {
                    total++;
                }
            }
            start++;
        }
        return total + stack;
    }
}
```

|         Time Complexity          |                    Space Complexity                    |
| :------------------------------: | :----------------------------------------------------: |
|               O(n)               |                          O(1)                          |
| I, j 两个for循环go over一遍array | 用left代替一个具体的stack实现，不需要额外matintain空间 |

