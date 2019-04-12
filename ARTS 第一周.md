## ARTS 第一周

> ### 每周完成一个ARTS： 每周至少做一个 leetcode 的算法题、阅读并点评至少一篇英文技术文章、学习至少一个技术技巧、分享一篇有观点和思考的技术文章。（也就是 Algorithm、Review、Tip、Share 简称ARTS）  

## Algorithm
### [LeetCode - 1.Two Sum](https://leetcode.com/problems/two-sum/)
给定一个int类型的数组和一个target目标值，返回2个索引值，这两个索引值所代表的该数组中的两个值的和等于target值。默认只有唯一解。
例如：
给定数组 nums = [2, 7, 11, 15], 给定目标值target = 9,
由于 nums[0] + nums[1] = 2 + 7 = 9,
所以返回 [0, 1].

### 解题
思路：由于默认只有唯一解，且是两数的和等于目标值，所以不用考虑得很复杂。前面加了对数组长度的判断，后面用了2层for循环，感觉应该还有更好的方法，暂时没深究

```
class Solution {
    public int[] twoSum(int[] nums, int target) {
        if(nums.length == 0) {
            return null;
        }
        
        if(nums.length == 1) {
            if(nums[0] == target) {
                return nums;
            } else {
                return null;
            }
        }
        
        int[] result = new int[2];
        for(int i=0; i < nums.length; i++) {
            for(int j = i+1; j < nums.length; j++) {
                if(nums[i] + nums[j] == target) {
                    result[0] = i;
                    result[1] = j;
                    break;
                }
            }
        }
        
        return result;
    }
    
}
```


## Review
### [Why Test Driven Development (TDD) is the Best Way for Robust Coding.](https://medium.com/swlh/why-test-driven-development-tdd-is-the-best-way-for-robust-coding-a1821de51e19)
文章主要从作者自身使用实践的角度出发，介绍了测试驱动开发（TDD，即Test Driven Development）对开发健壮代码的好处，总结起来主要有以下四点：
* 可以放心修复或优化代码，而不用担心把系统改坏
* 该模式的特性即强制性地产生了最低程度的开发文档
* 促使进行更好的设计
* 最佳的代码开发实践

在我有限的开发经历中，还未有机会在实际工作中见识TDD的魅力，但作者的文章的确引起了我很大的兴趣，唯一的问题是实现起来是否容易。  

个人理解只有对需求的理解非常透彻，且需求不太会变的情况下才适合执行这种方式，而这个要求在实际工作中本就是极难实现的。  

再一个业务的复杂性问题，正如文章中所说，TDD可以优化设计，天然地会要求系统各模块之间没有耦合，但如果是一个比较复杂的系统，各个模块之间
就是需要相互依赖的，那么TDD实现起来估计也会不太容易。  

不过这种模式绝对是值得探索深究一下的。

## Tip
分享一个工作中遇到的关于MySql的Group By语句的一个小坑。  

就是某天发现有一段在开发环境运行良好的sql语句到了测试环境突然不能运行了，看了报错的原因，里面有“this is incompatible with sql_mode=only_full_group_by”的字样，百度之后，发现是mysql在5.7版本之后，都默认了sql_model=only_full_group_by，而这种模式
的要求就是SELECT后面接的列必须被GROUP BY后面接的列所包含，否则就是不合法的。而开发环境用的是5.7之前的版本，所以没问题。 

针对这个问题，网上的解决办法都是直接修改sql_model，但是我们的测试环境不能随便改参数，所以这个方法不适合。最后的办法就是改sql语句，
在满足这个要求的基础上，又达到业务的需求。

谨在此记录一下，后续group by使用时注意。

## Share
### [java并发编程之ConcurrentHashMap](https://www.jianshu.com/p/9c713de7bbdb)

虽从事开发有一段时间了，但对并发编程仍然是有点模糊，更不要说其底层实现。最近工作中正好有需要用到ConcurrentHashMap，然后发现这篇
博文介绍得比较条理清晰和通俗易懂，虽然不能通过这篇博文就透彻理解ConcurrentHashMap的底层实现，但大致的原理是懂了的。借此机会分享
出来，也算给自己mark一下，后续有需要再来翻看。

