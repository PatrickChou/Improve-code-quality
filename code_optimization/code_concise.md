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

### 删除重复代码
很多时候虽然是同一个功能，但由于一两个不同点，让你不得不写两个几乎相同的函数。

要想优化重复代码需要有较强的抽象能力，错误的抽象还不如重复代码。所以在抽象过程中必须要遵循 <code>SOLID</code> 原则（<code>SOLID</code> 是什么？稍后会详细介绍）。

<strong>Bad: </strong>
```javascript {.class1 .class .line-numbers}
function showDeveloperList(developers) {
  developers.forEach((developer) => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();
    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers) {
  managers.forEach((manager) => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```
<strong>Good: </strong>
```javascript {.class1 .class .line-numbers}
function showEmployeeList(employees) {
  employees.forEach(employee => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();
    const data = {
      expectedSalary,
      experience,
    };
    
    switch(employee.type) {
      case 'develop':
        data.githubLink = employee.getGithubLink();
        break
      case 'manager':
        data.portfolio = employee.getMBAProjects();
        break
    }
    render(data);
  })
}
```
<a href="#back-to-top">↑回到顶部</a>

### 对象设置默认属性

<strong>Bad: </strong>
```javascript {.class1 .class .line-numbers}
const menuConfig = {
  title: null,
  body: 'Bar',
  buttonText: null,
  cancellable: true
};

function createMenu(config) {
  config.title = config.title || 'Foo';
  config.body = config.body || 'Bar';
  config.buttonText = config.buttonText || 'Baz';
  config.cancellable = config.cancellable !== undefined ? config.cancellable : true;
}

createMenu(menuConfig);
```
<strong>Good: </strong>
```javascript {.class1 .class .line-numbers}
const menuConfig = {
  title: 'Order',
  // 'body' key 缺失
  buttonText: 'Send',
  cancellable: true
};

function createMenu(config) {
  config = Object.assign({
    title: 'Foo',
    body: 'Bar',
    buttonText: 'Baz',
    cancellable: true
  }, config);

  // config 就变成了: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```
<a href="#back-to-top">↑回到顶部</a>

### 不要传 flag 参数
通过 flag 的 true 或 false，来判断执行逻辑，违反了一个函数干一件事的原则。

<strong>Bad: </strong>
```javascript {.class1 .class .line-numbers}
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```
<strong>Good: </strong>
```javascript {.class1 .class .line-numbers}
function createFile(name) {
  fs.create(name);
}
function createFileTemplate(name) {
  createFile(`./temp/${name}`)
}
```
<a href="#back-to-top">↑回到顶部</a>

### 避免副作用（第一部分）
函数接收一个值返回一个新值，除此之外的行为我们都称之为副作用，比如修改全局变量、对文件进行 IO 操作等。

当函数确实需要副作用时，比如对文件进行 IO 操作时，请不要用多个函数/类进行文件操作，有且仅用一个函数/类来处理。也就是说副作用需要在唯一的地方处理。

副作用的三大天坑：随意修改可变数据类型、随意分享没有数据结构的状态、没有在统一地方处理副作用。

<strong>Bad: </strong>
```javascript {.class1 .class .line-numbers}
// 全局变量被一个函数引用
// 现在这个变量从字符串变成了数组，如果有其他的函数引用，会发生无法预见的错误。
var name = 'Ryan McDermott';

function splitIntoFirstAndLastName() {
  name = name.split(' ');
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```
<strong>Good: </strong>
```javascript {.class1 .class .line-numbers}
var name = 'Ryan McDermott';
var newName = splitIntoFirstAndLastName(name)

function splitIntoFirstAndLastName(name) {
  return name.split(' ');
}

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```
<a href="#back-to-top">↑回到顶部</a>

### 避免副作用（第二部分）

在 JavaScript 中，基本类型通过赋值传递，对象和数组通过引用传递。以引用传递为例：

<p>假如我们写一个购物车，通过 <code>addItemToCart()</code> 方法添加商品到购物车，修改 <code>购物车数组</code>。此时调用 <code>purchase()</code> 方法购买，由于引用传递，获取的 <code>购物车数组</code> 正好是最新的数据。</p>

看起来没问题对不对？

<p>如果当用户点击购买时，网络出现故障， <code>purchase()</code> 方法一直在重复调用，与此同时用户又添加了新的商品，这时网络又恢复了。那么 <code>purchase()</code> 方法获取到 <code>购物车数组</code> 就是错误的。</p>

<p>为了避免这种问题，我们需要在每次新增商品时，克隆 <code>购物车数组</code> 并返回新的数组。</p>

<strong>Bad: </strong>
```javascript {.class1 .class .line-numbers}
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```
<strong>Good: </strong>
```javascript {.class1 .class .line-numbers}
const addItemToCart = (cart, item) => {
  return [...cart, {item, date: Date.now()}]
};
```
<a href="#back-to-top">↑回到顶部</a>

### 不要写全局方法
<p>在 JavaScript 中，永远不要污染全局，会在生产环境中产生难以预料的 bug。举个例子，比如你在 <code>Array.prototype</code> 上新增一个 <code>diff</code> 方法来判断两个数组的不同。而你同事也打算做类似的事情，不过他的 <code>diff</code> 方法是用来判断两个数组首位元素的不同。很明显你们方法会产生冲突，遇到这类问题我们可以用 ES2015/ES6 的语法来对 <code>Array</code> 进行扩展。</p>

<strong>Bad: </strong>
```javascript {.class1 .class .line-numbers}
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```
<strong>Good: </strong>
```javascript {.class1 .class .line-numbers}
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));        
  }
}
```
<a href="#back-to-top">↑回到顶部</a>

### 比起命令式我更喜欢函数式编程
函数式变编程可以让代码的逻辑更清晰更优雅，方便测试。

<strong>Bad: </strong>
```javascript {.class1 .class .line-numbers}
const programmerOutput = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  }, {
    name: 'Suzie Q',
    linesOfCode: 1500
  }, {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  }, {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

let totalOutput = 0;

for (let i = 0; i < programmerOutput.length; i++) {
  totalOutput += programmerOutput[i].linesOfCode;
}
```
<strong>Good: </strong>
```javascript {.class1 .class .line-numbers}
const programmerOutput = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  }, {
    name: 'Suzie Q',
    linesOfCode: 1500
  }, {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  }, {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];
let totalOutput = programmerOutput
  .map(output => output.linesOfCode)
  .reduce((totalLines, lines) => totalLines + lines, 0)
```
