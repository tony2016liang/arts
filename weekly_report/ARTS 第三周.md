## ARTS 第二周

> ### 每周完成一个ARTS： 每周至少做一个 leetcode 的算法题、阅读并点评至少一篇英文技术文章、学习至少一个技术技巧、分享一篇有观点和思考的技术文章。（也就是 Algorithm、Review、Tip、Share 简称ARTS）  

## Algorithm
### [LeetCode - 9. Palindrome Number](https://leetcode.com/problems/palindrome-number/)
判断一个整数是否是回文的。正例如121，反例如-121,10等 

### 解题思路：  
利用了之前一个算法题的思路，转换一个整数的位数。于是，该题就变成了先排除负值，然后转换整数值，再判断转换后的值与原值是否相等
```
class Solution {
    public boolean isPalindrome(int x) {
        
        if (x < 0) return false;
        int result = transferInteger(x);
        if (result == x) return true;
        else return false;
    }
    
    private int transferInteger(int x) {
        int result = 0;
        while(x != 0) {
            int temp = x % 10;
            x = x / 10;
            if(result > Integer.MAX_VALUE / 10 || result == Integer.MAX_VALUE / 10 && temp > 7) return 0;
            result = result * 10 + temp;
        }
        return result;
    }
}
```  


## Review
### [the best programming language](http://coding-geek.com/the-best-programming-language/)
关于什么开发语言最好，估计从有不同的开发语言存在，这种争论就没有停止过。  
本文作者从一种幽默的角度切入，主要从实践上简单论述了自己关于这个问题的观点。其主旨意思就是说：根本不存在什么最好的开发语言，
能把活干好的就是好的开发语言。  
借用邓老的观点：不管黑猫白猫，抓到老鼠的就是好猫。


### [Why Do Arrays Start With Index 0?](https://medium.com/@albertkoz/why-does-array-start-with-index-0-65ffc07cbce8)
另外分享一个有趣的小知识点，关于为什么数组下标从0开始的问题，可以点开原文看下。

## Tip
### 我不知道我有没有这方面的天赋，这条路我还是否能走下去？是否需要及时回头去寻找其他的路？



## Share
### [java int溢出总结](https://njucz.github.io/2017/08/16/java-int%E6%BA%A2%E5%87%BA%E6%80%BB%E7%BB%93/)

