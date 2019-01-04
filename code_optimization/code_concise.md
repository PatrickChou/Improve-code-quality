#### 我们将从几个方面展开讨论：
<a name="back-to-top"></a>
1. <a href="#variables">变量</a>
2. <a href="#function">函数</a>
3. 对象和数据结构
4. 类
5. SOLD
6. 测试
7. 异步
8. 错误处理
9. 代码风格
10. 注释

# 变量
<a name="variables"></a>
### 用有意义且常用的单词命名变量
<strong>Bad: </strong>
```ruby
const yyyymmdstr = moment().format('YYYY/MM/DD');
```
<strong>Good: </strong>
```ruby
const currentDate = moment().format('YYYY/MM/DD');
```
<a href="#back-to-top">↑回到顶部</a>

### 保持统一

可能同一个项目对于获取用户信息，会有三个不一样的命名。应该保持统一，如果你不知道该如何取名，可以去 <a target="_blank" href="https://link.juejin.im?target=https%3A%2F%2Funbug.github.io%2Fcodelf%2F" rel="nofollow noopener noreferrer">codelf</a> 搜索，看别人是怎么取名的。

<strong>Bad: </strong>
```ruby
getUserInfo();
getClientData();
getCustomerRecord();
```
<strong>Good: </strong>
```ruby
getUser()
```
<a href="#back-to-top">↑回到顶部</a>

### 每个常量都该命名
可以用 <a target="_blank" href="https://link.juejin.im?target=https%3A%2F%2Fgithub.com%2Fdanielstjules%2Fbuddy.js" rel="nofollow noopener noreferrer">buddy.js</a> 或者 <a target="_blank" href="https://link.juejin.im?target=https%3A%2F%2Fgithub.com%2Feslint%2Feslint%2Fblob%2F660e0918933e6e7fede26bc675a0763a6b357c94%2Fdocs%2Frules%2Fno-magic-numbers.md" rel="nofollow noopener noreferrer">ESLint</a> 检测代码中未命名的常量。

<strong>Bad: </strong>
```ruby
# 三个月之后你还能知道 86400000 是什么吗?
setTimeout(blastOff, 86400000);
```
<strong>Good: </strong>
```ruby
const MILLISECOND_IN_A_DAY = 86400000;

setTimeout(blastOff, MILLISECOND_IN_A_DAY);
```
<a href="#back-to-top">↑回到顶部</a>

### 可描述
