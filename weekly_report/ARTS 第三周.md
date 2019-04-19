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


## Tip
### 日期时间转换
日期时间转换的坑太多，虽也曾从总结的角度去梳理过，但一段时间过后，又会全然忘记。所以现在的模式就是碰到一个解决一个。 
 
这次的场景是我们的消息要过一遍elasticSearch，消息中有一个字段updatedTime，插入es之前的格式是“yyyy-MM-dd HH:mm:ss”，
插入es后会默认给转换成“yyyy-MM-dd'T'HH:mm:ss.SSS Z”格式，，然后再取出来的时候如果不加以处理的话就会报异常。  

我们的处理方式如下：

```
public static LocalDateTime parse(String sourceDate) {
        try {
            SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ss.SSS Z");
            Date date = format.parse(sourceDate.replace("Z", " UTC"));
            return LocalDateTime.ofInstant(date.toInstant(), ZoneId.systemDefault());
        } catch (ParseException e) {
            e.printStackTrace();
        }
        return null;
    }
```

## Share
### [java UTC时间格式化 时间带T Z](http://www.weizhixi.com/user/index/article/id/70.html)
上面解决日期时间转换时看的一篇博文，分享一下
