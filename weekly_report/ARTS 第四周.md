## ARTS 第四周

> ### 每周完成一个ARTS： 每周至少做一个 leetcode 的算法题、阅读并点评至少一篇英文技术文章、学习至少一个技术技巧、分享一篇有观点和思考的技术文章。（也就是 Algorithm、Review、Tip、Share 简称ARTS）  

## Algorithm
### [LeetCode - 13. Roman to Integer](https://leetcode.com/problems/roman-to-integer/)
给定一个由7个罗马字符的字符，解析并计算出该字符串对应的值是多少。  
罗马字符对应关系如下：  
```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```
正常情况下就是直接用每个字符对应的数值算出总和就可以，如III=3，VI=6，LVIII=58。  
但对于4和9以及二者的倍数则是另外的计算规则，采取的是后方的大数减前方的小数的方式，如IV=4，是用V-I得到的，IX=9是用X-I得到的，
另外还有XL=40,XC=90,CD=400,CM=900等。这是本题的一个稍难的点。

### 解题思路：
主要就是顺序解读，在解读的过程中对特殊的情况加进行判断以进行特殊的计算。  
在分享自己的答案的过程中，照旧同时分享下自己的失败过程。本题我是提交到第三次才通过的，前两次都有考虑不周到之处。闲话不多说，先上
代码。

#### 第一次提交

```
class Solution {
    public int romanToInt(String s) {
        int sum = 0;
        if (s != null) {
            int length = s.length();
            String[] strArray = s.split("");
            for(int i=length-1; i >= 0; i-=2) {
                if(i-1 >= 0) {
                    sum += getFromTwoCode(strArray[i], strArray[i-1]);
                } else {
                    sum += Roman.getNumberByCode(strArray[i]);
                }
            }
        }
        return sum;
    }
    
    public Integer getFromTwoCode(String code1, String code2) {
        if((("V".equals(code1) || "X".equals(code1)) && "I".equals(code2))
           || (("L".equals(code1) || "C".equals(code1)) && "X".equals(code2)) 
           || (("D".equals(code1) || "M".equals(code1)) && "C".equals(code2))) {
            return Roman.getNumberByCode(code1) - Roman.getNumberByCode(code2);
        } else {
            return Roman.getNumberByCode(code1) + Roman.getNumberByCode(code2);
        }
    }
}

enum Roman {
    ONE("I", 1),
    FIVE("V", 5),
    TEN("X", 10),
    FIFTY("L", 50),
    HUND("C", 100),
    FIVHUN("D", 500),
    THOUD("M", 1000);
    
    private String code;
    private Integer number;
    
    Roman(String code, Integer number) {
        this.code = code;
        this.number = number;
    }
    
    public static Integer getNumberByCode(String code) {
        for(Roman r : Roman.values()) {
            if(code.equals(r.code)) {
                return r.number;
            }
        }
        return 0;
    }
}
```  

从代码中可以看到，我是将罗马字符与数值的对应关系抽象成了一个枚举类Roman，这样方便在执行运算时取字符对应的值，代码相对来说也较清楚一些。
然后计算具体数值的逻辑我是给放在了一个getFromTwoCode方法中，真正的控制逻辑在系统默认的romanToInt中。  
现在回过头想，第一次提交时其实连题目都是没看透的，对于罗马字转数值的计算，手算都还没太清楚，所以代码必然是有问题的。于是这次提交系统给出
的反例是"MCDLXXVI"，应该的计算结果是1476，我给出的结果是1676，因为我是从后往前遍历的，且每次跳2个，所以就把应该相减的CD漏了，从而总值
多了200.  

#### 第二次提交

```
class Solution {
    public int romanToInt(String s) {
        int sum = 0;
        if (s != null) {
            int length = s.length();
            String[] strArray = s.split("");
            // for(int i=length-1; i >= 0; i-=2) {
            //     if(i-1 >= 0) {
            //         sum += getFromTwoCode(strArray[i], strArray[i-1]);
            //     } else {
            //         sum += Roman.getNumberByCode(strArray[i]);
            //     }
            // }
            for(int i=0; i<length; i++) {
                if("I".equals(strArray[i]) || "X".equals(strArray[i]) || "C".equals(strArray[i])) {
                    if(i+1 < length) {
                        sum += getFromTwoCode(strArray[i+1], strArray[i]);
                        i++;
                    } else {
                        sum += Roman.getNumberByCode(strArray[i]);
                    }
                } else {
                    sum += Roman.getNumberByCode(strArray[i]);
                }
            }
        }
        return sum;
    }
    
    public Integer getFromTwoCode(String code1, String code2) {
        if((("V".equals(code1) || "X".equals(code1)) && "I".equals(code2))
           || (("L".equals(code1) || "C".equals(code1)) && "X".equals(code2)) 
           || (("D".equals(code1) || "M".equals(code1)) && "C".equals(code2))) {
            return Roman.getNumberByCode(code1) - Roman.getNumberByCode(code2);
        } else {
            return Roman.getNumberByCode(code1) + Roman.getNumberByCode(code2);
        }
    }
}

enum Roman {
    ONE("I", 1),
    FIVE("V", 5),
    TEN("X", 10),
    FIFTY("L", 50),
    HUND("C", 100),
    FIVHUN("D", 500),
    THOUD("M", 1000);
    
    private String code;
    private Integer number;
    
    Roman(String code, Integer number) {
        this.code = code;
        this.number = number;
    }
    
    public static Integer getNumberByCode(String code) {
        for(Roman r : Roman.values()) {
            if(code.equals(r.code)) {
                return r.number;
            }
        }
        return 0;
    }
}
```
第二次的改动主要是2处：一是遍历变成了从前往后，而且每次跳1，而不是跳2；二是在控制逻辑中加多了判断，并将跳2的操作放在这里面执行。这次是重新审了题，
思路也更清晰了些，但是仍然考虑不充分，所以还是有问题。这次提交系统给出的反例是"MDCCCLXXXIV"，结果应该是1884，而我给出了1886.实际看代码就可以很容易
地看出来，我在进行for循环里面的第2个if判断是的条件i+1 < length并不充分，造成不该进入该分支的计算也进来了，从而将应该和V一起计算的I拉到了前面和X一起
算了，这样前后就差了2，从而得到了错误的结果。  

#### 第三次提交：

```
class Solution {
    public int romanToInt(String s) {
        int sum = 0;
        if (s != null) {
            int length = s.length();
            String[] strArray = s.split("");
            
            for(int i=0; i<length; i++) {
                if("I".equals(strArray[i]) || "X".equals(strArray[i]) || "C".equals(strArray[i])) {
                    if(i+1 < length && isNeedTwo(strArray[i+1], strArray[i])) {
                        sum += (Roman.getNumberByCode(strArray[i+1]) - Roman.getNumberByCode(strArray[i]));
                        i++;
                    } else {
                        sum += Roman.getNumberByCode(strArray[i]);
                    }
                } else {
                    sum += Roman.getNumberByCode(strArray[i]);
                }
            }
        }
        return sum;
    }
    
    public Boolean isNeedTwo(String code1, String code2) {
        return (("V".equals(code1) || "X".equals(code1)) && "I".equals(code2))
           || (("L".equals(code1) || "C".equals(code1)) && "X".equals(code2)) 
           || (("D".equals(code1) || "M".equals(code1)) && "C".equals(code2));
    }
}

enum Roman {
    ONE("I", 1),
    FIVE("V", 5),
    TEN("X", 10),
    FIFTY("L", 50),
    HUND("C", 100),
    FIVHUN("D", 500),
    THOUD("M", 1000);
    
    private String code;
    private Integer number;
    
    Roman(String code, Integer number) {
        this.code = code;
        this.number = number;
    }
    
    public static Integer getNumberByCode(String code) {
        for(Roman r : Roman.values()) {
            if(code.equals(r.code)) {
                return r.number;
            }
        }
        return 0;
    }
}
```
这次是通过了的提交，尽管效果并不理想。代码中可以看到，这次是将计算的逻辑都放在romanToInt方法中了，只是将判断的逻辑给抽取了出来，总体逻辑上就是相当于
计算特殊部分的条件更严密了而已。  
从结果上看，性能还勉强能算是中等偏上，空间复杂度就很差了。看来后续可优化的空间还是很大的。


## Review
本周一不小心看了一个系列文章，讲函数式编程的，总共是6篇。主要是函数式编程的入门介绍和Elm编程语言的推荐。这次先简单介绍两章，下周再继续分享其他的。
### [So You Want to be a Functional Programmer (Part 1)](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-1-1f15e387e536)
首先作者是用开车和开宇宙飞船做比喻，来形象地描述学习函数式编程的过程。开头总是最难的，但只要入了门，掌握了必要的规则，就好比拿到了驾照的你，再也不会视
开车为畏途，即使换了新车，也可以很快掌握。  
开宇宙飞船和开车又完全不同。甚至开车的规则这时都不会起作用了（比如开到一半发现方向错了，你是要像开车一样掉头，然后往回开吗？路都没有，何来的掉头？）。
类似函数式编程和命令式编程的区别。这时能做的就是尽量忘掉之前的一切，一切从头开始，从全新的规则开始，很快你就会发现原先的方式是多么的糟。  
吹过牛后，作者开始以实例入手，逐渐将函数式编程与命令式编程的区别最大的几个特点点了出来，如下：  
1. 纯粹性：纯粹的函数是最简单的函数，其往往有至少一个入参，并会有一个返回的结果。但是纯粹的函数也只会操作其入参，而不会引入外来的其他变量（即不会有状态），而且
保证相同的入参就会得到相同的结果，即不会有副作用；
2. 不变性：函数式编程是没有变量的，比如命令式编程中会有这样的语句：
```
var x = 1;
x = x + 1;
```
这段代码的意思是将变量x的值加1，再将新的值赋给x变量。在函数式编程中，这段代码会是非法的，因为在数学中，就不会有x = x + 1这样的等式。在函数式编程中，当
用x = 1对x进行赋值之后，x就是1，不会再是别的值。所以函数式编程中没有变量，其操作的都是常量。
3. 函数式编程中没有循环（loop），命令式编程中的循环的逻辑，函数式编程都是用递归（recursion）实现的。
4. 不变性带来的是更简单和更安全的代码。

### [So You Want to be a Functional Programmer (Part 2)](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-2-7005682cec4a)
第二部分主要是与JavaScript做对比，来讲解函数式程序代码的写法，并简单介绍了高阶函数和闭包（由于作者主要是要推荐Elm前端开发语言，而Elm是基于JS并想取代
JS的一门语言，所以必然要在与JS的对比中体现其优越性）。  
1. 重构（Refactoring），通过对一段JS代码的进一步抽象，提供了一种从JS向Elm重构的方式；
2. 高阶函数（Higher-Order Functions），第1点讲抽象时，将很多变量抽象成了一个函数的入参，由此进一步的考虑就是，函数本身是否可以作为另一个函数的入参？
所以就有了高阶函数，即函数的函数。同时指出了，在函数式编程中，函数就是一等公民，函数不仅可以作为入参，还可以作为结果进行返回；
3. 闭包（Closures），闭包属于管理状态的一种方式，由于函数式编程中不提倡变量，也即没有状态，但实际业务却经常是需要保存状态的，而函数式编程就是靠闭包实现
这一点的，其利用的就是函数的作用域的不同，当一个函数被创建出来之后，其作用域包括其中的变量也即被创建出来，然后只要对这个函数的引用还没有消除，那么这个函
数的作用域就还有效，其中的变量也就还是可以访问的（我对闭包还不是很理解，平常的工作中也从没用过，以上只是我的一些个人浅见，如有不对的地方还请指正）。


## Tip
### 这次还是从填坑的角度分享两个实际工作中遇到的问题，主要是MySql数据库的。
#### 1. MySql字符集不同造成的索引无效
生产上线时，发现有个数据迁移的sql执行的特别慢，基本是堵塞状态了。经过排查，发现是需要的几个索引都还没在生产数据库中建立起来（为啥没建的具体原因不清楚，
反正情况就是这样了），于是只好重新建索引，然后建好后重新跑。  
结果跑了半天，怎么还是这么慢，再一看，又被堵住原来的地方。不应该啊，不是有索引了吗？于是，一查，索引是有的，只是没起作用！
于是又是各种google、咨询他人、各种尝试，详情就不细说了，最后发现，是因为字符集不同造成的。比如两张表的字符集不同，却需要关联查询，而MySql为了在字符集
转换过程中不损失数据，就需要先将字符串从一个字符集转为另一个字符集，于是就变成了全表查询。  

#### 2. MySql删除大表数据慎用delete，尽量用truncate
在一次生产预发布的时候，突然反馈说数据库崩掉了，造成数据库异常重启，且需回滚恢复数据。而造成这次数据库崩掉的元凶就是对一张大表数据的删除操作。  
原来，我们有一张大表，字段不多，只有十来个，但数据量比较大，有约一亿三千万条数据，而在发布生产前，需要对这张表里的数据进行清空，于是开发的同事不管三七
二十一，直接来了个delete from ...，结果造成数据库日志激增，直至把硬盘撑爆，把数据库搞崩。  
其实很多道理我们都懂，却总是要等到犯了错误了才会去真正注意。  
delete删除和truncate删除的区别学习数据库知识时应该都有学过，delete删除会完整记录日志，虽然性能一般且产生的日志也很占空间，但为的是可以对删除的数据进行
恢复。而truncate则不会记录日志，自然删的又快，所占空间又小，只是删除后也就无法再恢复了。  
所以删除大量数据的最好方式是用truncate，如果实在为了保险，想用delete，也可以采取一种折中的办法，即对delete语句进行一定条件的限制，每次只删除一部分，比
如一百万条，将一个大任务分成若干个小任务分多次执行，这样就既不会造成日志拥堵，又可以保留记录，以备事后必要时进行恢复。

## Share
### 由于前面的坑，又把之前已经忘记的或者之前还没记住的内容又补了补，在此分享几篇相关的博文吧
#### [详解MySQL中EXPLAIN解释命令](http://database.51cto.com/art/200912/168453.htm)
#### [索引无效——字符集惹的祸](https://blog.csdn.net/xinghun61/article/details/5747344)
#### [从SQL Server删除大数据说开去](http://database.51cto.com/art/201203/326471.htm)

