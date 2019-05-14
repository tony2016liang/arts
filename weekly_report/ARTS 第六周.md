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

#### 第一次提交的代码
```
class Solution {
    public boolean isValid(String s) {
        //判断字符串不为空的情况，如果为空则是true
        if(!s.equals("")) {
            //如果字符串的s长度为奇数，则为false
            if(s.length() != 0 && s.length() % 2 != 0)  return false;
            
            //依次判断字符串中字符，为左括号则入栈，为右括号则出栈，并将其与出栈的字符比较
            Stack<String> strStack = new Stack<>();
            String chars = "(){}[]";
            String[] strs = s.split("");
            for(String str:strs) {
                //如果输入的字符串中包含除6个括号以外的字符，也为false
                if(!chars.contains(str)) return false;
                
                if("(".equals(str) || "[".equals(str) || "{".equals(str)) {
                    strStack.push(str);
                } else if (")".equals(str) || "]".equals(str) || "}".equals(str)) {
                    if(!isMatch(strStack.pop(), str)) return false;
                } else {
                    continue;
                }
            }
            return true;
        } 
        
        return true;
    }
    
    public boolean isMatch(String pre, String pos) {
        if("(".equals(pre) && ")".equals(pos)) {
            return true;
        } else if("[".equals(pre) && "]".equals(pos)) {
            return true;
        } else if("{".equals(pre) && "}".equals(pos)) {
            return true;
        } else {
            return false;
        }
    }
               
}
```
第一次的代码测试是失败的，因为没有考虑空栈的情况，比如如果一开始就传的是一个闭括号，那么栈里面是没有对应的开括号可以弹出的，所以这次
在测试"){"时失败了

#### 第二次提交的代码
```
class Solution {
    public boolean isValid(String s) {
        //判断字符串不为空的情况，如果为空则是true
        if(!s.equals("")) {
            //如果字符串的s长度为奇数，则为false
            if(s.length() != 0 && s.length() % 2 != 0)  return false;
            
            //依次判断字符串中字符，为左括号则入栈，为右括号则出栈，并将其与出栈的字符比较
            Stack<String> strStack = new Stack<>();
            String chars = "(){}[]";
            String[] strs = s.split("");
            for(String str:strs) {
                //如果输入的字符串中包含除6个括号以外的字符，也为false
                if(!chars.contains(str)) return false;
                
                if("(".equals(str) || "[".equals(str) || "{".equals(str)) {
                    strStack.push(str);
                } else if (")".equals(str) || "]".equals(str) || "}".equals(str)) {
                    //这里要对strStack判空，考虑输入为“){”的情况
                    if(strStack.empty() || !isMatch(strStack.pop(), str)) return false;
                } else {
                    continue;
                }
            }
            //如果此时strStack不为空，返回false，考虑“((”的情况
            if(strStack.empty()) 
                return true;
            else
                return false;
            
        } 
        
        return true;
    }
    
    public boolean isMatch(String pre, String pos) {
        if("(".equals(pre) && ")".equals(pos)) {
            return true;
        } else if("[".equals(pre) && "]".equals(pos)) {
            return true;
        } else if("{".equals(pre) && "}".equals(pos)) {
            return true;
        } else {
            return false;
        }
    }
               
}
```
这次的代码逻辑没有问题了，但性能上有点差强人意，时间上消耗10ms，内存用了35.4M。  
于是稍微简化了下代码，看性能上是否会有什么变化。

#### 第三次提交的代码
```
class Solution {
    public boolean isValid(String s) {
        //判断字符串不为空的情况，如果为空则是true
        if(!s.equals("")) {
            //依次判断字符串中字符，为左括号则入栈，为右括号则出栈，并将其与出栈的字符比较
            Stack<String> st = new Stack<>();
            String s1 = "({[";
            String s2 = ")}]";
            String[] strs = s.split("");
            for(String str:strs) {
                //如果输入的字符串中包含除6个括号以外的字符，也为false
                if(!(s1+s2).contains(str)) return false;
                
                if(s1.contains(str)) {
                    st.push(str);
                } else if (s2.contains(str)) {
                    //这里要对st判空，考虑输入为“){”的情况
                    if(st.empty() || !isMatch(st.pop()+str)) return false;
                } else {
                    continue;
                }
            }
            //如果此时st不为空，返回false，考虑“((”的情况
            if(st.empty()) 
                return true;
            else
                return false;
            
        } 
        
        return true;
    }
    
    public boolean isMatch(String str) {
        return "()".equals(str) || "[]".equals(str) || "{}".equals(str);
    }
               
}
```
这次把上次的48行代码简化到了38行，不过性能却更差了。。。好吧，至少证明了性能和代码的长短是没关系的，性能的提升之路还很长啊

## Review
So You Want to be a Functional Programmer (Part 5 & Part 6) 。
### [Part5](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-5-c70adc9cf56a)
1. 透明引用（Referential Transparency）：这里作者称之为逆向重构（Reverse Refactoring）。意思就是函数式编程中引用的函数完全可以用
其原函数体代替，而不会产生任何副作用。这个特性比较有助于对函数作用的推理，尤其是递归函数。
2. 执行顺序（Execution Order）：这一部分主要就是讲述函数式编程天生就是适合并发的，因为其每个函数的执行都是无副作用的，所以不用考虑函数
执行过程中的状态问题，自然也就可以多个任务并行而不会产生不同任务执行结果不同的现象。
3. 类型注解（Type Annotations）：Java类型安全但太麻烦，JavaScript使用灵活但安全方面又有点欠缺，所以Elm综合了两者的优点，方法就是通过
在函数定义之前加多一行类型注解，对函数的入参和出参进行约束，尽管刚开始会在理解这些注解方面有些困扰，但其的确是解决了JS中的动态类型可能
会产生的问题。

### [Part6](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-6-db502830403)
最后一部分主要就实际的使用推荐了一些库文件，比如JavaScript的Immutable.js和Ramda，另外是就Elm的学习提供了一些资源和学习方法，最后就
将来的形式做了一些简单的预计。

## Tip
分享一个IntelliJ IDEA使用小技巧，debug模式仅显示需要观察的字段（可能许多人都已经知道了，但对于我来说还是有点小意外的，我相信应该还是有
不少人不知道这一点的），以下为示例代码：

```
//测试类
public class IdeaTest {

    public static void main(String[] args) {
        People pe = new People();
        System.out.println(pe);
        pe.refresh();
        System.out.println(pe);
    }

}

//用于展示的实体类
class People {
    private String name_one;
    private String name_two;
    private String name_three;
    private String name_four;
    private String name_five;
    private String name_six;
    private String name_seven;
    private String name_eight;

    public People () {
        this.name_one = "one";
        this.name_two = "two";
        this.name_three = "three";
        this.name_four = "four";
        this.name_five = "five";
        this.name_six = "six";
        this.name_seven = "seven";
        this.name_eight = "eight";
    }

    @Override
    public String toString() {
        return "people{" +
                "name_one='" + name_one + '\'' +                
                ", name_eight='" + name_eight + '\'' +
                '}';
    }

    public void refresh() {
        this.name_eight = "eight_refresh";
    }

}
```

附上debug时的截图（其中红色框中为idea自动展示的东西）
![debug截图](../picture/idea%20debug小技巧.png)

debug时经常需要排查对象的字段值变化情况，我以前的做法都是在console中把对象点开，然后一个个找需要查看的字段，费劲又耗时。而idea默认就
是会展示当前对象中的字段，只是以前没注意观察过其中的规则，所以也就没利用起来。现在终于知道了，原来这个地方会展示什么字段，是根据对象的
**toString()**方法来的，所以如果你只想跟踪一两个字段的值变化情况，那不妨重写一下**toString()**方法。这个技巧对于一些有很多字段的大对象来说，
尤其有效。

## Share

这是在工作中遇到的一个问题，UnsupportedOperationException异常，看了下面的文章才知道原来通过Arrays.asList()生成的ArrayList对象并不是
我们常用的那个java.util.ArrayList，而是Arrays类中的一个内部类，其并没有实现对集合进行修改的方法，又长见识了，详情请查看原文。
### [Java容器——UnsupportedOperationException](https://my.oschina.net/itblog/blog/524792)

