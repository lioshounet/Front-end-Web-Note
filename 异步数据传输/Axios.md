# Axios

## 背景介绍

局部刷新--在不刷新web的情况下，获取服务器的数据

一开始使用的是jQuery

后来封装了之后，就是ajax

然后来到框架时代，vue开始推荐使用axios，也就是ajax封装

## 安装

附件条件：环境：node+vue

部分内容来自github，github连接：https://github.com/typicode/json-server

### axios和json-server的安装

```bash
npm install json-server
npm install axios
```

### 在vue里使用axios

```js
const axios = require("axios");
```

## 使用

### 把一个json当成数据库

- 写入json命名为db.json

```json
{
  "posts": [
    {
      "id": 1,
      "title": "json-server",
      "author": "typicode"
    },
    {
      "title": "卷王肖江早上八点起来赶ppt",
      "author": "卷王肖江",
      "id": 7
    }
  ],
  "comments": [
    {
      "id": 1,
      "body": "some comment",
      "postId": 1
    }
  ],
  "profile": {
    "name": "typicode"
  }
}
```

- 连接服务

```bash
json-server --watch db.json
```

增删改查函数的写法

url与json的对应关系

url:："http://localhost:3000/posts"   对应的端口号会在连接db.json终端中显示

 "http://localhost:3000/posts/2"对应的是json的id，如果是/2就是在找id为2的那条信息

- 增

  ```js
    bupost() {
        axios({
          method: "POST",
          url: "http://localhost:3000/posts",
          data: {
            title: "卷王肖江早上八点起来赶ppt",
            author: "卷王肖江",
          },
        }).then((response) => {
          console.log(response.data);
        });
      },
  ```

- 删

  ```js
   budel() {
        axios({
          method: "delete",
          url: "http://localhost:3000/posts/2",
        }).then((response) => {
          console.log(response.data);
        });
      },
  ```

- 改

  ```js
   buput() {
        axios({
          method: "PUT",
          //数据修改
          url: "http://localhost:3000/posts/4",
          data: {
            title: "卷王肖江早上八点起来赶ppt,卷死了",
            author: "卷王肖江不愧卷王",
          },
        }).then((response) => {
          console.log(response.data);
        });
      },
  ```

  

- 查

  ```js
    buget() {
        axios({
          method: "GET",
          url: "http://localhost:3000/posts/2",
        }).then((response) => {
          console.log(response);
        });
      },
  ```

### axios自带的方法（只举例两个）

- github的axios连接:https://github.com/axios/axios

- request

```js
  arequest() {
      axios
        .request({
          method: "GET",
          url: "http://localhost:3000/comments",
        })
        .then((response) => {
          console.log(response);
        });
    },
```

- post

```js
 apost() {
      axios
        .post("http://localhost:3000/comments", {
          body: "卷",
          postId: "2",
        })
        .then((response) => {
          console.log(response);
        });
    },
```

### 拦截器

### 请求取消(防止有人通过非正常手段修改数据)

一个订单提交案例:

- 拦截器
- config为传递内容，是一个对象，通过访问内容决定传递

```js
 axios.interceptors.request.use(
      (config) => {
        if (config.method != "post") {
          // console.log(config.key);
          // console.log("拦截器启动成功,方法非post");
          console.log(config);
          return config;
        } else {
          console.log("检测到post方法");
          if (config.key == "KOI333") {
            console.log("KEY正确");
            return config;
          } else {
            alert("非法post");
            console.log("非法post");
            // return null;
          }
        }
      },
      (err) => {
        console.log("拦截器启动失败");
      }
    );

```

- 受限制的方法

```js
 axios({
            method: "POST",
            url: "http://localhost:3000/bookroom",
            key: "KOI333",
            data: {
              hotlename: this.roomlist[idnmb].hotlename,
              roomnmb: this.roomlist[idnmb].roomnmb,
              price: this.roomlist[idnmb].price,
              where: this.roomlist[idnmb].where,
              link: this.roomlist[idnmb].link,
              start: this.thetime[0],
              end: this.thetime[1],
            },
          }).then((response) => {
            // console.log(response.data);
          });
```



