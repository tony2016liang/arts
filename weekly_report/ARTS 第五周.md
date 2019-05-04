## ARTS 第五周

> ### 每周完成一个ARTS： 每周至少做一个 leetcode 的算法题、阅读并点评至少一篇英文技术文章、学习至少一个技术技巧、分享一篇
> ### 有观点和思考的技术文章。（也就是 Algorithm、Review、Tip、Share 简称ARTS）  

## Algorithm
### [LeetCode - 14. Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)
判断一组字符串中最大的相同字符数，并打印出相同的字符。条件是从前往后数，且是连续的才算，这就大大减小了难度，所以仅是easy级别。

### 解题思路：
主要的思路就是将每个字符串的每个字符都往一个set集合里面放，利用set的去重性，去掉相同的字符，最后判断每个set集合的大小，只有当set
集合中有且仅有一个元素时，才可以判定每个字符串的该位置的字符是相同的。不过因为第一次没考虑到空字符串的情况，还是提交了两次才通过。  

#### 闲话不多说，上代码，第一次的：
```
class Solution {
    public String longestCommonPrefix(String[] strs) {
        StringBuffer result = new StringBuffer("");
        int shortest = 0;
        //取得字符串数组中最短的字符串的长度
        for (String str : strs) {
            if (shortest == 0 || shortest > str.length()) {
                shortest = str.length();
            }
        }
        //利用set的去重性保存字符串的各个对应位置的字符，然后判断set中保存的字符数量
        List<HashSet<Character>> sets = new ArrayList<HashSet<Character>>(shortest);
        for (int i = 0; i < shortest; i++) {
            HashSet<Character> chars = new HashSet<Character>();
            for (int j = 0; j < strs.length; j++) {
                chars.add(strs[j].charAt(i));
            }
            sets.add(chars);
        }
        //遍历各个set集，如果大小有多于1个，则说明有不同的字符
        for (HashSet set : sets) {
            if (set.isEmpty() || set.size() > 1) {
                break;
            } else {
                Iterator it = set.iterator();
                if (it.hasNext()) {
                    result.append(it.next().toString());
                }
            }
        }
        return result.toString();
    }
}
```
结果遇到["","b"]时报了异常

#### 第二次，加了对空字符串的判断：
```
class Solution {
    public String longestCommonPrefix(String[] strs) {
        StringBuffer result = new StringBuffer("");
        int shortest = 0;
        //取得字符串数组中最短的字符串的长度
        for (String str : strs) {
            if (str.length() == 0) {
                return result.toString();
            }
            if (shortest == 0 || shortest > str.length()) {
                shortest = str.length();
            }
        }
        //利用set的去重性保存字符串的各个对应位置的字符，然后判断set中保存的字符数量
        List<HashSet<Character>> sets = new ArrayList<HashSet<Character>>(shortest);
        for (int i = 0; i < shortest; i++) {
            HashSet<Character> chars = new HashSet<Character>();
            for (int j = 0; j < strs.length; j++) {
                chars.add(strs[j].charAt(i));
            }
            sets.add(chars);
        }
        //遍历各个set集，如果大小有多于1个，则说明有不同的字符
        for (HashSet set : sets) {
            if (set.isEmpty() || set.size() > 1) {
                break;
            } else {
                Iterator it = set.iterator();
                if (it.hasNext()) {
                    result.append(it.next().toString());
                }
            }
        }
        return result.toString();
    }
}
```
虽然也是费了些脑子才想起来的解决办法，但是和solution里面的解决方案比起来，还是太low了。关键solution里面的方法都没怎么看懂，还在消化中。。。


## Review
本周一不小心看了一个系列文章，讲函数式编程的，总共是6篇。主要是函数式编程的入门介绍和Elm编程语言的推荐。这次先简单介绍两章，下周再继续分享其他的。
### [So You Want to be a Functional Programmer (Part 1)](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-1-1f15e387e536)


### [So You Want to be a Functional Programmer (Part 2)](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-2-7005682cec4a)



## Tip


## Share


