## ARTS 第五周

> ### 每周完成一个ARTS： 每周至少做一个 leetcode 的算法题、阅读并点评至少一篇英文技术文章、学习至少一个技术技巧、分享一篇
> ### 有观点和思考的技术文章。（也就是 Algorithm、Review、Tip、Share 简称ARTS）  

## Algorithm
### [LeetCode - 26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
> Given a sorted array nums, remove the duplicates in-place such that each element appear only once and return the new length.
> Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.
Example :
```
Given nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.

It doesn't matter what you leave beyond the returned length.
```

### 解题思路
题目本身不难，但是因为有一个只能在数组内部调整的限制条件，所以就稍微有了点难度。主要考察的是用多个指针通过操控数组下标进而操控数值的能力。
需要思维比较缜密。我的前两次提交就都是因为考虑不周，造成java.lang.ArrayIndexOutOfBoundsException异常，从而校验失败的。

### 以下为提交成功的代码
```
class Solution {
    public int removeDuplicates(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        } else if (nums.length == 1) {
            return 1;
        }
        // 用于比较的变量，可视作基准值
        int temp = nums[0];
        // 用于指示不同的遍历层次，其中j表示数组整体移动到的下标，i表示已去重的数组部分的下标
        int i, j; 
        for (i=1,j=1; i<nums.length - 1 && j<nums.length - 1; ) {
            // nums[i]表示待返回数组中未确定的一个位置，如果等于基准值，则需要继续遍历整个数组，寻找不同的值
            if (nums[i] == temp) {
               nums[i] = nums[j + 1];
               j++;
            } else {
                // 考虑到{0,1,2,1,1,3,...}的情况，如果nums[i]不等于基准值，需要比较两者的大小
                // 如果nums[i]小于基准值，表示待返回又可以向前推进一位
                // 如果nums[i]不小于基准值，则表示还需继续向后遍历整体数组，以查看是否还有更大的数
                if (temp < nums[i]) {
                    temp = nums[i];
                    i++;
                } else {
                    nums[i] = nums[j + 1];
                    j++;
                }

            }
        }
        // 针对原始数组为{0, 0, 1, 1, 1, 2, 2, 3, 3, 4, 5, 6, 7, 8, 8}
        // 去重后的数组可能会是{0, 1, 2, 3, 4, 5, 6, 7, 8, 8, 5, 6, 7, 8, 8}这种类型
        // 此时的i=9,所以实际应该返回的数组长度为9
        if (nums[i] == nums[i-1]) {
            j = i;
        } else {
            j = i+1;
        }

        for (i=j;i<nums.length; i++) {
            nums[i] = 0;
        }

        return j;
    }
}
```


## Review

### [How to handle very large numbers in Java without using java.math.BigInteger](https://stackoverflow.com/questions/5318068/how-to-handle-very-large-numbers-in-java-without-using-java-math-biginteger)

此文来自于StackOverFlow上对一个问题的回答，其实原本作者是有贴心地准备另一个更具可读性的版本的，但这个[版本](https://paul-ebermann-blog.tumblr.com/post/6277562800/big-numbers-self-made-part-014-introduction)
似乎目前只有第一页概述部分可以访问，其他部分都访问不到了，所以上面贴的是StackOverFlow原帖的地址。

## Tip
### Java 8 Stream 教程

[Java 8 Stream 教程](https://www.jianshu.com/p/0c07597d8311)

## Share
如何判断int类型数值溢出，自己一直不是很熟，mark下
### [Java整型变量溢出的判断方法](https://my.oschina.net/u/3284953/blog/1621263)

同时复习下原码/反码/补码的相关知识和背后的原理
### [原码, 反码, 补码 详解](https://www.cnblogs.com/zhangziqiu/archive/2011/03/30/ComputerCode.html)

BigInteger的常用方法
### [Java中BigInteger方法总结](https://www.jianshu.com/p/8b89ab19db84)
网上总结BigInteger用法的帖子很多，却都不太全面（自己也懒得总结(#-.-)），以上的帖子算是较为全面的了，另外补充几个我用到过而这里面没有的:

1. valueOf构造
直接new一个BigInteger当然是用String或byte[]作为入参比较方便，但如果手头正好有long或int值，用new就不方便了，这时就可以用valueOf
```
BigInteger hun = BigIteger.valueOf(100);
```
实际上，BigInteger.ONE, BigInteger.TWO 等就是调用内部的valueOf方法生成的

2. 重载的toString
BigInteger的toString()方法默认是返回十进制的数值转化成的字符串，但实际上toString还有个含一个入参的重载方法，传入的参数即代表输出的字符串
要以几进制的方式来进行展现，如toString(16)即表示以16进制的模式进行展示。



