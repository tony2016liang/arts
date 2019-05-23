## ARTS 第五周

> ### 每周完成一个ARTS： 每周至少做一个 leetcode 的算法题、阅读并点评至少一篇英文技术文章、学习至少一个技术技巧、分享一篇
> ### 有观点和思考的技术文章。（也就是 Algorithm、Review、Tip、Share 简称ARTS）  

## Algorithm
### [LeetCode - 21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)


### 解决方法一
```

```
这个方法的时间复杂度是1ms

### 解决方法二
```

```
这个方法就是利用的递归的思路，简单的几行代码就把问题解决了，而且时间复杂度是0ms，比第一个方法性能还要好。递归的思想还是值得好好研究研究的。

## Review

### [Java: Equality vs Identity](https://medium.com/@NomadicAlex/java-equality-vs-identity-3b045c9f6c68)

本周分享一篇较简单的文章，讲述java中==和equals之前的区别。这个相信只要学过java的都会知道，只是平时实际工作中不一定能想到要用。  
例如上面算法题的第一种方法，其实就是巧妙利用了引用特性，和==、equals之间的区别的原理都是相通的。

## Tip
### HttpRequest传LocalDate参数技巧
其实就是加一个注解@DateTimeFormat(iso = ISO.DATE)在形参的前面，这样在传参的过程中内部会根据注解进行一定的转化，否则就会报错。  
实际这个技巧也是从一篇博文中看来的，原文如下，可自行查阅：
[Parsing of LocalDate query parameters in Spring Boot](https://blog.codecentric.de/en/2017/08/parsing-of-localdate-query-parameters-in-spring-boot/)

## Share
最近工作中有需要用es，官方文档太博大精深，看懂看完会比较费劲，于是偷懒搜罗了下是否有简单些的中文总结性的博文，于是找到了下面这篇，准确的说应该是一个系列，  
分享给大家，简单入门的话应该可以了
### [ES系列十五、ES常用Java Client API](https://www.cnblogs.com/wangzhuxing/p/9609127.html)

