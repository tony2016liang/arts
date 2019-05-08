## ARTS 第五周

> ### 每周完成一个ARTS： 每周至少做一个 leetcode 的算法题、阅读并点评至少一篇英文技术文章、学习至少一个技术技巧、分享一篇
> ### 有观点和思考的技术文章。（也就是 Algorithm、Review、Tip、Share 简称ARTS）  

## Algorithm
### [LeetCode - 14. Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)
判断一组字符串中最大的相同字符数，并打印出相同的字符。条件是从前往后数，且是连续的才算，这就大大减小了难度，所以仅是easy级别。

### 解题思路：

## Review
So You Want to be a Functional Programmer (Part 3 & Part 4) 。
### [Part3](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-3-1b0fd14eb1a7)
1. 函数组合（Function Composition）：代码复用听着很棒，做起来却难。写得太具体，不易于后期的复用；写得太抽象，则连第一次的使用都会很困难。  
在函数式编程中，函数就类似一个个乐高积木，单个的用于执行具体的任务，同时又可以组合他们在一起执行更复杂的任务。
2. 无参编程（Point-Free）：不用指定冗余的参数，代码更简单，更简便（比如不用为了想出不同的变量名而发愁）；另外没有参数掺杂在
逻辑中，代码也更易读，更易分析。
3. 柯里化（Currying）：通过上述的函数组合和无参化可以将多个不同的简单函数组合在一起，并写成极简便的形式，但对于入参多于一个的函数这一方式
却不太适用，由此引出了柯里化的概念。

### [Part4](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-2-7005682cec4a)
1. 柯里函数（Curried Function）：每次只会处理一个参数的函数。
2. 柯里化和重构：通过将函数柯里化，可以很方便地将普通的函数重构成无参的简单形式，而充分利用柯里化的重要一点就是要设置好参数的顺序。
3. 函数式编程常用函数：即Map、Filter、Reduce，其实就是将对列表的一些操作封装起来，使用时只需传入待处理的列表和需执行的操作函数即可，
而不用每次都显式地去遍历列表。

## Tip
spring boot针对多数据源的事务管理(以一个简单的示例进行说明)



## Share
MySql批量插入数据及插入重复数据的处理
### [Mysql中INSERT ... ON DUPLICATE KEY UPDATE的实践](https://www.jianshu.com/p/78ea17c6d190)

