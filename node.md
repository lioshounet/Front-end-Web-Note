# Node.js

## 基本信息

### Nodejs是什么

基于js的后端框架

- 可以写常见的接口

### Js前端和后端的学习模式

前端：Js基础语法+浏览器内置Api（Dom+Bom）+第三方库（jQ，element）

后端：Js基础语法+Node.js内置Api（fs，path，http等）+第三方库（express，mysql）

### 后端语言对比

| 语言 | Java   | Python | JavaScript |
| ---- | ------ | ------ | ---------- |
| 框架 | Spring | DJango | Nodejs     |

## 服务端之前的语法

### FS模块

FS模块属于node自有模块，可以一句话直接导入

导入

````javascript
//导入FS模块以用于操作文件
const fs = require('fs');
const txtPath = './Hw.txt';
````

正常的读取文件

- 回调参数（错误，读取内容）
- 这玩意异步

``````javascript
fs.readFile(txtPath, 'utf-8', function (err, dataStr) {
    if (!err) {
        console.log(dataStr);
    } else {
        console.log("读取失败");
    }
})
``````

正则提取案例

`````javascript
str1 = "ssss余肖江sssd"
zhengze = /ssss[\s\S]*sssd/

const TiQuArr = zhengze.exec(str1)
str2 = TiQuArr[0].replace('ssss', '').replace('sssd', '')

console.log(str2);
`````

路径动态拼接，防止因为运行路径错误导致的错误拼接

`````javascript
fs.readFile(__dirname + '/Hw.txt', 'utf-8', function (err, dataStr) {
    if (!err) {
        console.log(dataStr);
    } else {
        console.log("读取失败");
    }
})
`````

写入文件

- writeFile：覆盖写入
- join：追加

````javascript
var writeStr = 'something';
fs.writeFile(txtPath, writeStr, function (err) {
    if (err) {
        console.log(err);
    }
})
````

## HTTP服务端

HTTP模块属于node自有模块，可以一句话直接导入

`````js
const http = require('http')
`````

基本的开启服务和端口监听

```js
server.on('request', function (req, res) {
    //req是请求对象
    //res是响应对象

    var ConTest = ''
    const url = req.url
    const method = req.method
    res.setHeader('Content-Type', 'text/plain;charset=utf-8')

    ConTest = `检测到访问,这个url是${url},方法是${method}`

    res.end(ConTest);

})

server.listen(Port, function () {
    console.log("端口启动," + BaseUrl + ":" + String(Port));
})
```

### req

这是请求对象，可以获取请求相关的属性

- req.url

  端口号之后的字符串

- req.method

  请求的方法

### res

这是响应对象，控制输出的html字符串

核心语句

````js
res.end(ConTest);
````

这个东西在一个流程里面只能被触发一次

### 中文乱码

``````js
res.setHeader('Content-Type', 'text/plain;charset=utf-8')
``````

## npm

npm基本在学vue 的时候都学的擦不多了

这里补充几点

### init

- 最近的一层不要有特殊字符
- 添加packagejs

`````shell
npm init -y
`````

### -D和-S

- 生产环境和服务环境会直接影响依赖安装的选择

- -D安装的依赖不会被打包

- 这种设计的原因是，一些纯粹的工具和测试包

### nodemon开发小工具

作用是更新自动重启脚本

- 尽可能cmd和powershell都试一下

- 尝试翻墙

- 尝试管理员

- 尝试cnpm

- 问题解决小命令

  ``````shell
  set-ExecutionPolicy RemoteSigned
  ``````

  

## express模块

### 基本信息

- 第三方模块
- 本质就是一种对于http的封装

``````shell
npm i express
``````

### 正常使用

``````js
const express = require('express')
const app = express()

var Port = 80
var BaseUrl = 'http://127.0.0.1'

app.get('/user/:id', (req, res) => {
    res.send({
        name: 'Lio',
        age: '20',
        gender: 'man',
        id: req.params.id
    })
})

app.post('/user', (req, res) => {
    res.send('POST')
})

app.listen(Port, () => {
    console.log(`服务器启动成功,${BaseUrl}:${String(Port)}`);
})

``````

- 参数传递

通过  `/:变量` 这种格式传递

`req.params`  是变量的集合数组

### 静态资源挂载

```````js
//node http 封装的框架 是一个极简的服务器框架 本质是一个npm包

const express = require('express')

const app = express()

var Port = 80
var BaseUrl = 'http://127.0.0.1'

//静态资源托管
app.use('/html', express.static('./htmlEx'))

app.listen(Port, () => {
    console.log(`服务器启动成功,${BaseUrl}:${String(Port)}`);
})
```````

## router模块

由于express的路由有优化需求

所有才有的router

### 最基础的路由模块

`````js
// 这是路由模块
// 1. 导入 express
const express = require('express')
// 2. 创建路由对象
const router = express.Router()

// 3. 挂载具体的路由
router.get('/user/list', (req, res) => {
  res.send('Get user list.')
})
router.post('/user/add', (req, res) => {
  res.send('Add new user.')
})

// 4. 向外导出路由对象
module.exports = router
`````

## 中间件

### 基本信息

- 处理数据的基本模块
- 中间件串连成链条形成数据流
- `next()`是关键函数

### 全局中间件

``````js
const express = require('express')

const app = express()

var Port = 80
var BaseUrl = 'http://127.0.0.1'

var exUrl = ''

app.use((req, res, next) => {
    exUrl = 'show/' + req.url
    next()
})

app.use((req, res, next) => {
    exUrl = 'show/' + exUrl
    next()
})

app.get('/', (req, res) => {
    res.send(exUrl)
})

app.listen(Port, () => {
    console.log(`服务器启动成功,${BaseUrl}:${String(Port)}`);
})
``````

### 局部中间件

```````js
const express = require('express')

const app = express()

var Port = 80
var BaseUrl = 'http://127.0.0.1'

const mw1 = (req, res, next) => {
    console.log('调用了局部生效的中间件mw1')
    next()
}

const mw2 = (req, res, next) => {
    console.log('调用了局部生效的中间件mw2')
    next()
}

app.get('/', [mw1, mw2], (req, res) => {
    res.send(req.url)
})

app.listen(Port, () => {
    console.log(`服务器启动成功,${BaseUrl}:${String(Port)}`);
})
```````

### 内置中间件

`````js
const express = require('express')
const app = express()

// 注意：除了错误级别的中间件，其他的中间件，必须在路由之前进行配置
// 通过 express.json() 这个中间件，解析表单中的 JSON 格式的数据
app.use(express.json())
// 通过 express.urlencoded() 这个中间件，来解析 表单中的 url-encoded 格式的数据
app.use(express.urlencoded({ extended: false }))

app.get('/user', (req, res) => {
    // 在服务器，可以使用 req.body 这个属性，来接收客户端发送过来的请求体数据
    // 默认情况下，如果不配置解析表单数据的中间件，则 req.body 默认等于 undefined
    console.log(req.body)
    res.send('ok')
})

app.get('/book', (req, res) => {
    // 在服务器端，可以通过 req,body 来获取 JSON 格式的表单数据和 url-encoded 格式的数据
    console.log(req.body)
    res.send('ok')
})

app.listen(80, function () {
    console.log('Express server running at http://127.0.0.1')
})
`````

### 其他注意项

- 中间件注意事项
- 写在路由前面
- 语法规范next（）结尾
- 连续多个中间件，共享res和req
- 中间件可外置，但不是什么必须的技能

## 接口怎么写

本质就是监听端口根据参数返回对应的值

### 必要的流程

- 引入内置的解析表单数据的中间件
- 配置jsonp
- 配置跨域的npm包-----cors
- 路由挂载

`````js
const express = require('express')
const app = express()

// 配置解析表单数据的中间件
app.use(express.urlencoded({ extended: false }))

// 必须在配置 cors 中间件之前，配置 JSONP 的接口
app.get('/api/jsonp', (req, res) => {
    // TODO: 定义 JSONP 接口具体的实现过程
    // 1. 得到函数的名称
    const funcName = req.query.callback
    // 2. 定义要发送到客户端的数据对象
    const data = { name: 'zs', age: 22 }
    // 3. 拼接出一个函数的调用
    const scriptStr = `${funcName}(${JSON.stringify(data)})`
    // 4. 把拼接的字符串，响应给客户端
    res.send(scriptStr)
})

// 一定要在路由之前，配置 cors 这个中间件，从而解决接口跨域的问题
const cors = require('cors')
app.use(cors())

const router = require('./apiRouter')
app.use('/api', router)

app.listen(80, () => {
    console.log('express server running at http://127.0.0.1')
})
`````

### 路由本体

`````js
const express = require('express')
const router = express.Router()

router.get('/get', (req, res) => {
  // 通过 req.query 获取客户端通过查询字符串，发送到服务器的数据
  const query = req.query
  
  res.send({
    status: 0,
    msg: 'GET 请求成功！', 
    data: query, 
  })
})

router.post('/post', (req, res) => {
  // 通过 req.body 获取请求体中包含的 url-encoded 格式的数据
  const body = req.body
  
  res.send({
    status: 0,
    msg: 'POST 请求成功！',
    data: body,
  })
})

router.delete('/delete', (req, res) => {
  res.send({
    status: 0,
    msg: 'DELETE请求成功',
  })
})
//暴露
module.exports = router

`````

## mysql

安装mysql第三方库

`````shell
npm i mysql
`````

链接

`````````js
const mysql = require('mysql')
//建立与 MySQL 数据库的连接关系
const db = mysql.createPool({
    host: '192.168.75.143',
    user: 'root',
    password: 'xxxxxxxxxxxxxxxxxxxxx',
    database: 'NodeEx',
})
`````````

常见的sql实例

`````js
var sqlStr = 'select * from man'
db.query(sqlStr, (err, results) => {
    // 查询数据失败
    if (err) return console.log(err.message)
    // 查询数据成功
    // 注意：如果执行的是 select 查询语句，则执行的结果是数组
    console.log(results)
})
`````

````js
sqlStr = 'insert into man(user,id,sex) values ("xiao","dd","man")'
db.query(sqlStr, (err, results) => {
    // 执行 SQL 语句失败了
    if (err) return console.log(err.message)
    // 成功了
    // 注意：如果执行的是 insert into 插入语句，则 results 是一个对象
    // 可以通过 affectedRows 属性，来判断是否插入数据成功
    if (results.affectedRows === 1) {
        console.log('插入数据成功!')
    }
})
````

## NodeWeb

### http无状态

http是无状态的，简单来说就是本身没有记录相关的登录或者历史的信息，解决这个问题最初级也是最不安全的方法是使用cookies，以此来区别用户和会话

### Session机制

Session的诞生是为了突破http无状态的同时，加强安全

通过在用户浏览器的cookies中加入一个Sessionid的字段，然后在服务器与这个字符串相对比，提取相关信息，以此校对身份

#### 流程

1. 直接把用户塞入权限界面
2. 打开界面的同时，发出异步请求，请求服务器Session认证标识
3. 如果没有Session则直接弹回登录界面，反之则留下
4. 登录成功后，后端node设置认证标识
5. 登出销毁标识

#### 核心代码

- 配置 Session 中间件

``````````javascript
//....导入
const session = require('express-session')
app.use(
  session({
    secret: 'itheima',
    resave: false,
    saveUninitialized: true,
  })
)
``````````

- 请将登录成功后的用户信息，保存到 Session 中

`````javascript
// 登录的 API 接口
app.post('/api/login', (req, res) => {
  // 判断用户提交的登录信息是否正确
  if (req.body.username !== 'admin' || req.body.password !== '000000') {
    return res.send({ status: 1, msg: '登录失败' })
  }
  // 注意：只有成功配置了 express-session 这个中间件之后，才能够通过 req 点出来 session 这个属性
  req.session.user = req.body // 用户的信息
  req.session.islogin = true // 用户的登录状态

  res.send({ status: 0, msg: '登录成功' })
})
`````

- 获取用户身份标识的代码

````````javascript
app.get('/api/username', (req, res) => {
  // TODO_03：请从 Session 中获取用户的名称，响应给客户端
  if (!req.session.islogin) {
    return res.send({ status: 1, msg: 'fail' })
  }
  res.send({
    status: 0,
    msg: 'success',
    username: req.session.user.username,
  })
})
````````

### JWT机制

json web token

#### 流程

1. 准备一个密钥加密一些用户信息
2. 在登录成功后，后端把这些东西传回去
3. 前端收到加密token，遇到权限界面直接传加密token
4. 后端收到加密token，解密，放行

#### 核心代码

- 基本工作

`````javascript
//...导入
const jwt = require('jsonwebtoken')
const expressJWT = require('express-jwt')

// 允许跨域资源共享
const cors = require('cors')
app.use(cors())

// 解析 post 表单数据的中间件
const bodyParser = require('body-parser')
app.use(bodyParser.urlencoded({ extended: false }))
`````

- 定义 secret 密钥，建议将密钥命名为 secretKey

`````javascript
const secretKey = 'itheima No1 ^_^'
`````

- 注册将 JWT 字符串解析还原成 JSON 对象的中间件

`````javascript
app.use(expressJWT({ secret: secretKey }).unless({ path: [/^\/api\//] }))
`````

- 登录接口

``````javascript
app.post('/api/login', function (req, res) {
  // 将 req.body 请求体中的数据，转存为 userinfo 常量
  const userinfo = req.body
  // 登录失败
  if (userinfo.username !== 'admin' || userinfo.password !== '000000') {
    return res.send({
      status: 400,
      message: '登录失败！',
    })
  }
  // 登录成功
``````

- 在登录成功之后，调用 jwt.sign() 方法生成 JWT 字符串。并通过 token 属性发送给客户端
- jwt.sign的注意事项
  - 参数1：用户的信息对象
  - 参数2：加密的秘钥
  - 参数3：配置对象，可以配置当前 token 的有效期
  - 千万不要把密码加密到 token 字符中

````javascript
const tokenStr = jwt.sign({ username: userinfo.username }, secretKey, { expiresIn: '30s' })
  res.send({
    status: 200,
    message: '登录成功！',
    token: tokenStr, // 要发送给客户端的 token 字符串
  })
})
````

- 有权限的 API 接口

  使用 req.user 获取用户信息，并使用 data 属性将用户信息发送给客户端

`````javascript
app.get('/admin/getinfo', function (req, res) {
  console.log(req.user)
  res.send({
    status: 200,
    message: '获取用户信息成功！',
    data: req.user, // 要发送给客户端的用户信息
  })
})
`````

- 使用全局错误处理中间件，捕获解析 JWT 失败后产生的错误

````javascript
app.use((err, req, res, next) => {
  // 这次错误是由 token 解析失败导致的
  if (err.name === 'UnauthorizedError') {
    return res.send({
      status: 401,
      message: '无效的token',
    })
  }
  res.send({
    status: 500,
    message: '未知的错误',
  })
})
````



