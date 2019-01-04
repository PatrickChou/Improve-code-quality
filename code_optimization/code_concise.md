#### 我们将从几个方面展开讨论：
1. <a href="#variables">变量</a>
2. 函数
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

