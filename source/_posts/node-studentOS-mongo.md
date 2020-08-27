---
title: Node学生管理系统（使用MongoDB -> mongoose）
date: 2020-05-24 21:25:01
tags: node	
categories: node
---

<div class='post-summary'>
    <p>
        在node开发中推荐使用第三方模块mongoose来使用MongoDB；这个项目优化了上一个同样的学生管理系统，减少了代码量，结构也更清晰，针对数据库的操作也独自封装成模块，不同的操作有不同的方法；
    </p>
    <p>
        在封装针对数据库操作的模块的时候，里面方法中也涉及到了回调函数，多次使用使我对回调函数有了更多的理解，也能熟练封装和调用回调函数了。
    </p>
</div>

<!--more-->

这个系统内没有上一个系统的`Action.js`，而是单独有一个针对`mongoDB`的操作模块`db.js`

```javascript
// 引入 mongoose 
const mongoose = require('mongoose')
// 链接数据库，加入参数
mongoose.connect('mongodb://localhost:27017/students', { useNewUrlParser: true ,useUnifiedTopology: true})
// 创建 Schema 图表
const Schema = mongoose.Schema
// 创建 Student 图表并且发布为模型
const Student = mongoose.model('Student', new Schema({
    name: {
        type: String,
        required: true
    },
    sex: {
        type: String,
        //枚举，只能是男或者女
        enum: ['男', '女'],
        default: '男'
    },
    age: {
        type: Number,
        required: true
    },
    hobby: {
        type: String,
        required: true
    }
}))
/**
 * 查找数据库中所有数据，用回调函数 handleData 来处理得到的数据
 */
exports.find = (handleData) => {
    Student.find((err, data) => {
        if(err) return console.log(err);
        handleData(data)
    })
}
/**
 * 保存添加的新数据
 */
exports.save = (newData, callback, handleShow) => {
    new Student(newData).save((err, ret) => {
        if(err) return callback(err)
        handleShow(ret)
    })
}
/**
 * 通过ID查找数据
 */
exports.findById = (id, handleError, handleData) => {
    Student.findById(id, (err, data) => {
        if(err) return handleError(err)
        handleData(data)
    })
}
/**
 * 通过 Id 删除一条数据
 */
exports.delete = (id, handleError) => {
    Student.deleteOne({_id: id}, (err) => {
        if(err) handleError(err)
    })
}
/**
 * 通过 id 来修改查找到的数据
 */
exports.modify = (id, newData, callback) => {
    Student.findByIdAndUpdate(id, newData, (err) => {
        if(err) return callback(err)
    })
}
```

同时，路由模块`router.js`也改变了

```javascript
const express = require('express')
const router = express()
// 导入自己封装的mongodb数据库操作模块
const db = require('./db')

router.get('/students', (req, res) => {
    db.find((data) => {
        let students = data
        res.render('index.html', {
            students: students
        })
    })
})

router.get('/students/new', (req, res) => {
    res.render('add_student.html')
})
    
router.post('/students/new', (req, res) => {
    // 使用 body-parser 获取的请求数据给student
    let student = req.body
    
    // 调用封装的db里的save方法， 传入填入的数据进行保存
    db.save(student, (err) => {
        console.log('Something Wrong' + err);
    }, (data) => {
    })
    // 重定向到首页 
    res.redirect('/students')
})

router.get('/students/edit', (req, res) => {
    // 直接通过body-parser获取的id有引号，用replace消除
    let id = req.query.id.replace('"', '').replace('"', '')
    // 根据 id 查找到数据，然后传递到渲染的编辑页面
    db.findById(id, (err) => {
        return console.log(err);
    }, (data) => {
        let student = data
        res.render('edit_student.html', {
            student: student
        })
    })
})

router.post('/students/edit', (req, res) => {
    // 获取更新的数据内容
    let student = req.body
    // 获取 id
    let id = req.body.id
    // 调用封装的db数据库操作里面的modify方法
    // 第一个参数为 要修改的对象的id，第二个为要修改后的数据，有错误就用回调函数处理
    db.modify(id, student, (err) => {
        console.log("Update Error!");
    })
    // 重定向到首页
    res.redirect('/students')
})

router.get('/students/delete', (req, res) => {
    // 直接通过body-parser获取的id有引号，用replace消除
    let id = req.query.id.replace('"', '').replace('"', '')
    // 使用封装的db里的delete
    db.delete(id, (err) => {
        return console.log('Delete Error!');
    })
    res.redirect('/students')
})

module.exports = router
```



<style>
    .post-summary{
        display: none;
    }
</style>

