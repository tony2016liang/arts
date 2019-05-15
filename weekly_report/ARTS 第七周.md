## ARTS 第五周

> ### 每周完成一个ARTS： 每周至少做一个 leetcode 的算法题、阅读并点评至少一篇英文技术文章、学习至少一个技术技巧、分享一篇
> ### 有观点和思考的技术文章。（也就是 Algorithm、Review、Tip、Share 简称ARTS）  

## Algorithm
### [LeetCode - 20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)
给定一组带括号字符的字符串，判断括号的配对是否是有效的，规则有两个：
1. 有开就要有闭
2. 顺序一定要对
另外要注意字符串中不能有除了6个括号（'(', ')', '{', '}', '[', ']'）之外的其他字符，否则为无效。
另外空字符串视为有效。

### 解题思路
利用栈的特性，分别通过对开括号入栈和闭括号出栈，然后进行比对的方式进行判断，另外需注意一些空指针异常，在这里是EmptyStackException

## Review

## Tip

## Share

这是在工作中遇到的一个问题，UnsupportedOperationException异常，看了下面的文章才知道原来通过Arrays.asList()生成的ArrayList对象并不是
我们常用的那个java.util.ArrayList，而是Arrays类中的一个内部类，其并没有实现对集合进行修改的方法，又长见识了，详情请查看原文。
### [Java容器——UnsupportedOperationException](https://my.oschina.net/itblog/blog/524792)

