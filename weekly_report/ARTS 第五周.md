## ARTS 第五周

> ### 每周完成一个ARTS： 每周至少做一个 leetcode 的算法题、阅读并点评至少一篇英文技术文章、学习至少一个技术技巧、分享一篇
> ### 有观点和思考的技术文章。（也就是 Algorithm、Review、Tip、Share 简称ARTS）  

## Algorithm
### [LeetCode - 14. Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)
判断一组字符串中最大的相同字符数，并打印出相同的字符。条件是从前往后数，且是连续的才算，这就大大减小了难度，所以仅是easy级别。

### 解题思路：
主要的思路就是将每个字符串的每个字符都往一个set集合里面放，利用set的去重性，去掉相同的字符，最后判断每个set集合的大小，只有当set
集合中有且仅有一个元素时，才可以判定每个字符串的该位置的字符是相同的。不过因为第一次没考虑到空字符串的情况，还是提交了两次才通过。  

#### 闲话不多说，上代码，第一次的：
```
class Solution {
    public String longestCommonPrefix(String[] strs) {
        StringBuffer result = new StringBuffer("");
        int shortest = 0;
        //取得字符串数组中最短的字符串的长度
        for (String str : strs) {
            if (shortest == 0 || shortest > str.length()) {
                shortest = str.length();
            }
        }
        //利用set的去重性保存字符串的各个对应位置的字符，然后判断set中保存的字符数量
        List<HashSet<Character>> sets = new ArrayList<HashSet<Character>>(shortest);
        for (int i = 0; i < shortest; i++) {
            HashSet<Character> chars = new HashSet<Character>();
            for (int j = 0; j < strs.length; j++) {
                chars.add(strs[j].charAt(i));
            }
            sets.add(chars);
        }
        //遍历各个set集，如果大小有多于1个，则说明有不同的字符
        for (HashSet set : sets) {
            if (set.isEmpty() || set.size() > 1) {
                break;
            } else {
                Iterator it = set.iterator();
                if (it.hasNext()) {
                    result.append(it.next().toString());
                }
            }
        }
        return result.toString();
    }
}
```
结果遇到["","b"]时报了异常

#### 第二次，加了对空字符串的判断：
```
class Solution {
    public String longestCommonPrefix(String[] strs) {
        StringBuffer result = new StringBuffer("");
        int shortest = 0;
        //取得字符串数组中最短的字符串的长度
        for (String str : strs) {
            if (str.length() == 0) {
                return result.toString();
            }
            if (shortest == 0 || shortest > str.length()) {
                shortest = str.length();
            }
        }
        //利用set的去重性保存字符串的各个对应位置的字符，然后判断set中保存的字符数量
        List<HashSet<Character>> sets = new ArrayList<HashSet<Character>>(shortest);
        for (int i = 0; i < shortest; i++) {
            HashSet<Character> chars = new HashSet<Character>();
            for (int j = 0; j < strs.length; j++) {
                chars.add(strs[j].charAt(i));
            }
            sets.add(chars);
        }
        //遍历各个set集，如果大小有多于1个，则说明有不同的字符
        for (HashSet set : sets) {
            if (set.isEmpty() || set.size() > 1) {
                break;
            } else {
                Iterator it = set.iterator();
                if (it.hasNext()) {
                    result.append(it.next().toString());
                }
            }
        }
        return result.toString();
    }
}
```
虽然也是费了些脑子才想起来的解决办法，但是和solution里面的解决方案比起来，还是太low了。关键solution里面的方法都没怎么看懂，还在消化中。。。


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

### 配置多数据源
application.yml: 配置参数
```
spring:
    datasource:
        driver-class-name: com.mysql.jdbc.Driver
        url: jdbc:mysql://localhost:3306/test?characterEncoding=utf8&serverTimezone=UTC&useSSL=false&allowPublicKeyRetrieval=true
        username: test
        password: test
    backup:
        driver-class-name: com.mysql.jdbc.Driver
        url: jdbc:mysql://localhost:3306/backup?characterEncoding=utf8&serverTimezone=UTC&useSSL=false&allowPublicKeyRetrieval=true
        username: test
        password: test    
```
PrimaryDataSourceConfig：主数据源初始化
```
@Configuration
@MapperScan(basePackages = "com.test.mapper.primary", sqlSessionTemplateRef = "primarySqlSessionTemplate")
public class PrimaryDataSourceConfig {

    @Value("${spring.datasource.driver-class-name}")
    String driverClassPrimary;
    @Value("${spring.datasource.url}")
    String urlPrimary;
    @Value("${spring.datasource.username}")
    String usernamePrimary;
    @Value("${spring.datasource.password}")
    String passwordPrimary;
    
    @Primary
    @Bean(name = "primaryDataSource")
    public DataSource primaryDataSource() {
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName(driverClassPrimary);
        dataSource.setUrl(urlPrimary);
        dataSource.setUsername(usernamePrimary);
        dataSource.setPassword(passwordPrimary);
        return dataSource;
    }
    
    @Primary
    @Bean(name = "primarySqlSessionFactory")
    public SqlSessionFactory primarySqlSessionFactory(@Qualifier("primaryDataSource") DataSource primaryDataSource) 
                throws Exception {
        SqlSessionFactoryBean factoryBean = new SqlSessionFactoryBean();
        factoryBean.setDataSource(primaryDataSource);
        factoryBean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources("classpath:mapper/test/*.xml"));
        return factoryBean.getObject();
    }
    
    @Primary
    @Bean(name = "primarySqlSessionTemplate")
    public SqlSessionTemplate primarySqlSessionTemplate(
            @Qualifier("primarySqlSessionFactory") SqlSessionFactory primarySqlSessionFactory) {
        return new SqlSessionTemplate(primarySqlSessionFactory);        
    }
}

```
BackupDataSourceConfig：备份数据源初始化，和主数据源类似
```
@Configuration
@MapperScan(basePackages = "com.test.mapper.backup", sqlSessionTemplateRef = "backupSqlSessionTemplate")
public class BackupDataSourceConfig {

    @Value("${spring.backup.driver-class-name}")
    String driverClassBackup;
    @Value("${spring.backup.url}")
    String urlBackup;
    @Value("${spring.backup.username}")
    String usernameBackup;
    @Value("${spring.backup.password}")
    String passwordBackup;
    
    @Bean(name = "backupDataSource")
    public DataSource backupDataSource() {
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName(driverClassBackup);
        dataSource.setUrl(urlBackup);
        dataSource.setUsername(usernameBackup);
        dataSource.setPassword(passwordBackup);
        return dataSource;
    }
    
    @Bean(name = "backupSqlSessionFactory")
    public SqlSessionFactory backupSqlSessionFactory(@Qualifier("backupDataSource") DataSource backupDataSource) 
                throws Exception {
        SqlSessionFactoryBean factoryBean = new SqlSessionFactoryBean();
        factoryBean.setDataSource(backupDataSource);
        factoryBean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources("classpath:mapper/backup/*.xml"));
        return factoryBean.getObject();
    }
    
    @Bean(name = "backupSqlSessionTemplate")
    public SqlSessionTemplate backupSqlSessionTemplate(
            @Qualifier("backupSqlSessionFactory") SqlSessionFactory backupSqlSessionFactory) {
        return new SqlSessionTemplate(backupSqlSessionFactory);        
    }
}

```

### 为每个数据源配置事务管理
TransManagerConfig
```
@Configuration
public class TransManagerConfig {
    
    @Bean(name = "primaryTransManager")
    @Primary
    public PlatformTransactionManager primaryTransManager(@Qualifier("primaryDataSource") DataSource primaryDataSource) {
        return new DataSourceTransactionManager(primaryDataSource);
    }
    
    @Bean(name = "backupTransManager")
    public PlatformTransactionManager backupTransManager(@Qualifier("backupDataSource") DataSource backupDataSource) {
        return new DataSourceTransactionManager(backupDataSource);
    }
}
```
### 使用具体的事务管理
public class Test {

    @Transactional(value = "primaryTransManager", rollbackFor = Exception.class, isolation = Isolation.READ_COMMITTED)
    public void testPrimary() {
        //todo
    }
    
    @Transactional(value = "backupTransManager", rollbackFor = Exception.class, isolation = Isolation.READ_COMMITTED)
    public void testBackup() {
        //todo
    }
}

## Share
最近工作中有用到分页插件PageHelper，就查了下使用的方法，跟大家也分享一下
### [Spring Boot集成Mybatis分页插件PageHelper](https://www.jianshu.com/p/336e17cdb40b)

之前没用过不知道，用起来才发现真的是非常简单，大致可分为3步：
1. 引入依赖
```
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper-spring-boot-starter</artifactId>
</dependency>
```
2. 参数配置： spring boot直接在application.yml中配置即可
```
pagehelper:
  helper-dialect: mysql
  reasonble: true
  support-methods-arguments: true
  params: count=countsql
```
3. 具体使用:
直接在查询之前调用PaegHelper的一个startPage方法即可
```
//currentPage-当前页数， pageSize-每页包含数据条数
PageHelper.startPage(currentPage, pageSize);
```
需注意的是一次startPage只适用于一次查询，如要进行另外的查询需再次调startPage。
另如需获取分页的其他详细信息，可利用PageInfo的相关api。