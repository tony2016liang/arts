## ARTS 第五周

> ### 每周完成一个ARTS： 每周至少做一个 leetcode 的算法题、阅读并点评至少一篇英文技术文章、学习至少一个技术技巧、分享一篇
> ### 有观点和思考的技术文章。（也就是 Algorithm、Review、Tip、Share 简称ARTS）  

## Algorithm
### [LeetCode - 21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
将两个有序的列表组装成一个有序的列表，例子：
```
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```

本周的这个算法题做的比较失败，没有得出自己的解决办法（自己想了一个笨办法：先把两个链表的元素都加到一个数组中，再对数组排序，然后将排好序的值再依次加入一个新的
链表，但是这个办法直接报时间超限了），下面贴一下别人的解决方法吧，第一个方法让我对java的引用类型的理解又加深了一些，第二个方法则让我再次见识到了递归的威力，而
递归的精髓也是我一直想得而不可得的。

### 解决方法一
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode merged = null, mergedTail = null, node;
        while(l1 != null && l2 != null) {
            if (l1.val <= l2.val) {
                node = new ListNode(l1.val);
                l1 = l1.next;
            } else {
                node = new ListNode(l2.val);
                l2 = l2.next;
            }
            
            /**
             * 下面这几行是一开始让我比较迷糊的地方，其实理清了思路也就不难理解了，他是用merged和mergedTail两个变量指向同一个对象，
             * 通过变化mergedTail的值，从而间接地在将值加到merged指向的节点上。有点绕，但如果熟悉java的引用机制的话应不难理解。
             */
            if (merged == null) {
                merged = node;
                mergedTail = node;
            } else {
                mergedTail.next = node;
                mergedTail = mergedTail.next;
            }
        }
        
        if (l1 == null) {
            if (merged == null) {
                return l2;
            }
            mergedTail.next = l2;
            return merged;
        }
        
        if (l2 == null) {
            if (merged == null) {
                return l1;
            }
            mergedTail.next = l1;
            return merged;
        }
        
        return null;
    }
}
```
这个方法的时间复杂度是1ms

### 解决方法二
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) return l2;
        if (l2 == null) return l1;
        if (l1.val <= l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        } else {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }
    }
}
```
这个方法就是利用的递归的思路，简单的几行代码就把问题解决了，而且时间复杂度是0ms，比第一个方法性能还要好。递归的思想还是值得好好研究研究的。

## Review

### [Java: Equality vs Identity](https://medium.com/@NomadicAlex/java-equality-vs-identity-3b045c9f6c68)

本周翻译一篇较简单的文章，讲述java中==和equals之前的区别。这个相信只要学过java的都会知道，只是平时实际工作中不一定能想到要用。  
例如上面算法题的第一种方法，其实就是巧妙利用了引用特性，和==、equals之间的区别的原理都是相通的。

## Tip
### HttpRequest传LocalDate参数技巧
其实就是加一个注解@DateTimeFormat(iso = ISO.DATE)在形参的前面，这样在传参的过程中内部会根据注解进行一定的转化，否则就会报错。  
实际这个技巧也是从一篇博文中看来的，原文如下，可自行查阅：
[Parsing of LocalDate query parameters in Spring Boot](https://blog.codecentric.de/en/2017/08/parsing-of-localdate-query-parameters-in-spring-boot/)

## Share
Spring Boot 2.0 新特性和发展方向，好多也没看完，在这里记录下
### [Spring Boot 2.0 新特性和发展方向](http://blog.didispace.com/Spring-Boot-2-0-%E6%96%B0%E7%89%B9%E6%80%A7%E5%92%8C%E5%8F%91%E5%B1%95%E6%96%B9%E5%90%91/)

