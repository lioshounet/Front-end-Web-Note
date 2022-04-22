# VUE-cli（脚手架）

## 基础概念

前端：属于前台，界面

后端：属于前台，

后台：界面跳转，数据共享

## Node.js(cmd的操作全部在管理员权限下进行)

后端的玩意

前端所需要的东西：那几个npm指令

安装

- 无脑下一步

- 检测：

  - ```bash
    npm -v
    node -v
    ```

- 配置环境变量

## vue-cli2

安装

- npm与cnpm的区别

  - npm：node package manager

  ​       nodejs 包管理器

  - cnpm：npm的国内版本

- 参数与命令

  - -g 全局安装
  - -D 全部依赖

环境变量

- Path新建
  - \nodejs
  - \nodejs\node_global\

### npm语法

- ```bash
  npm install packagename --save #-S 安装到生产环境依
  npm install packagename --save --dev #-D 安装到开发环境 
  npm install packagename -g #--global  全局 
  npm install packagename #i 安装缩写
  ```

- 安装完成后，会在package.json文件中出现

### vue语法

- 创建项目

  ```bash
  vue init webpack name
  ```

- 界面的创建，跳转

  - 界面新建

    - ```vue
      <template>
        <div>
          <p>推特</p>
          <router-link to="/">OnlyFans</router-link>
            <!-- 跳转的标签 -->
        </div>
      </template>
      ```

  - 界面导入

    - ```js
      import HelloWorld from '@/components/HelloWorld'
      ```

  - index.js文件下面的路由

    - ```js
      export default new Router({
        routes: [
          {
            path: '/',            //------------路径小写
            name: 'HelloWorld',   //-------------只有首页有与文件名保持一致
            component: HelloWorld //--------与文件名保持一致
          },
          {
            path: '/tui',
            component: Tui
          },
        ]
      })  
      ```

- 子路由

  - ```js
    export default new Router({
      routes: [
        {
          path: '/',
          name: 'HelloWorld',
          component: HelloWorld
        },
        {
          path: '/tui',
          component: Tui,
          children: [
            {
              path: '/onlyfans',
              component: Onlyfans,
            }
          ]
        },
      ]
    })
    ```

  - 父级的路由添加

    ```vue
    <div>
        <p>推特</p>
        <router-link to="/Onlyfans">Onlyfans</router-link>
        <router-view></router-view>
      </div>
    ```

  - 目录结构

    - src下面有assets文件夹，组件文件夹和router文件夹以及App.vue，main.js
    - App.vue下面放的是最重要的组件，简单来说就是界面中不会变的东西
    - main.js是设置App.vue的相关文件
    - 组件文件夹下面是写的组件，而router则是相关的路由，挂载
    - assets文件夹下面主要是媒体素材

### 项目到手后的流程

- npm,node,以及vuecli的安装

- ```bash
  # install dependencies
  #安装依赖
  npm install
  
  # serve with hot reload at localhost:8080
  npm run dev
  ```

## vue-cli3（全程在管理员权限下）

- 二代的卸载

  - ```bash
    npm uni vue-cli -g
    ```

- 三代的安装

  - ```bash
    npm install -g @vue/cli
    ```

- ### CSS文件的引入

  - ```html
    <style>
    @import "../../public/css/list.css";
    </style>
    ```

- ### 组件调用

- ```js
  //声明
  import List from "./components/List";
  import Goods from "./components/Goods";
  
  export default {
    components: { List, Goods },
  };
  ```

- ```html
  <!-- 使用 -->
  <div class="left_box">
        <List></List>
      </div>
      <div class="right_box">
        <div class="top">
          <img
            src="../public/img/main/best_sell/小熊你好.png"
            alt=""
            width="980px"
          />
        </div>
        <Goods></Goods>
      </div>
  ```

- ### 参数传递

  - 新建msg.js写入一下内容

    ```js
    import Vue from 'vue'
    export default new Vue
    ```

  - 触发传出

    ```html
    <li @click="autumnforest()">秋日森林</li>
    ```

  - 参数传出

    ```js
    import Msg from "./msg";
    export default {
      methods: {
        missjane: function () {
            //核心代码
          Msg.$emit("showwhat", "missjane");
        },
        autumnforest: function () {
          Msg.$emit("showwhat", "autumnforest");
        },
      },
    };
    ```

  - 参数传入

    ```js
    import Msg from "./msg.js";
    export default {
      data() {
        return {
          xi: "",
        };
      },
      mounted() {
        var _this = this;
        Msg.$on("showwhat", function (m) {
          _this.xi = m;
        });
      },
    };
    ```

- json数据的使用（http，axios）

  - 在脚手架里面安装axios

    ```bash
    npm i axios -S
    ```

    - 安装成功之后

      在package.json中会有这么一行

      ```json
       "dependencies": {
          "axios": "^0.21.1",
          "core-js": "^3.6.5",
          "vue": "^2.6.11",
          "vue-router": "^3.2.0"
        },
      ```

  - 在main.js中写入

    - ```javascript
      import axios from 'axios'
      Vue.prototype.$http = axios
      ```

  - 在public文件夹里面新建json文件夹

    - 在里面写入json

    - ```json
      [
          {
              "goodName": "【小物】阿尔可纳之梦-头饰蝴蝶结.png",
              "img": "img/goods/【小物】阿尔可纳之梦-头饰蝴蝶结.png"
          },
          {
              "goodName": "【小物】阿尔可纳之梦-头饰蝴蝶结.png",
              "img": "img/goods/【小物】阿尔可纳之梦-头饰蝴蝶结.png"
          }
      ]
      ```

  - 使用axios引用json的数据

    ```vue
    <template>
      <div class="sheet" name="sheet">
        <div class="good" v-for="good in list">
          <img v-bind:src="good.img" alt="" />
          <p>{{ good.goodName }}</p>
        </div>
      </div>
    </template>
    <style>
    @import "../../../public/css/sheets.css";
    </style>
    <script>
    export default {
      name: "sheet",
      data() {
        var _this = this;
        this.$http.get("json/good_anthonycake.json").then(function (res) {
          _this.list = res.data;
        });
        return {
          list: [],
        };
      },
    };
    </script>
    ```

- ### props与监听

- 父组件使用属性的捆绑

  ```vue
  <div class="goods" v-if="xi == 'missjane' || xi == ''">
        <div class="til">
          <p>货架</p>
        </div>
        <sheets :goodId="'missjane'"></sheets>
      </div>
  ```

- 子组件接受属性并使用监听触发函数

  ```js
   props: {
      goodId: String,
    },
  
    watch: {
      goodId() {
        var _this = this;
        var url = "";
        url = "json/good_" + _this.goodId + ".json";
        this.$http.get(url).then(function (res) {
          _this.list = res.data;
        });
  
        return {
          list: [],
        };
      },
    },
  ```

  

- ### vue3的可视化界面

  - 启动

    ```bash
    vue ui
    ```


### 案例问题讨论

#### vue函数绑定使用参数作为变量

- 不使用v-model
- 不使用diaplay的标签参数传递
- vue在标签直接绑定函数
- 传入的函数是一个变量

```vue
 <div
     :class="{ upbook: index === clickIndex }"
     @click="upbook(index)"
 >
     <el-button type="text">提交</el-button>
</div>
```

- 用法
  - 触发的标签需要一个父标签
  - 父标签最好是ul或者div
  - 第一步
    - ：class=“{ 绑定函数：变量 === 事件+变量 }”
    - 第二个变量首字母大写
  - 第二步
    - 绑定函数
  - 第三步
    - 函数正常书写
  - 写完之后，点击父标签的任意子标签都会触发函数
- 案例：配合之前的element写的用户确认框，包含一个axios的POST请求

```js
    upbook(idnmb) {
      //此处可添加按钮重复点击校验
      this.$confirm("是否提交？", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(() => {
          axios({
            method: "POST",
            url: "http://localhost:3000/bookroom",
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
            console.log(response.data);
          });
          this.$message({
            type: "success",
            message: "提交成功!",
          });
        })
        .catch(() => {
          console.log("提交取消");
          this.$message({
            type: "info",
            message: "取消提交",
          });
        });
    },
```



