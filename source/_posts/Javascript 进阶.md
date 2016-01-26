---
title: Javascript 进阶
date: 2016-01-26 12:53:00
tags:
- Javascript
---
## Head object/Global object

- Head object只有一个，即window
- Global object为全局变量（函数也是变量），它们的上级是Head object

<pre>
var name = 'jason chuh';
var title = 'Mr.';
var walk = function(){
    console.log('我在走路...');
};
console.log('name' in window);
console.log('title' in window);
console.log('walk' in window);

console.log(window['name']);
console.log(window['title']);
console.log(window['walk']);
</pre>

### var 声明

- 在函数内未使用var声明变量，会自动提升为global object

<pre>
var func1 = function(){
    withoutVar = '我是全局的?';
};
func1();
console.log(withoutVar);
</pre>

## [] 与 dot

- 对象的属性与方法，同时可以使用[]或.的方式来调用

<pre>
var aSong = {
    title: '江南Style',
    singer: 'PSV',
    duration: '6:43',
    summary: function(prefix){
        return prefix.concat(this.title).concat(this.singer).concat(this.duration);
    }
};
console.log(aSong.title);
console.log(aSong['title']);
console.log(aSong['summary']('MTV:'));
</pre>

## 函数

- 创建函数的3种方法

<pre>
function sum(x, y){ return x+y; }
var sum = function(x, y){ return x+y; }; // var sum = function sum(x,y){ return x+y; };
var sum = new Function('x','y', 'return x+y');
console.log(sum(3,4));
</pre>

### 匿名函数

- 用于临时函数，一次性函数(定义后马上运行)
- 匿名函数的循环调用

<pre>
var list = [
  {name: 'jerry', age: 23},
  {name: 'bill', age: 3},
  {name: 'tom', age: 42},
  {name: 'jason', age: 8}
];

var sortedList = list.sort(function(a,b){
  return a.age - b.age; // 为什么不使用 a.age === b.age

  // 姓名如何排序？
  // retun a.name > b.name;

  // 多个字段如何排序？
  // return (a.age - b.age) + (a.name > b.name);
});
console.log(sortedList);

// Count Down

(function(num){
    if(num &lt; 0) return;

    console.log(num);
    num--;
    arguments.callee(num);
})(5);

</pre>

### 自动运行

<pre>
(function sum(x, y){})(5,6); // 常用
(function sum(x, y){}(5,6));
!function sum(x, y){}(5,6);
</pre>

### 作用域

- 作用域是在定义时已经确定，非运行时得到
- 只存在函数作用域，不存在块作用域

<pre>
var myName = 'Mac';
function tellMeName(){
    var myName = 'PC';
    console.log(myName);
}

console.log(myName);

if(true){
    myName = 'Pad';
}
console.log(myName);


var countDownFrom = function(num){
    console.log(num);
    num--;
    if(num &lt; 0)
        return false;
    
    arguments.callee(num);
};

countDownFrom(5);
</pre>

### arguments

- 函数的可变长参数
- 使用arguments.callee得到函数自身

<pre>
var sum = function(){
  var len = arguments.length;
  if(0 >= len){
    return 0;
  }else if(len &lt; 2){
    return arguments[0];
  } else {
    var total = 0;
    for(var i = 0; i &lt; len; i++){
      total += arguments[i];
    }
    return total;
  }
};

console.log(sum());
console.log(sum(2));
console.log(sum(2,3,4,5));
</pre>

## this对象

- 运行时确定，可以被更改
- 针对实例的引用
- window是顶层的this

<pre>
console.log(window === this);
var weight = 1.23;
console.log(weight, window.weight, this.weight);

var playGame = function(){};
console.log(playGame, window.playGame, this.playGame);

// 高阶函数

var callOtherFunction = function(otherFunc){
    var testVariable = 123;
    otherFunc();
    console.log('调用结束');
};

var testVariable = 999;
callOtherFunction(function(){
    console.log(testVariable);
});
</pre>

### call/apply

- call与apply用于调用函数，同时改变this的定义
- call与apply不同在于参数传递方式不同

<pre>
var Phone = function(){
    this.myName = 'Tim';
    this.phoneNumber = '13888888888';
    this.makeAPhoneCall = function(toName, toPhone){
        console.log(this.myName+'('+this.phoneNumber+')打给'+toName+'('+toPhone+')');
    };
};

var myPhone = new Phone();
myPhone.makeAPhoneCall('Bill', '020 12345678');

var mike = {myName:'Mike', phoneNumber:'18999999999'};
myPhone.makeAPhoneCall.call(mike, 'Jerry', '10086');
myPhone.makeAPhoneCall.apply(mike, ['Jerry', '10086']);
</pre>

## prototype

- 对象化的实现方案
- prototype chain原型键, instance.property -> (class of instance).prototype.property -> superclass(class of instance).prototype.property -> ... -> object.prototype
- instance.construct.prototype == class.prototype

<pre>
var Fruit = function(){
    this.name = 'Fruit';
    this.tellMyName = function(){
        console.log('My name is ' + this.name);
    };
};
Fruit.prototype.color = 'Green';
Fruit.prototype.season = 'Autumn';

var Orange = function(){
    this.name = 'Orange';
};
Orange.prototype = new Fruit();
Fruit.prototype.season = 'Summer';
Fruit.prototype.taste = 'Sweet';

var newFruit = new Orange();
newFruit.taste = 'Acid';

newFruit.tellMyName();
console.log(newFruit.color);
console.log(newFruit.season);
console.log(newFruit.taste);
</pre>

## String

- concat与Array.join的效率

<pre>
// #1 使用+连接
var stringWithPlus = '';
for (var i = 0; i &lt; 1000000; i++) {
    stringWithPlus += 'hello ';
}

// #2 使用concat方法
var stringWithConcat = '';
for (var i = 0; i &lt; 1000000; i++) {
    stringWithConcat.concat('hello ');
}

// #3 使用Array.join
var stringBuffer = [];
for (var i = 0; i &lt; 1000000; i++) {
    stringBuffer.push('hello');
}
var stringByBuffer = stringBuffer.join(' ');

// #4 使用underscore模板
var template = "<% _.each(_.range(1000000), function(){ %><%=name%> <% }); %>";
var compiled = _.template(template);
console.log(compiled({name: 'hello'}));

// #5 使用定时器执行, 支持亿次级运算，不会阻止主进程
function concatString(count, str, callback){
    var stringPool = [];
    setTimeout(function(){
        stringPool.push(str);
        count--;
        if(count > 0){
            setTimeout(arguments.callee, 0.001);
        } else {
            callback(stringPool);
        }
    }, 0.001);
}

concatString(100000000, 'hello', function(stringPool){
    var combined = stringPool.join(" ");
    console.log(combined);
});
</pre>

## null 与 undefined

- undefined 代表没有初始化，null可用于表示空值状态
- 不应人工赋予undefined值

<pre>
var uninitVariable;
console.log(uninitVariable === undefined);
uninitVariable = 0;
console.log(uninitVariable === undefined);
</pre>

## Boolean

- 包括空格的字符串转换为Boolean对象，其值为true（所有类型转换为Boolean对象，其值为true）
- Boolean对象实例始终为true，不要new 方法创建Boolean实例
- false, 0, '', "", NaN, undefined, null为false，其它为true

<pre>
console.log(Boolean(new Boolean(false)));
console.log(Boolean(new Boolean('')));

console.log(Boolean(false));
console.log(Boolean(0));
console.log(Boolean(''));
console.log(Boolean(""));
console.log(Boolean(NaN));
console.log(Boolean(undefined));
console.log(Boolean(null));
</pre>