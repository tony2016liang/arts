## ARTS 第二周

> ### 每周完成一个ARTS： 每周至少做一个 leetcode 的算法题、阅读并点评至少一篇英文技术文章、学习至少一个技术技巧、分享一篇有观点和思考的技术文章。（也就是 Algorithm、Review、Tip、Share 简称ARTS）  

## Algorithm
### [LeetCode - 7.Reverse Integer](https://leetcode.com/problems/reverse-integer/)
Given a 32-bit signed integer, reverse digits of an integer.

### 解题思路：  
先判断正负，然后从低位到高位循环处理每一位数，代码如下：

```
class Solution {
    public int reverse(int x) {
        //判断正负
        int flag = 0;
        if (x > 0) {
            flag = 1;            
        } else if (x < 0) {
            flag = -1;
            x = -x;
        } else {
            return flag;
        }
        
        //被除数每次*10，当被除数>原数，循环退出
        //步骤就是从低位到高位每次处理一位，最后要判断正负
        int result = 0;
        for (int i = 1; ; i *= 10) {
            if (i > x) {
                break;
            }
            int temp = x/i%10; 
            
            result = result * 10 + temp;
        }
        
        return flag > 0 ? result : -result;
        
        //int ge = x % 10;个位
        //int shi = x / 10 % 10;十位
        //int bai = x / 100 % 10;百位
        
    }
}
```
但是这个代码只是自测的时候没看出问题，一旦提交，就报了如下异常
```
java.lang.ArithmeticException: / by zero
  at line 21, Solution.reverse
  at line 85, MainClass.main
```
后来分析了自己的代码，最关键参考了下答案的算法，知道应该是自己没有判断int范围造成的，于是代码改成如下：
```
class Solution {
    public int reverse(int x) {
        //判断正负
        int flag = 0;
        if (x > 0) {
            flag = 1;            
        } else if (x < 0) {
            flag = -1;
            x = -x;
        } else {
            return flag;
        }
        
        //被除数每次*10，当被除数>原数，循环退出
        //步骤就是从低位到高位每次处理一位，最后要判断正负
        int result = 0;
        for (int i = 1; ; i *= 10) {
            if (i > x) {
                break;
            }
            int temp = x/i%10; 
            if (result > Integer.MAX_VALUE/10 
                || (result == Integer.MAX_VALUE / 10 && temp > 7)) {
                return 0;
            }
                
            if (result < Integer.MIN_VALUE/10 
                || (result == Integer.MIN_VALUE / 10 && temp < -8)) {
                return 0;
            }
            result = result * 10 + temp;
        }
        
        return flag > 0 ? result : -result;
        
        //int ge = x % 10;个位
        //int shi = x / 10 % 10;十位
        //int bai = x / 100 % 10;百位
        
    }
}
```
结果提交时仍然报错，这次不是运行异常，而是得出的答案和预期的不符，原来当输入为-2147483412时，for循环中的第一个if判断条件会进去，然后直接返回0了，而实际上应该返回-2143847412的。但是如果不进行判断，直接执行后面的*10+temp的话，又会int溢出，并报运行时异常。看来答案也是不能随便抄的。
由于自己没掌握其中的窍门，只能边试边看了，通过加日志调试，代码又调整了一版：
```
class Solution {
    public int reverse(int x) {
        //判断正负
        int flag = 0;
        if (x > 0) {
            flag = 1;            
        } else if (x < 0) {
            flag = -1;
            x = -x;
        } else {
            return flag;
        }
        
        //被除数每次*10，当被除数>原数，循环退出
        //步骤就是从低位到高位每次处理一位，最后要判断正负
        int result = 0;
        for (int i = 1; ; i *= 10) {
            if (i > x) {
                break;
            }
            int temp = x/i%10; 
            if (result > Integer.MAX_VALUE/10 
                || (result == Integer.MAX_VALUE / 10 && temp > 7)) {
                System.out.print("result:" + result);
                System.out.print(",i:" + i);
                System.out.print(",x:" + x);
                System.out.print(",Integer.MAX_VALUE/10:" + Integer.MAX_VALUE/10);
                System.out.print(",Integer.MAX_VALUE / 10:" + Integer.MAX_VALUE / 10);
                break;
            }
                            
            result = result * 10 + temp;
        }
        
        return flag > 0 ? result : -result;
        
              
    }
}
```
这回是针对-2147483412的输入返回了正确的答案，但当提交时还是有了问题，即当输入是1534236469时，期望的返回是0，而我却返回了964632435，所以还是int范围的界定问题没搞定，还是自己的代码有问题。于是，继续搞。  

在改的过程中，我意识到也许自己一开始的思路就是有点问题的，因为对于负值来说，改成正值可能更易于理解，但对于计算和判断边界值反而有点麻烦。另外我采用的循环判断条件也有点别扭，而且采用这种方式的话不光要考虑待输出的数的边界问题，还要考虑判断条件中i的边界问题，否则也会造成返回值和预期不符。总而言之，几经修改，总算是通过校验，提交成功，最终代码如下：
```
class Solution {
    public int reverse(int x) {
        //判断正负
        int flag = 0;
        if (x > 0) {
            flag = 1;            
        } else if (x < 0) {
            flag = -1;
            x = -x;
        } else {
            return flag;
        }
        
        //被除数每次*10，当被除数>原数，循环退出
        //步骤就是从低位到高位每次处理一位，最后要判断正负
        int result = 0;
        for (long i = 1L; ; i *= 10) {
            if (i > x) {
                break;
            }
            int ti = (int)i;
            int temp = x / ti % 10; 
            if (result > Integer.MAX_VALUE/10 
                || (result == Integer.MAX_VALUE / 10 && temp > 7)) {
                if (flag > 0) {
                    return 0;
                } else {
                    if (-result < Integer.MIN_VALUE/10 
                       || (- result == Integer.MIN_VALUE/10 && temp > 8)) {
                        return 0;
                    }
                }
            }
                            
            result = result * 10 + temp;
        }
        
        return flag > 0 ? result : -result;
        
              
    }
}
```
耗时1ms，占用内存32.3MB，超过了100%的针对该问题的java算法，看来可优化的空间还很大。
另附上系统给出的标准答案，如下：
```
class Solution {
    public int reverse(int x) {
        int rev = 0;
        while (x != 0) {
            int pop = x % 10;
            x /= 10;
            if (rev > Integer.MAX_VALUE/10 || (rev == Integer.MAX_VALUE / 10 && pop > 7)) return 0;
            if (rev < Integer.MIN_VALUE/10 || (rev == Integer.MIN_VALUE / 10 && pop < -8)) return 0;
            rev = rev * 10 + pop;
        }
        return rev;
    }
}
```

## Review
### [A Study List for Java Developers](https://medium.com/@jianbao.tao/a-study-list-for-java-developers-56a3f35527a5)
作者从自己的工作实践出发，推荐了JAVA开发者应该学习和关注的领域和方向，简单总结下，详情可看原文。
1. 整洁的代码：在此作者推荐了 _Clean Code_ 和 _Effective Java_ 两本书，而且作者分享了自己训练的方式，那就是通过 _LeetCode_
2. JDK官方文档：尤其重点推荐了几个重要的部分，如泛型、JavaBeans等
3. Gradle构建工具
4. Jetty
5. Hadoop
6. Java EE
7. Spring
8. Zookeeper
9. 定位问题的能力：可以先学习怎么在 _Stack Overflow_ 上提问题

从中可以看出作者一方面非常重视基础的能力，另一方面对技术的发展特别是大数据和分布式等的重视程度。作为一个开发小白，以上的每一条也都可以作为自己深耕的方向。  

### [Why Do Arrays Start With Index 0?](https://medium.com/@albertkoz/why-does-array-start-with-index-0-65ffc07cbce8)
另外分享一个有趣的小知识点，关于为什么数组下标从0开始的问题，可以点开原文看下。

## Tip
### 我不知道我有没有这方面的天赋，这条路我还是否能走下去？是否需要及时回头去寻找其他的路？
本周没有学习什么值得分享的技术技巧，但却通过阅读博文，再加上自己的一些实际经历，从而有了一点点感悟，在此分享给大家。  
题目的问题是我一直以来心中的疑问，由于我的略显复杂和曲折的经历，我对自己的能力和未来有很大的不自信和不确定性，现在虽然稍有改观，但仍然
疑虑甚多。因此这点感悟我写出来，既算是对自己的一点点交待，以稍稍减轻内心的疑虑，也可作为有同样心情的朋友的参考。具体过程就不详写了，
有同样问题的人应该会自然地有所体悟，
1. 天赋并不都是天生的，也是可以训练出来的；天赋并不总是那么明显的，可能需要你多付出一点，才会发现自己原来也是有天赋的
2. 自己认定并且经过深思熟虑的路就有值得走下去的必要，不管这条路是否能通向你所向往的终点。但路总没有白走的，这句话只有真正走过的人才会真
的有体会
3. 人生从来都是取舍的过程。所以，当觉得走不下去想回头时，那就回头吧，因为再往前走，也不一定有你想要的东西，说不定回头反而会有新的发现。
但如果决定继续走下去，那就不要犹豫，勇往直前下去，你一定会看到更美好的明天。  

最近看到一篇博文说，梦想蒙蔽了很多人的双眼，使人们都寄希望于将来那若有若无的想象，但实际生活当中绝大多数人的梦想却注定是无法实现的。
很现实，却深得我心。但正如作者所言，揭露这种现实的残酷，并不是要打击大家的信心，散布恐慌的情绪，而是要借现实的这一面，使大家警醒，从而
能更脚踏实地地做好自己。俗话说，努力并不一定会有想要的结果，但努力也从来不会一无所获。


## Share
### [java int溢出总结](https://njucz.github.io/2017/08/16/java-int%E6%BA%A2%E5%87%BA%E6%80%BB%E7%BB%93/)

由之前的算法题意识到了自己缺漏的一个知识点，关于int类型数据的范围问题，以及溢出时的处理问题，以上文章即分享了关于此问题的一些知识。

