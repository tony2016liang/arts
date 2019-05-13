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

