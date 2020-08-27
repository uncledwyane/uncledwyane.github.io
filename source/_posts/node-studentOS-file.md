---
title: Node 学生管理系统（Express + 读写JSON文件）
date: 2020-05-20 21:02:29
tags: node
categories: node
---

<div class='notification is-primary post-summary'>
    <p>基于上一个node极简评论系统做的一个具有增删改的小型学生管理系统,这次使用到了Express，使用js的模块化编程将路由和增删改操作分别抽离成模块再使用。</p>
</div>
<ul class='post-summary'>
        <li>这次使用到的知识点有：</li>
        <li>express</li>
        <li>body-parser</li>
        <li>art-template</li>
</ul>

总结：这个demo里面涉及到了回调函数，其实在之前的vue项目中使用axios请求服务器数据，处理服务器返回的数据也是回调函数，当时只是知道是回调函数，不知道或者不理解到底是什么；这个项目多次使用到了封装的回调函数，让自己对这个有了初步的理解；这个项目中也有针对增、删、改的操作而封装的模块，也让自己对模块有了更深的理解。

还存在的问题：虽然封装了增、删、改的操作，减少了代码的重复率，但是封装不彻底，仍有部分代码是重复的，后续熟练使用回调函数的时候再进行进一步优化。

<!--more-->

<div align='center'>
    <img src='https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/crud.gif' alt='效果图'/>
    <br>
    <img src='https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/trees.jpg' alt='项目结构'/>
</div>


### 路由模块 router.js

```javascript
const express = require('express')
const router = express()
const fs = require('fs')
const Action = require('./action')

router.get('/students', (req, res) => {
    fs.readFile('./students.json', (err, data) => {
        let students = JSON.parse(data).students
        if(err)
            return this.response.status(500).send('oops! Something Crashed!!!')
        
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
    // 使用封装的保存学生的 save 方法，两个参数，一个是学生实参，一个为错误，错误用用回调函数处理
    Action.save(student, (err) => {
        if(err)
            res.status(500).send('Error')
        // 重定向到首页
        res.redirect('/students')
    })
})

router.get('/students/edit', (req, res) => {
    let reqId = parseInt(req.query.id)
    Action.findById(reqId, (err, student) => {
        if(err)
            return res.status(500).send('Oops! Something Error')
        res.render('edit_student.html', {
            student: student
        })
    })
})

router.post('/students/edit', (req, res) => {
    let student = req.body
    Action.editById(student, (err) => {
        if(err)
            return res.status(500).send('Oops! Something Error')
        res.redirect('/students')
    })
})

router.get('/students/delete', (req, res) => {
    let id = parseInt(req.query.id)
    Action.deleteById(id, (err) => {
        if(err)
            return res.status(500).send('Oops! Something Error')
        res.redirect('/students')
    })
})
module.exports = router
```

### 读写文件模块 action.js

```javascript
const fs = require('fs')
const db_url = './students.json'

/**
 * 保存添加学生数据到json文件里
 * 封装save方法，使用回调函数处理错误
 * 两个参数 student为传入的新增学生对象
 * callback为回调函数，处理错误
 */
exports.save = (student,callback) => {
    // 读取文件内容
    fs.readFile(db_url, 'utf8', (err, data) => {
        // 如果有错误，使用回调函数处理     
        if(err)
            return callback(err)
        // 没有错误就把获取的数据转换成字符串并赋值给students
        let students = JSON.parse(data).students
        if(students.length === 0){
            student.id = 1
            students.push(student)
            let newStudents = JSON.stringify({
                students: students
            })
            // 写入文件，如果有错，就是用回调函数处理
            fs.writeFile(db_url, newStudents, (err) => {
                // 如果有错误，使用回调函数处理
                if(err)
                    return callback(err)
                // 没有错误，使回调函数参数为空
                callback(null)
            })
            return
        }
        // 添加 id 
        student.id = students[students.length - 1].id + 1
        // 将新增的学生添加到从文件获取的数据中
        students.push(student)
        // 将 包含新增学生的数据格式化为JSON格式，并传递给newStudents
        let newStudents = JSON.stringify({
            students: students
        })
        // 写入文件，如果有错，就是用回调函数处理
        fs.writeFile(db_url, newStudents, (err) => {
            // 如果有错误，使用回调函数处理
            if(err)
                return callback(err)
            // 没有错误，使回调函数参数为空
            callback(null)
        })
    })
}

/**
 * 保存编辑学生方法
 * 使用 学生id 为关键字进行编辑
 */

exports.editById = (student, callback) => {
    fs.readFile('./students.json', (err, data) => {
        if(err) 
            return callback(err)
        let students = JSON.parse(data).students

        student.id = parseInt(student.id)

        let stu = students.find((item) => {
            return item.id === student.id
        })
        
        for(let key in student){
            stu[key] = student[key]
        }
        // 将 包含新增学生的数据格式化为JSON格式，并传递给newStudents
        let newStudents = JSON.stringify({
            students: students
        })
        // 写入文件，如果有错，就是用回调函数处理
        fs.writeFile(db_url, newStudents, (err) => {
            // 如果有错误，使用回调函数处理
            if(err)
                return callback(err)
            // 没有错误，使回调函数参数为空
            callback(null)
        })
    })
}
/**
 * 根据 id 查询学生信息
 */
exports.findById = (id, callback) => {
    fs.readFile('./students.json', (err, data) => {
        if(err)
            return console.log('编辑失败');
        let students = JSON.parse(data).students
        let student = students.find((item) => {
            return item.id === parseInt(id)
        })
        callback(null, student)
    })
}

/**
 * 根据 id 删除
 */

exports.deleteById = (id, callback) => {
    fs.readFile('./students.json', (err, data) => {
        if(err) 
            return callback(err)
        let students = JSON.parse(data).students

        let index = students.findIndex((item) => {
            return item.id === id
        })
        
        students.splice(index, 1)

        // 将 包含新增学生的数据格式化为JSON格式，并传递给newStudents
        let newStudents = JSON.stringify({
            students: students
        })
        // 写入文件，如果有错，就是用回调函数处理
        fs.writeFile(db_url, newStudents, (err) => {
            // 如果有错误，使用回调函数处理
            if(err)
                return callback(err)
            // 没有错误，使回调函数参数为空
            callback(null)
        })
    })
}
```




<style>
    .post-summary{
        display:none;
    }
</style>

