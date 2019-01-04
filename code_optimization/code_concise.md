<a name="back-to-top"></a>
#### 我们将从几个方面展开讨论：
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

<a name="variables"></a>
# 变量
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
```javascript {.class1 .class .line-numbers}
// 三个月之后你还能知道 86400000 是什么吗?
setTimeout(blastOff, 86400000);
```
<strong>Good: </strong>
```javascript {.class1 .class .line-numbers}
const MILLISECOND_IN_A_DAY = 86400000;

setTimeout(blastOff, MILLISECOND_IN_A_DAY);
```
<a href="#back-to-top">↑回到顶部</a>

### 可描述
通过一个变量生成了一个新变量，也需要为这个新变量命名，也就是说每个变量当你看到他第一眼你就知道他是干什么的。

<strong>Bad: </strong>
```ruby
const ADDRESS = 'One Infinite Loop, Cupertino 95014';
const CITY_ZIP_CODE_REGEX = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(ADDRESS.match(CITY_ZIP_CODE_REGEX)[1], ADDRESS.match(CITY_ZIP_CODE_REGEX)[2]);
```
<strong>Good: </strong>
```ruby
const ADDRESS = 'One Infinite Loop, Cupertino 95014';
const CITY_ZIP_CODE_REGEX = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [, city, zipCode] = ADDRESS.match(CITY_ZIP_CODE_REGEX) || [];
saveCityZipCode(city, zipCode);
```
<a href="#back-to-top">↑回到顶部</a>

### 直接了当
<strong>Bad: </strong>
```javascript {.class1 .class .line-numbers}
const locations = ['Austin', 'New York', 'San Francisco'];
locations.forEach((l) => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // 需要看其他代码才能确定 'l' 是干什么的。
  dispatch(l);
});
```
<strong>Good: </strong>
```javascript {.class1 .class .line-numbers}
const locations = ['Austin', 'New York', 'San Francisco'];
locations.forEach((location) => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  dispatch(location);
});
```
<a href="#back-to-top">↑回到顶部</a>

### 避免无意义的前缀

如果创建了一个对象 car，就没有必要把它的颜色命名为 carColor。

<strong>Bad: </strong>
```javascript {.class1 .class .line-numbers}
const car = {
  carMake: 'Honda',
  carModel: 'Accord',
  carColor: 'Blue'
};

function paintCar(car) {
  car.carColor = 'Red';
}
```
<strong>Good: </strong>
```javascript {.class1 .class .line-numbers}
const car = {
  make: 'Honda',
  model: 'Accord',
  color: 'Blue'
};

function paintCar(car) {
  car.color = 'Red';
}
```
<a href="#back-to-top">↑回到顶部</a>

### 使用默认值
<strong>Bad: </strong>
```javascript {.class1 .class .line-numbers}
function createMicrobrewery(name) {
  const breweryName = name || 'Hipster Brew Co.';
  // ...
}
```
<strong>Good: </strong>
```javascript {.class1 .class .line-numbers}
function createMicrobrewery(name = 'Hipster Brew Co.') {
  // ...
}
```
<a href="#back-to-top">↑回到顶部</a>

<a name="function"></a>
# 函数

### 参数越少越好
如果参数超过两个，使用 <code>ES2015/ES6</code> 的解构语法，不用考虑参数的顺序。

<strong>Bad: </strong>
```javascript {.class1 .class .line-numbers}
function createMenu(title, body, buttonText, cancellable) {
  // ...
}
```
<strong>Good: </strong>
```javascript {.class1 .class .line-numbers}
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```
<a href="#back-to-top">↑回到顶部</a>

### 只做一件事情
这是一条在软件工程领域流传久远的规则。严格遵守这条规则会让你的代码可读性更好，也更容易重构。如果违反这个规则，那么代码会很难被测试或者重用。

<strong>Bad: </strong>
```javascript {.class1 .class .line-numbers}
function emailClients(clients) {
  clients.forEach((client) => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```
<strong>Good: </strong>
```javascript {.class1 .class .line-numbers}
function emailActiveClients(clients) {
  clients
    .filter(isActiveClient)
    .forEach(email);
}
function isActiveClient() {
  const clientRecord = database.lookup(client);    
  return clientRecord.isActive();
}
```
<a href="#back-to-top">↑回到顶部</a>

### 顾名思义
看函数名就应该知道它是干啥的。

<strong>Bad: </strong>
```javascript {.class1 .class .line-numbers}
function addToDate(date, month) {
  // ...
}

const date = new Date();

// 很难知道是把什么加到日期中
addToDate(date, 1);
```
<strong>Good: </strong>
```javascript {.class1 .class .line-numbers}
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```
<a href="#back-to-top">↑回到顶部</a>

### 只需要一层抽象层

如果函数嵌套过多会导致很难复用以及测试。

<strong>Bad: </strong>
```javascript {.class1 .class .line-numbers}
function parseBetterJSAlternative(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(' ');
  const tokens = [];
  REGEXES.forEach((REGEX) => {
    statements.forEach((statement) => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach((token) => {
    // lex...
  });

  ast.forEach((node) => {
    // parse...
  });
}
```
<strong>Good: </strong>
```javascript {.class1 .class .line-numbers}
function parseBetterJSAlternative(code) {
  const tokens = tokenize(code);
  const ast = lexer(tokens);
  ast.forEach((node) => {
    // parse...
  });
}

function tokenize(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(' ');
  const tokens = [];
  REGEXES.forEach((REGEX) => {
    statements.forEach((statement) => {
      tokens.push( /* ... */ );
    });
  });

  return tokens;
}

function lexer(tokens) {
  const ast = [];
  tokens.forEach((token) => {
    ast.push( /* ... */ );
  });

  return ast;
}
```
<a href="#back-to-top">↑回到顶部</a>
