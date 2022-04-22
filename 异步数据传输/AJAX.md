# AJAX

## 基本信息

### 什么是AJAX

Ajax即**A**synchronous **J**avascript **A**nd **X**ML（异步JavaScript和XML）

Ajax 不是一种新的编程语言，而是一种用于创建更好更快以及交互性更强的Web应用程序的技术。

使用Ajax技术网页应用能够快速地将增量更新呈现在用户界面上，而不需要重载（刷新）整个页面，这使得程序能够更快地回应用户的操作

### 包含什么

- jQuery

- fetch

- axios

### 相关文档

W3Sxhool-jQuery ajax - 方法：https://www.w3school.com.cn/jquery/ajax_ajax.asp

W3Sxhool-ajax：https://www.w3school.com.cn/js/js_ajax_intro.asp

Axios中文文档：http://www.axios-js.com/

### 优缺点

- 可以根据用户的事件，发出请求
- 有跨域问题，多服务请求
- SEO问题，爬虫爬不到

### 预备工具

- node启动

  `````bash
  node xxxx.js
  `````

- nodemon启动

  ``````bash
  #安装
  npm install -g nodemon
  #启动
  nodemon xxxx.js
  ``````

## 知识预备

### HTTP

超文本传输协议（Hyper Text Transfer Protocol）

- 响应报文

  - 行头

    `````http
    Accept:text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
    Accept-Encoding: gzip, deflate, br
    Connection: keep-alive
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/95.0.4638.69 Safari/537.36
    `````

  - 空行体

    `````html
    <html></html>
    `````

- 什么是状态码

  - 404找不到

### node

一个服务端

### Express框架

看成json-server，一个假后端即可

````bash
npm install express
````

#### 配置对应的js文件

`````js
//引入express
const express = require('express');
//创建对象
const app = express();

//设置响应体
app.get('/server', (request, response) => {
    response.setHeader('Access-Control-Allow-Origin', '*')
    response.send("hello world");
});

//监听端口8000
app.listen(8000, () => {
    console.log("服务启动，8000端口");
});
`````

- response.setHeader('Access-Control-Allow-Origin', '*')  -----------允许跨域

#### 启动服务端

````bash
node day1.js
````

## 请求

### GET-HelloWorld

1. 创建对象
2. 初始化，设置方法，路由
3. 发送
4. 事件触发
   - onreadystatechange：当某个对象的readystate属性改变时候触发
   - readystate：状态显示
     - 0：未初始化
     - 1：open方法调用完毕
     - 2：send方法调用完毕
     - 3：服务端返回部分内容
     - 4：返回完成了
   - 状态码
     - 2xx：成功
     - 404：找不到

````js
  var bu = document.getElementById('bu');
  var result = document.getElementById('result');
        bu.onclick = function () {
            const xhr = new XMLHttpRequest();
            xhr.open('GET', 'http://127.0.0.1:8000/server');
            xhr.send();
            xhr.onreadystatechange = function () {
                if (xhr.readyState === 4) {
                    if (xhr.status >= 200 && xhr.status <= 300) {
                        result.innerHTML = xhr.response;
                    }
                }
            }
        }
````

#### 传递参数

 xhr.open('GET', 'http://127.0.0.1:8000/server?a=100&b=100');

### POST

`````js
app.post('/server', (request, response) => {
    response.setHeader('Access-Control-Allow-Origin', '*')
    response.send("hello world");
});
`````

````js
  bu.onclick = function () {
            const xhr = new XMLHttpRequest();
            xhr.open('POST', 'http://127.0.0.1:8000/server');
            //修带参数 
            xhr.send('a=100&b=200');
            xhr.onreadystatechange = function () {
                if (xhr.readyState === 4) {
                    if (xhr.status >= 200 && xhr.status <= 300) {
                        result.innerHTML = xhr.response;
                    }
                }
            }
        }
````

#### 自定义属性

````js
app.all('/server', (request, response) => {
    response.setHeader('Access-Control-Allow-Origin', '*')
    response.setHeader('Access-Control-Allow-Headers', '*')
    response.send("hello world");
});
````

- 之所以写all是因为可能掺杂了其他请求
- Origin--->来源     允许跨域
- Headers--->请求头    允许自定义请求体

```````js
 bu.onclick = function () {
            const xhr = new XMLHttpRequest();
            xhr.open('POST', 'http://127.0.0.1:8000/server');
            xhr.setRequestHeader('Content-Type', "application/x-www-form-urlencoded");
            xhr.setRequestHeader('name', "atguigu");
            xhr.send('a=100&b=200');
            xhr.onreadystatechange = function () {
                if (xhr.readyState === 4) {
                    if (xhr.status >= 200 && xhr.status <= 300) {
                        result.innerHTML = xhr.response;
                    }
                }
            }
        }
```````

## 服务端响应

### json处理

#### 响应体

````js
app.all('/server', (request, response) => {
    response.setHeader('Access-Control-Allow-Origin', '*')
    response.setHeader('Access-Control-Allow-Headers', '*')
    const data = {
        name: 'atguigu'
    }
    let str = JSON.stringify(data);
    response.send(str);
});
````

#### 请求

````js
 xhr.onreadystatechange = function () {
                if (xhr.readyState === 4) {
                    if (xhr.status >= 200 && xhr.status <= 300) {
                        // 手动转化
                        let data = JSON.parse(xhr.response);
                        console.log(data);
                        //自动转化
                        console.log(xhr.response.name);
                    }
                }
            }
````

### IE缓存清理

`````js
xhr.open('POST', 'http://127.0.0.1:8000/server' + "?=" + Date.now());
`````

### 手动取消请求

````js
 qu.click = function () {
                xhr.abort()
            }
````

### 延迟设置和处理函数

``````js
 xhr.timeout = 2000
            xhr.ontimeout = function () {
                alert("404")
            }
``````

### 取消重复点击

``````js
        let xhr = null;
        let IsSent = false;
        bu.onclick = function () {
            if (IsSent) {
                xhr.abort()
            }
            const xhr = new XMLHttpRequest();
            xhr.timeout = 2000
            xhr.ontimeout = function () {
                alert("404")
            }
            xhr.open('POST', 'http://127.0.0.1:8000/server');
            xhr.send();
            xhr.onreadystatechange = function () {
                if (xhr.readyState === 4) {
                    let IsSent = true;
                    if (xhr.status >= 200 && xhr.status <= 300) {
                        // 手动转化
                        let data = JSON.parse(xhr.response);
                        console.log(data);
                        //自动转化
                        console.log(xhr.response.name);
                    }
                }
            }
        }
``````

## 常用AJAX工具包

### jQuery

https://www.w3school.com.cn/jquery/ajax_ajax.asp

- CDN

  `````js
     <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
  `````

#### 常规get，post

- 函数代码

  - get / post

  ``````js
       $('#get').click(function () {
              $.get('http://localhost:8000/jq', { a: 100 }, function () {
                  console.log(data);
              })
          })
  
          $('#post').click(function () {
              $.post('http://localhost:8000/jq', { a: 100 }, function () {
                  console.log(data);
              })
          })
  ``````
  
- json
  
  ``````js
            $('#get').click(function () {
                $.get('http://localhost:8000/jq', { a: 100 }, function () {
                    console.log(data);
                }, 'json')
            })
  ``````

响应体代码

````js
app.all('/jq', (request, response) => {
    response.setHeader('Access-Control-Allow-Origin', '*')
    response.setHeader('Access-Control-Allow-Headers', '*')
    response.send("hello world");
});
````

#### ajax方法

函数

``````js
 $('#get').click(function () {
            $.ajax({
                url: "http://localhost:8000/jq",
                data: {
                    a: 100
                },
                type: 'GET',
                dataType: 'json',
                success: function (data) {
                    console.log(data);
                }
            })
        })
``````

响应体

```````js
app.all('/jq', (request, response) => {
    response.setHeader('Access-Control-Allow-Origin', '*')
    response.setHeader('Access-Control-Allow-Headers', '*')
    let data = { name: 'shangguig' };
    response.send(JSON.stringify(data));
});
```````

### Axios

- 引入

  ````````js
  <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
  ````````

  一次简单的请求

````````js
     var get = document.getElementById('get');
        axios.defaults.baseURL = 'http://localhost:8000';
        get.onclick = function () {
            axios.get('/jq', {
                params: {
                    id: 100,
                    vip: 7,
                },
                headers: {
                    name: 'lio',
                    age: 20
                }
            }).then(response => {
                console.log(response.status);
                console.log(response.statusText);
                console.log(response.headers);
                console.log(response.data);
            })
        }
````````

响应体

````js
app.all('/jq', (request, response) => {
    response.setHeader('Access-Control-Allow-Origin', '*')
    response.setHeader('Access-Control-Allow-Headers', '*')
    let data = { name: 'shangguig' };
    response.send(JSON.stringify(data));
});
````

## 跨域

### 同源策略

ajax和目标资源的协议，域名，端口，全部相同

### 完全同域的案例

`````````js
     var get = document.getElementById('get');

        get.onclick = function () {
            const xhr = new XMLHttpRequest();
            xhr.open('GET', '/data');
            xhr.send();
            xhr.onreadystatechange = function () {
                if (xhr.readyState === 4) {
                    let IsSent = true;
                    if (xhr.status >= 200 && xhr.status <= 300) {
                        alert(xhr.response);
                    }
                }
            }
        }
`````````

`````js
const express = require('express');
const app = express();

app.all('/home', (request, response) => {
    response.sendFile(__dirname + '/jQ.html')
});

app.all('/data', (request, response) => {
    response.send('用户数据')
});

app.listen(9000, () => {
    console.log("服务启动，9000端口");
});
`````

### get跨域/jsonP

- 非官方

#### 要点

- 服务端的链接在函数的后面
- 原理
  - 通过访问服务端的端口，得到一段js代码
  - 通过制作函数和获取函数，输出变量
  - 注意变量的作用域

HTML代码

`````html
    <style>
        #result {
            width: 300px;
            height: 300px;
            border: solid 1px rgb(0, 0, 0);
        }
    </style> 
    <button id="get">get</button>
    <button id="post">post</button>
    <!-- <button id="bu"> sned</button> -->
    <div id="result"></div>
    <script>
        function handle(data) {
            const re = document.getElementById("result");
            re.innerHTML = data.name;
        }
    </script>
    <script src="http://127.0.0.1:8000/data"></script>
`````

响应体

``````js
app.all('/data', (request, response) => {
    const data = {
        name: "lio"
    };

    let str = JSON.stringify(data);
    response.end(`handle(${str})`);
});
``````

### CORS

- 官方跨域

- 星号部分可以填具体的内容，例如地址，方法等

``````js
app.all('/data', (request, response) => {
    response.setHeader('Access-Control-Allow-Origin', '*');
    response.setHeader('Access-Control-Allow-Headers', '*');
    response.setHeader('Access-Control-Allow-Method', '*');
  
    response.end('ddd');
});
``````







