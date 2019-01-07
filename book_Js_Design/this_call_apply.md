# 《JavaScript设计模式与开发实践》基础篇（1）—— this、call 和 apply

关于书籍《JavaScript设计模式与开发实践》的一些学习总结，本章梳理关于this、call和apply的知识，作为设计模式的基础知识的第一部分

## this

### JavaScript 的 this 总是指向一个对象，而具体指向哪个对象是在运行时基于函数的执行环境动态绑定的，而非函数被声明时的环境

## this的指向

基本可以分成以下四种情况
1. 作为对象的方法调用
2. 作为普通函数调用
3. 构造器调用
4. Function.prototype.call 或 Function.prototype.apply 调用

### 当函数作为对象的方法被调用时，this 指向该对象

```javascript {.class1 .class .line-numbers}
var obj = { 
    a: 1,
    getA: function(){
        alert ( this === obj ); // 输出:true alert ( this.a ); // 输出: 1
    }
};

obj.getA();
```

### 当函数不作为对象的属性被调用时，也就是我们常说的普通函数方式,此时的 this 总是指向全局对象。在浏览器的 JavaScript 里，这个全局对象是 window 对象。

```javascript {.class1 .class .line-numbers}
window.name = 'globalName';
var myObject = { 
    name: 'sven',
    getName: function(){ 
        return this.name;
    } 
};
var getName = myObject.getName; 
console.log( getName() ); // globalName
```

### 构造器调用

1. 当用 new 运算符调用函数时，该函数总 会返回一个对象，通常情况下，构造器里的 this 就指向返回的这个对象

```javascript {.class1 .class .line-numbers}
var MyClass = function(){ 
    this.name = 'sven';
};
var obj = new MyClass();
alert ( obj.name );    // 输出:sven
```

2. 如果构造器显式地返回了一个 object 类型的对象，那么此次运算结果最终会返回这个对象，而不是我们之前期待的 this

```javascript {.class1 .class .line-numbers}
var MyClass = function(){
    this.name = 'sven';
    return { // 显式地返回一个对象
        name: 'anne' 
    }
};
var obj = new MyClass();
alert ( obj.name ); // 输出:anne
```

### 跟普通的函数调用相比，用 Function.prototype.call 或Function.prototype.apply 可以动态地 改变传入函数的 this

```javascript {.class1 .class .line-numbers}
var obj1 = { 
    name: 'sven',
    getName: function(){
         return this.name;
    } 
};
var obj2 = { 
    name: 'anne'
};
console.log( obj1.getName() );// 输出: sven
console.log( obj1.getName.call( obj2 ) ); // 输出:anne
```

## call 和 apply

### Function.prototype.call 和 Function.prototype.apply 都是非常常用的方法。它们的作用一模一样，区别仅在于传入参数形式的不同

<table>
<thead>
<tr>
<th>方法</th>
<th>apply</th>
<th>call</th>
</tr>
</thead>
<tbody>
<tr>
<td>第一个参数</td>
<td>函数体内 this 对象的指向</td>
<td>函数体内 this 对象的指向</td>
</tr>
<tr>
<td>第二个参数</td>
<td>带下标的集合</td>
<td>从第二个参数开始，每个参数被依次传入函数</td>
</tr>
</tbody>
</table>

### apply

```javascript {.class1 .class .line-numbers}
var func = function( a, b, c ){
     alert ( [ a, b, c ] ); // 输出 [ 1, 2, 3 ]
};
func.apply( null, [ 1, 2, 3 ] );
```

### call 

```javascript {.class1 .class .line-numbers}
var func = function( a, b, c ){
     alert ( [ a, b, c ] ); // 输出 [ 1, 2, 3 ]
};
func.call( null, 1, 2, 3 );
```

## call和apply的用途

1. 改变 this 指向
2. 借用其他对象的方法

### 改变 this 指向

```javascript {.class1 .class .line-numbers}
var obj1 = { 
    name: 'sven'
};
var obj2 = { 
    name: 'anne'
};
window.name = 'window';
var getName = function(){ 
    alert ( this.name );
};
getName();     // 输出：window
getName.call( obj1 ); // 输出: sven
getName.call( obj2 ); // 输出: anne
```

### 借用其他对象的方法

```javascript {.class1 .class .line-numbers}
(function(){
    Array.prototype.push.call( arguments, 3 ); 
    console.log ( arguments ); // 输出[1,2,3]
})( 1, 2 );
```

### 回到目录
[《JavaScript设计模式与开发实践》最全知识点汇总大全]