# Vue-Element-Admin

## VEA是什么

是一个字节跳动大佬为首的一群人写的前端模板，         ***star高达72.8K***

高频率出现在各种，不想学前端的后端视频，毕设速成视频，全栈速成

### 作者原话

基于 [vue](https://github.com/vuejs/vue) 和 [element-ui](https://github.com/ElemeFE/element)实现。它使用了最新的前端技术栈，内置了 i18 国际化解决方案，动态路由，权限验证，提炼了典型的业务模型，提供了丰富的功能组件，它可以帮助你快速搭建企业级中后台产品原型。相信不管你的需求是什么，本项目都能帮助到你

### 相关链接：

github：https://github.com/PanJiaChen/vue-element-admin

中文文档：https://panjiachen.github.io/vue-element-admin-site/zh/

作者写的vue进阶教程：https://juejin.cn/post/6844903476661583880

### 其内容包括但不限于：

- 面包屑
- 下拉栏切换
- excel输入输出
- 常见的图片操作，花式布局
- zip操作
- element的各种组件
- 权限管理

### 依赖包括但不限于：

-  "axios": "0.18.1",
-   "echarts": "4.2.1",
-   "element-ui": "^2.13.2",
-   "vue-count-to": "1.0.13",
-   "vue-router": "3.0.2",
-   "vuex": "3.1.0",
-   "xlsx": "0.14.1"

## 借鉴（cv）指南

本项目的定位是后台集成方案，不太适合当基础模板来进行二次开发。因为本项目集成了很多你可能用不到的功能，会造成不少的代码冗余。如果你的项目不关注这方面的问题，也可以直接基于它进行二次开发

- 集成方案: vue-element-admin:   https://github.com/PanJiaChen/vue-element-admin
- 基础模板: vue-admin-template:    https://github.com/PanJiaChen/vue-admin-template
- 桌面终端: electron-vue-admin:      https://github.com/PanJiaChen/electron-vue-admin
- Typescript 版: vue-typescript-admin-template:    https://github.com/Armour/vue-typescript-admin-template
- Others: awesome-project:    https://github.com/PanJiaChen/vue-element-admin/issues/2312

## 无权限登录

这个模板封装的最死的东西分别是登录，权限实现，登出！

这个模板封装的最死的东西分别是登录，权限实现，登出！

这个模板封装的最死的东西分别是登录，权限实现，登出！

### 模板权限实现原理

极简：后端传回来一个字符串，然后根据这个字符串，加载路由，路由可以携带一个权限数组。

### 无权限登录实现

#### 宏观步骤

1. 登录按钮按下，调用层层封装的axios，把数据传递到后端
2. 后端返回token
3. request会有一个自动的回调函数，会设置header加入token，自动发第二次请求
4. 后端返回用户基本信息，包含权限字符串

#### 登录按钮的函数解析

1. handleLogin函数把用户名和密码传递给一个封装的axios函数

   `````js
         this.$refs.loginForm.validate((valid) => {
           if (valid) {
             this.loading = true;
             this.$store
               // .dispatch("user/login", this.loginForm)
               .dispatch("user/login", this.encryptData.new)
               .then(() => {
                 this.$router.push({ path: this.redirect || "/" });
                 this.loading = false;
               })
               .catch(() => {
                 this.loading = false;
               });
           } else {
             console.log("error submit!!");
             return false;
           }
         });
   `````

2. axios被高度封装，使用的方法是   一半的路径+传递内容

3. axios中的路径被一个js同一管理  即api/userjs

   ``````js
   export function login(data) {
     return request({
       url: '/user/login',
       method: 'post',
       data
     })
   }
   ``````

4. api/userjs的来自工具类的request，而request则是axios的封装

   - 包含常见错误code的处理
   - 是否发起第二次请求
   - 一切的内容通过service对象和ES暴露提供给其他封装函数
   - 第二次请求还会涉及一个cookie的设置，一个js，但是可以不管

5. request中修改基础的URL，以实现字符串的拼接，整个过程封装三次

   ``````js
   const service = axios.create({
     baseURL: "http://hcj.fit:8889/model",
     timeout: 5000
   })
   ``````

### 第二次请求的浅析

#### token自定义

``````js
service.interceptors.request.use(
  config => {
    if (store.getters.token) {
      config.headers['Authorization'] = "Bearer " + getToken()
    }
    return config
  },
  error => {
    console.log(error) 
    return Promise.reject(error)
  }
)
``````

#### 后端返回元素示例

`````json
{
    "success": true,
    "code": 200,
    "msg": null,
    "data": {
        "username": "Shem",
        "nickname": "向小姐",
        "roles": [
            "admin"
        ],
        "introduction": "introduction",
        "avatar": "图片链接",
        "phone": "52346547",
        "mail": "9999999999@qq.com"
    }
}
`````







