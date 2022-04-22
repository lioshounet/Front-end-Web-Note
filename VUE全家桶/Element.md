# element（本教程基于vue2）

### 安装流程

```bash
npm i element-ui -S
```

- 写入main.js

```js
import Vue from 'vue';
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
import App from './App.vue';

Vue.use(ElementUI);

new Vue({
  el: '#app',
  render: h => h(App)
});
```

## Container布局容器

`<el-container>`：外层容器。当子元素中包含 `<el-header>` 或 `<el-footer>` 时，全部子元素会垂直上下排列，否则会水平左右排列。

`<el-header>`：顶栏容器。

`<el-aside>`：侧边栏容器。

`<el-main>`：主要区域容器。

`<el-footer>`：底栏容器。

## 一种基本布局举例(举例见图片1)

```vue
<el-container style="height: 500px; border: 1px solid #eee">
  <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
    <el-menu :default-openeds="['1', '3']">
      <el-submenu index="1">
         右侧选项栏
      </el-submenu>
    </el-menu>
  </el-aside>
  
  <el-container>
    <el-header style="text-align: right; font-size: 12px">
         左侧顶栏
    </el-header>
    
    <el-main>
         左侧主题
    </el-main>
  </el-container>
</el-container>

<style>
  .el-header {
    background-color: #B3C0D1;
    color: #333;
    line-height: 60px;
  }
  
  .el-aside {
    color: #333;
  }
</style>
```



### 下拉框

```vue
<!--完整的案例-->
  <el-menu :default-openeds="['1', '3']">
    <el-submenu index="1">
      <template slot="title">
          图标的设置
          <i class="el-icon-message"></i>导航一
      </template>
      <el-menu-item-group>
        <template slot="title">分组一</template>
        <el-menu-item index="1-1">选项1</el-menu-item>
        <el-menu-item index="1-2">选项2</el-menu-item>
      </el-menu-item-group>
      <el-menu-item-group title="分组2">
        <el-menu-item index="1-3">选项3</el-menu-item>
      </el-menu-item-group>
      <el-submenu index="1-4">
        <template slot="title">选项4</template>
        <el-menu-item index="1-4-1">选项4-1</el-menu-item>
      </el-submenu>
    </el-submenu>
   </el-menu>
```

- 下拉表外框

```vue
<el-menu :default-openeds="['1', '3']">
    <!--:default-openeds="['1', '3']"是选项范围-->   
</el-menu>
```

- 下拉框的主标题

```vue
 <template slot="title">
      <!--图标的设置-->   
      <i class="el-icon-message"></i>
      <!--文字设置-->   
      导航一
 </template>
```

- 下拉框分组

```vue
  <el-menu-item-group>
      <!--组题-->   
    <template slot="title">分组一</template>
      <!--选项-->   
    <el-menu-item index="1-1">选项1</el-menu-item>
    <el-menu-item index="1-2">选项2</el-menu-item>
  </el-menu-item-group>
```

- 子下拉框（el-submenu嵌套）

```vue
<el-submenu index="3">
     <el-submenu index="3-4">
       <template slot="title">选项4</template>
       <el-menu-item index="3-4-1">选项4-1</el-menu-item>
     </el-submenu>
</el-submenu>
```

## 常见表格的基本使用(举例见图片1)

element部分

- 三个input框+一个提交按钮

```vue
  <el-input placeholder="请输入内容" v-model="userinfo[0].name">
        <template slot="prepend">昵称</template>
      </el-input>
      <br />
      <el-input placeholder="请输入内容" v-model="userinfo[0].tel">
        <template slot="prepend">电话</template>
      </el-input>
      <br />
      <el-input placeholder="请输入内容" v-model="userinfo[0].email">
        <template slot="prepend">邮箱</template>
      </el-input>
      <el-row>
        <el-button>提交</el-button>
      </el-row>
```

js部分

```js
export default {
  name: "ShemHotelMyinfo",
  data() {
    var _this = this;
    this.$http.get("json/home/userinfo.json").then(function (res) {
      _this.userinfo = res.data;
    });
    return {
      userinfo: [],
      input1: "",
        // 一般的形式是input的v-model连接
      input2: "",
      input3: "",
    };
  },

  mounted() {},

  methods: {},
};
```

### 图片1

![图片1](D:\ZPan\HTMLcode\自助学习\2021大三上自学\element\笔记以及相关图片\图片1.jpg)

### element表格的使用

- label是对应的表格标题
- prop是连接词，对应的是json的属性

```vue
 <div class="costlist">
      <el-table :data="costData" stripe>
        <el-table-column prop="date" label="日期" width="200">
        </el-table-column>
        <el-table-column prop="RevenueExpenditure" label="支出/收入">
        </el-table-column>
        <el-table-column prop="howmach" label="金额"> </el-table-column>
        <el-table-column prop="where" label="资金流动"> </el-table-column>
        <el-table-column prop="nmb" label="订单号码"> </el-table-column>
      </el-table>
    </div>
```

数据的连接

- 读取json到表格

  - :data捆绑
  - 返回一个json

  ```js
   data() {
      var _this = this;
      this.$http.get("json/home/costlist.json").then(function (ress) {
        _this.costData = ress.data;
      });
      return {
        //用户基本信息返回
        userinfo: [],
        costData: [],
      };
    },
  ```

- 直接使用

  - :data捆绑
  - 返回一个写好的json

  ```js
   data() {
      var _this = this;
      this.$http.get("json/home/costlist.json").then(function (ress) {
        _this.costData = ress.data;
      });
      return {
        //用户基本信息返回
        userinfo: [],
        costData: [    {
          hotlename: "艾尔迪亚大酒店",
          roomnmb: "404",
          price: "44",
          where: "希尔雅区689号",
          link: "/img/room/hotel-1979406_1920.png"
      },
      {
           hotlename: "艾尔迪亚大酒店",
          roomnmb: "404",
          price: "44",
          where: "希尔雅区689号",
          link: "/img/room/hotel-1979406_1920.png"
      }],
      };
    },
  ```

### card

- 一个实例的讲解

```vue
 <div class="boxs">
      <el-card class="box-card" v-for="card in roomlist">
        <div slot="header" class="clearfix">
          <span>卡片名称</span>
          <el-button style="float: right; padding: 3px 0" type="text"
            >查看详情
          </el-button>
        </div>
        <img v-bind:src="card.link" alt="" width="200PX" />
        <!-- <img src="/img/userinfo/indoors-4234071_1920.png" alt="" /> -->
        <div class="textbox">
          <div
            v-for="(item, o, i) in card"
            <!-- item是属性值  -->
            <!-- o是属性名  -->
            <!-- i是0123456  -->
            :key="o"
            v-if="i != 4"
           <!--不等于4就输出，最后一个连接就打不出来 -->
            class="text item"
          >
            {{ cardmarklist[i] + ":" + item }}
          </div>
        </div>
      </el-card>
    </div>
```

```js
export default {
  name: "ShemHotelMyinfo",
  data() {
    var _this = this;
    this.$http.get("json/room/stor.json").then(function (res) {
      _this.roomlist = res.data;
    });
    return {
      roomlist: [],
      cardmarklist: ["酒店名称", "房号", "价格", "地址"],
        //内容的头，对应cardmarklist[i]
    };
  },
};
```

### el-dialog(对话框)

- 这是一个通过按钮触发的对话框
- 这个对话内容非常多，可以塞下超多组件

```vue
<el-button
            style="float: right; padding: 3px 0"
            type="text"
            @click="dialogTableVisible = true"
            >订购
</el-button>

<el-dialog title="订单提交" :visible.sync="dialogTableVisible">
            <!-- 弹出内容 -->
            <el-form :model="form">
            <!--内部内容最好是el组件 -->
            </el-form>
</el-dialog>
```

```vue
<script>
export default {
  name: "ShemHotelMyinfo",
  data() {
    return {
      dialogTableVisible: false,
      dialogFormVisible: false,
      formLabelWidth: "120px",
    };
  },
</script>
```

### el-date-picker日期段选择

- 这个案例不仅有日期选择器
- change事件：指用户完成日期选择之后的鼠标焦点移动
- 通过change触发hh()函数
- hh()函数访问data必须加this
- el-date-picker会返回一个数组
  - 这是两个两位的数组
  - 第一位是起始时间，date对象，本质是一个毫秒计算的1970时间戳
  - 第二位是终止时间，date对象，本质是一个毫秒计算的1970时间戳
  - 两个相减会得到一个毫秒数
- 为什么+1，因为是算天数

```vue
  <div class="price">单价{{ card.price }}/晚</div>
              <div class="allprice">
                共计{{ Number(card.price) * Number(daynub) }}
              </div>
              <div class="block">
                <span class="demonstration">选择时间</span>
                <br />
                <el-date-picker
                  v-model="value1"
                  type="daterange"
                  range-separator="至"
                  start-placeholder="开始日期"
                  end-placeholder="结束日期"
                  @change="hh()"
                >
                </el-date-picker>
 </div>
```

```vue
<script>
var daynub = 0;
export default {
  name: "ShemHotelMyinfo",
  data() {
    var _this = this;
    this.$http.get("json/room/stor.json").then(function (res) {
      _this.roomlist = res.data;
    });
    return {
      roomlist: [],
      value1: "",
      daynub: 0,
    };
  },
  methods: {
    hh() {
      this.daynub =
        Number((this.value1[1] - this.value1[0]) / 1000 / 3600 / 24) + 1;
      // alert(this.daynub);
    },
  },
};
</script>
```

### 信息弹出框（微量内容）

- log成功和取消分别对应其响应内容

```vue
 <el-button type="text" @click="upbook()">提交</el-button>
```

```js
 upbook() {
      //防止重复请求的校验   还没做
      this.$confirm("是否提交？", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(() => {
          console.log("成功");
          this.$message({
            type: "success",
            message: "提交成功!",
          });
        })
        .catch(() => {
          console.log("取消");
          this.$message({
            type: "info",
            message: "取消提交",
          });
        });
    },
```

### 评价组件

- value2返回打出的星星

```vue
           <div class="givepoint">
              <span class="demonstration">给我打分</span>
              <el-rate v-model="value2" :colors="colors"> </el-rate>
            </div>
```

```js
    return {
      //-----------------------------------------------------------------------
      //这里的value1没有写完，应该加入list，每个卡片的值保持独立
      value1: null,
      colors: ["#99A9BF", "rgb(0, 207, 232)", "rgb(104, 213, 196)"],
      //这里是不同等级的颜色
      // 等同于 { 2: '#99A9BF', 4: { value: '#F7BA2A', excluded: true }, 5: '#FF9900' }
    };
```

### 步骤条

````vue
          <el-steps
            :active="i.how"
            finish-status="success"
            direction="vertical"
          >
            <el-step title="审核"></el-step>
            <el-step title="快递捡包"></el-step>
            <el-step title="运输"></el-step>
            <el-step title="到站检查"></el-step>
            <el-step title="入库"></el-step>
          </el-steps>
````

参数active控制到到达哪一步

- 可以没有按钮控制，呈现静止状态
- direction="vertical"控制横竖

### 弹框

````vue
<el-popover
  placement="right"
  width="400"
  trigger="click">
  <el-table :data="gridData">
    <el-table-column width="150" property="date" label="日期"></el-table-column>
    <el-table-column width="100" property="name" label="姓名"></el-table-column>
    <el-table-column width="300" property="address" label="地址"></el-table-column>
  </el-table>
  <el-button slot="reference">click 激活</el-button>
</el-popover>
````

#### 关键参数：

- #### placement，弹出方向

- trigger，触发方式

- slot="reference"：谁触发

### 走马灯（轮播图）:

````vue
     <el-carousel trigger="click" :interval="2000">
          <el-carousel-item
            v-for="item in imgarr"
            :key="item"
            :height="'800px'"
          >
            <!-- <h3 class="small">{{ item }}</h3> -->
            <img :src="item" alt="" height="920px" />
          </el-carousel-item>
        </el-carousel>
````

- imgarr是中间图片的循环的list

#### 重要的css控制：

- 整体大小的控制

  ```scss
    .el-carousel{
          position: absolute;
          margin-top: 8px;
          z-index: -1;
          .el-carousel__container{
              width: 1920px;
              height: 920px;
              img{
                  filter: blur(15px);
                  width: 100%;
  
                  height: 920px;
              }
          }
      }
  ```

  -  z-index: -1;是当做背景的写法1

### 表格为空的校验技巧

在官方文档下面没有找到空白校验

自己写了一个

````js
 if (String(this.$refs.element.$el.innerText).indexOf("暂无数据") != -1) {
        this.$message({
          message: "如果没有结果，请尝试细化地址，或者该地区没有救助站",
          type: "warning",
        });
      }
````

#### 原理

- $refs.element.$el.innerText是组件的全部文字
- $refs.element.$el还有很多信息，可以自行log
- indexOf函数是检索字符串中是否有xx内容，返回第一个位置，没有就是-1
- if的检测就是所有字符当中，没有“暂无数据”这个字符



