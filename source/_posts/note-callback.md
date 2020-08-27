---
title: 对回调函数的一点理解
date: 2020-05-25 17:45:18
tags: 笔记
categories: 笔记
---

<div class='post-summary notification is-primary'>
    <p>
        在异步编程中，回调函数是非常常见的；在前面的帖子里，多处涉及到了回调函数，在网上搜索回调函数然后学习了一下，对回调函数有了一定的认识与理解，记录一下。
    </p>
</div>

如果很好的利用了回调函数，则会让工作思路变得很清晰，方便开发。

<!--more-->

### 举例

看一下这一段代码，这段代码里面三个控制台打印出1、2、3，但是2在异步的定时器里面，输出顺序为`1、3、2`，这就是异步，定时器里面的`2`不会在1输出之后输出，而是会在1、3输出一秒后输出：

```javascript
function test(){
    console.log(1)
    setTimeOut(function (){
        var data = 2
        console.log(data)
    }, 1000)
    console.log(3)
}
```

真正要讲的是，如何才能拿到定时器里面定义的`data`，它是一个局部变量，只作用在定时器内，在外部是访问不到的，这个时候就该回调函数登场，下面就是使用回调函数获取到定时器里面的data并打印输出：

```javascript
function test(callback){
    console.log(1);
    setTimeout(() => {
        var data = 2;
        callback(data)
    }, 1000);
    console.log(3);
}

test(function (data){
    console.log('拿到了：' + data);
})
```

这里的 test 方法中的***callback***就是定义的回调，作为参数传到test方法，使用这个回调去获取data；

在调用test方法的时候，传入的这个具体的带参方法就是回调函数的具体实现，回调的参数就是使用的时候的具体参数，这就拿到了data，可以对data做任何操作。

### 实际应用

在之前的mongodb的操作中，就频繁使用到了回调函数来处理错误和数据，例如下面定义的一个根据id查找数据的方法，传入三个参数，handleError 和 handleData就为传入的回调，使用这两个回调来分别处理错误和数据：

```javascript
exports.findById = (id, handleError, handleData) => {
    Student.findById(id, (err, data) => {
        if(err) return handleError(err)
        handleData(data)
    })
}
```

这是上面方法的具体使用，第一个参数就是实际的id，第二个是处理拿到的错误的函数，第三个就是处理拿到的数据的函数：

```javascript
db.findById(id, (err) => {
    return console.log(err);
}, (data) => {
    let student = data
    res.render('edit_student.html', {
        student: student
    })
})
```



<style>
    .post-summary{
        display: none;
    }
</style>

