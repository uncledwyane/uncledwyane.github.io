---
title: es6的一些改变
thumbnail: https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/es6.png
date: 2020-04-28 13:48:40
tags: JavaScript
toc: true
categories: JavaScript
---

<div class='post-summary'>
    <p>
       	从表老师<a href='https://biaoyansu.com'>@表严肃</a>那里看了ES6精讲，做个学习笔记！
    </p>
</div>




<!--more-->

### let

#### 语法

```javascript
let a = 1; // 定义一个变量并赋值为1
```

因为和`var`类似但又有不同，用例子来比较：

```javascript
// 第一个例子
if(true){
    var a = 1;
    let b = 2;
}
console.log('a:' + a);
console.log('b:' + b);
// 这里a能够输出1，但是b会报错，显示未定义

// 第二个例子
if(true){
    let b = 2;
    console.log('b:' + b); // 这个b能够正常输出
}
console.log('b:' + b); // 不能正常输出，显示未定义

// 第三个例子
for(var i = 0; i < 5; i++){ // 使用 var 定义
    console.log(i); 
} // 这里会输出 0 1 2 3 4
console.log(i); // 到这里会输出 0 1 2 3 4 5

for(let i = 0; i < 5; i++){ // 使用 let 定义
    console.log(i); 
} // 这里会输出 0 1 2 3 4
console.log(i); // 到这里会报错,显示这个i未定义
```

<div class='notification is-info'>
    与var相比较而言，let定义的变量更安全，更严谨，只在自己的作用域下能访问。
</div>

### const

#### 语法

```javascript
const LOVE_YOU = true; // 声明一个常量，不能被更改
-------------------------------------------
// 例：如果声明的是一个对象，则可以更改对象里面的属性
var user = {
    name: 'James',
    age: 35
};
const PLAYER = user;
console.log('age:' + user.age); //  输出age：35
user.age = 40;
console.log('age:' + user.age); // 输出age：40
```

<div class='notification is-danger'>
    <p>
        const 是 constant 的缩写，常量的意思，不能更改的量。
    </p>
    <p>
        在用const声明一个常量时，必须同时给它赋值。
    </p>
    <p>
        不能直接修改const的值，如果const被赋值为一个对象，则可以修改对象里面的属性；
       	在用对象给const赋值之后，就不能用另一个对象来覆盖const已经被赋予的对象，即不能修改。
    </p>
</div>

### 变量的解构赋值（数组）

#### 赋值

在es6之前如果要赋值几个值，写法为：

```javascript
var a = 1;
var b = 2;
var c = 3;
// 或者
var a = 1, b = 2, c = 3;
```

在es6中，可以写为：

```javascript
var [a, b, c] = [1, 2, 3];
```

其他：

```javascript
var [a, , c] = [1, 2, 3];
console.log(a);
console.log(c);
console.log(b);
// 这里输出b会报错，神奇的是，虽然b没有定义，但是a和c可以正常输出。这就是另一个特点：跨越传值
//-------------------------
var [a, ...c] = [1, 2, 3];
console.log(a); // 输出 1
console.log(c); // 输出 [2, 3]
//-------------------------
var [a, b, c='default', d='default'] = [1, 2, 3];
console.log(a); // 输出 1
console.log(b); // 输出 2
console.log(c); // 输出 3
console.log(d); // 输出 default
```

#### 数组的内容赋值给变量

##### 在es6之前想要把数组的内容赋值给变量，则需要：

```javascript
var arr = [1, 2, 3];
var a = arr[0];
var b = arr[1];
var c = arr[2];
if(arr[3])
    var d = arr[3];
else
    var d = 'default';
```

##### 用es6只需要：

```javascript
var arr = [1, 2, 3];
var [a, b, c, d='default'] = arr;
```

#### 在es6中给变量用数组内容赋值

例如下面代码所示，如果右边没有对应的内容，则就是没有，不会赋值为undefined。

```javascript
let [a, b, c] = [1, 2]; // 这里右边的数组里没有对应c的值，则c为null，不会把c定义为undefined,更严谨
```

<div class='notification is-warning'>
    <p>
        用数组赋值时，索引很重要，要一一对应。
    </p>
</div>



### 变量的解构赋值（对象）

#### 使用对象赋值：

```javascript
var obj = {
    a: 1,
    b: 2
}
let {a, b} = obj; // 这样就把obj里面的a, b分别赋值给了外面的a，b，a为1，b为2，一一对应。
let {c, b} = obj; // 这样c会是undefined，b为2，obj中没有c的属性

```

#### 赋值之后改为其他名称

如果想把赋值之后的变量改为其他名称，使用如下方法：

```javascript
let {a:A, b} = obj; // 这样A=1，b=2
```

#### 更复杂的解构（很少，较复杂，可做了解）：

```javascript
var obj = {
    arr: [
        'Yo.',
        {
            a: 1
        }
    ]
}
let {arr:[say, {a}]} = obj;
```

#### 指定默认值：

```javascript
let {a:A=1, b=2} = {a:10};
```

#### 在实际中的使用：例如请求api的数据：

```javascript
// 自定义一个服务器返回的数据
var res = {
    status: 200,
    id: 'user',
    data: [{name: 'wade', age: 33}, {name: 'james', age: 35}]
}
// es6之前的解析数据的方式
var status = res.status;
var data = res.data;

// es6
let {status, data} = res;
```

解构含有方法的对象，用JavaScript里的Math这个数学对象来举例，Math里面有很多方法，这里用其中的乘方pow方法来举例：

```javascript
let {pow} = Math;
console.log('pow(2, 3):' + pow(2, 3)); // 输出：pow(2, 3): 8, 直接使用，非常强大
```

### 变量的解构赋值（其他）

#### 直接得出字符串长度：

```javascript
let {length} = 'Hello World';
console.log('length:' + length);
```

#### 解构字符串为数组：

```javascript
let [a, b, c] = 'Ha.'; // a = H, b = a, c = .
```

### 新增字符串方法

#### 检测字符串中是否包含另一个字符串：

```javascript
// es6之前
console.log('Hello World'.indexOf('H') !== -1);// 说明不存在

// es6
console.log('Hello World'.includes('H')); // 返回true
```

#### 检测字符串中是否由一个字符串开头：

```javascript
console.log('Hello'.startsWith('H')); // 返回true
```

#### 检测字符串中是否由一个字符串结束：

```javascript
console.log('Hello'.endsWith('H')); // 返回true
```

#### 重复字符串：

```javascript
console.log('Hello'.repeat(3)); // 重复Hello这个字符串三次
```

### 模板字符串

#### 模板语法

```javascript
let title = 'Hello World'
let tpl = `
	<div>
		<span>${title}</span>
	</div>	
`;
// 模板中还可以嵌套模板或者再嵌套引用,例：
let tpl2 = `
	<div>
        <span>${title + `
            <span>${123}</span>
        `}</span>
	</div>	
`;
```

### Symbol类型（暂时不予深入）

#### JavaScript数据类型

+ Symbol
+ undefined
+ null
+ Boolean
+ String
+ Number
+ Object

#### Symbol

```javascript
let a = Symbol('对这个Symbol的解释')
```



### Proxy(代理)

#### 语法

```javascript
var 名称 = new Proxy(源对象, 配置项)
```

举例：

```javascript
var user = new Proxy({}, {
    get: function(obj, prop){
        if(prop == 'full_name')
            return obj.fname + ' ' + obj.lname;
    },
    set: function(obj, prop){
        
    }
});
user.fname = 'Dwyane';
user.lname = 'Wade';

console.log(user.full_name);
```



### Set

#### 定义

```javascript
var s = new Set([1, 2, 3, 4]);
```

#### 特点

<div class='notification is-success'>
    Set中的值唯一，如果有多个重复值，只取一个。
</div>

#### 方法

+ add  // 添加元素
+ detete  // 删除元素
+ has  // 是否有某个元素，返回Boolean型
+ clear  // 清空Set中所有内容

#### 其他

##### size

和数组的length类似，大小

<style>
    .post-summary{
        display: none;
    }
</style>
