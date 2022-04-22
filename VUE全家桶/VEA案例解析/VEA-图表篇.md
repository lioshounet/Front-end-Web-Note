# Echarts

## 基本信息

### 什么是Echarts 

Apache  写的图表js应用，也有TS写法，拥有非常牛逼的社区案例和十分简陋的文档

### 相关网址

https://echarts.apache.org/handbook/zh/get-started/

## 基础

### VUE基本使用

#### main.js

``````js
import echarts from 'echarts'
Vue.prototype.$echarts = echarts
``````

#### 导入

````js
import * as echarts from "echarts";
````

### 模板原理

使用二次封装的图表组件

使用props传值，等定义的的内容如下

- 宽高
- id 
- class

使用$el识别id

## 柱状图与折线图

#### 核心代码

``````js
     tryshowDraw() {
      var tryshow = echarts.init(document.getElementById("tryshow"));
      tryshow.setOption({
        title: {
          text: "新冠数据模拟示例",
        },
        tooltip: {
          trigger: "axis",
          // 坐标轴指示器配置项。
          axisPointer: {
            type: "cross", //指示器样试
          },
        },
        dataZoom: [//滚动条
          {
            show: true,
            height: 20,
            xAxisIndex: [0],
            bottom: 10,
            start: 10,
            end: 80,
            zoomLock: false,
            handleSize: "110%",
            type: "slider",
            handleStyle: {
              color: "#d3dee5",
            },
            textStyle: {
              color: "#fff",
            },
            moveOnMouseMove: false,
            borderColor: "#90979c",
          },
          {
            type: "inside",
            show: true,
            height: 15,
            start: 1,
            end: 35,
          },
        ],
        toolbox: {//工具栏。内置有导出图片，数据视图，动态类型切换，数据区域缩放，重置五个工具。
          feature: {// 动态类型切换,切换图表
            magicType: {
              type: ["line", "bar"],
            },//数据视图工具，可以展现当前图表所用的数据，编辑后可以动态更新。
            dataView: {
              show: 0, //是否显示 数据示图
              readOnly: false, //是否 直接可以编辑数据。
            }, //保存为图片的配置
            saveAsImage: {
              show: 0,
            },//配置项还原
            restore: { show: true },
          },
        },
        //图例组件展现了不同系列的标记(symbol)，颜色和名字。可以通过点击图例控制哪些系列不显示。
        legend: {
          data: ["境外输入", "累计死亡"], //对应的是name
        },
        xAxis: {
          //坐标轴类型
          type: "category",
          //配置类目名称。 可以逐一设置，具体看文档
          data: this.DataList,
        //配置 鼠标移入时显示的 阴影指示器'line' 直线指示器 'shadow' 阴影指示,器  'none' 无指示器
          axisPointer: {
            show: true, //show 必须写，开关的作用
            type: "shadow",
          },
        },
        yAxis: [
          {
            type: "value", //坐标轴类型
            name: "境外输入", //名称
            min: 2000, //最小值
            max: 6000, //最大值
            axisLabel: { //坐标轴刻度标签的相关设置。
              formatter: "{value}人", //字符串模板
            },
          },
        ], //系列列表。每个系列通过 type 决定自己的图表类型,一个对象表示一个列表
        series: [
          {
            name: "境外输入",
            type: "bar",
            data: this.NowConfirmList,
            itemStyle: { //图形的颜色。默认指向全局的option.color
              color: "#F60",
            },
          },
          {
            name: "累计死亡",
            type: "line",
            data: this.DeadList,
          },
        ],
      });
      //鼠标事件
      tryshow.on("click", function (params) {
        console.log(params);
      });
    },
``````

### 数据使用

#### ob：vue观察者

- 属性不能直接取


#### 数据使用和函数的绘制应该在一个作用域以内

`````js
 var this_ = this;
    axios({
      method: "GET",
      url: "http://localhost:3002/DayList",
    }).then((response) => {
      this_.DataList = response.data[0].date;
      this_.DeadList = response.data[0].dead;
      this_.NowConfirmList = response.data[0].nowConfirm;
      this_.confirmList = response.data[0].confirm;
      console.log(this_.confirmList);
      this_.tryshowDraw();
      this_.confirmShowDraw();
    });
`````

## 雷达图

案例

```````js
  RateChart() {
      var tryshow = echarts.init(document.getElementById("tryshow"));
      tryshow.setOption({
        tooltip: {
          trigger: "axis",
          axisPointer: {
            type: "shadow", 
          },
        },
        radar: {
          radius: "66%",
          center: ["50%", "42%"],
          splitNumber: 8,
          splitArea: {
            areaStyle: {
              color: [
                "rgba(127,95,132,.3)",
                "rgba(127,95,132,.3)",
                "rgba(127,95,132,.3)",
                "rgba(127,95,132,.3)",
                "rgb(69, 51, 71,0.3)",
                "rgb(69, 51, 71,0.3)",
                "rgb(69, 51, 71,0.3)",
                "rgb(69, 51, 71,0.3)",
              ],
              opacity: 1,
              shadowBlur: 45,
              shadowColor: "rgba(0,0,0,.5)",
              shadowOffsetX: 0,
              shadowOffsetY: 15,
            },
          },
          indicator: [
            { name: "华北地区", max: 2 },
            { name: "东北地区", max: 2 },
            { name: "华东地区", max: 2 },
            { name: "中南地区", max: 2 },
            { name: "西南地区", max: 2 },
            { name: "西北地区", max: 2 },
          ],
        },
        color: ["#32dadd", "#63c2ff", "#c8b2f4"],
        legend: {
          left: "center",
          bottom: "10",
          data: ["死亡率", "治疗率"],
        },
        series: [
          {
            type: "radar",
            symbolSize: 0,
            areaStyle: {
              normal: {
                shadowBlur: 13,
                shadowColor: "rgba(0,0,0,.2)",
                shadowOffsetX: 0,
                shadowOffsetY: 10,
                opacity: 1,
              },
            },
            data: [
              {
                value: [
                  this.ChinaArea.HuaBei.deadRate / 5,
                  this.ChinaArea.DongBei.deadRate / 7,
                  this.ChinaArea.HuaDong.deadRate / 8,
                  this.ChinaArea.ZhongNan.deadRate / 6,
                  this.ChinaArea.XiNan.deadRate / 5,
                  this.ChinaArea.XiBei.deadRate / 5,
                ],
                name: "死亡率",
                areaStyle: {
                  normal: {
                    type: "default",
                    color: "#32dadd", // 选择区域颜色
                  },
                },
              },
              {
                value: [
                  this.ChinaArea.HuaBei.healRate / 500,
                  this.ChinaArea.DongBei.healRate / 700,
                  this.ChinaArea.HuaDong.healRate / 800,
                  this.ChinaArea.ZhongNan.healRate / 600,
                  this.ChinaArea.XiNan.healRate / 500,
                  this.ChinaArea.XiBei.healRate / 500,
                ],
                name: "治疗率",
                areaStyle: {
                  normal: {
                    type: "default",
                    color: "#63c2ff", // 选择区域颜色
                  },
                },
              },
            ],
            animationDuration: 1200,//时间
          },
        ],
      });
    },
```````

## 饼图

代码案例

`````js
 distributionChart() {
      var distribution = echarts.init(document.getElementById("distribution"));
      distribution.setOption({
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b} : {c} ({d}%)",
        },
        legend: {
          left: "center",
          bottom: "10",
          data: ["华北", "东北"],
        },
        series: [
          {
            name: "中国疫情分布",
            type: "pie",
            roseType: "radius",
            radius: [15, 95],
            center: ["50%", "38%"],
            data: [
              {
                value: this.ChinaArea.HuaBei.nowConfirm,
                name: "华北",
                itemStyle: { color: "#2ec7c9" },
              },
              {
                value: this.ChinaArea.DongBei.nowConfirm,
                name: "东北",
                itemStyle: { color: "#b6a2de" },
              }
            ],
            animationEasing: "cubicInOut",
            animationDuration: 2600,  //时间
          },
        ],
      });
    },
`````

## Echarts地图与资源

### 地图类的图表是进阶内容

- 进阶内容学习参考手册
  - 社区案例资源：https://echarts.apache.org/examples/zh/index.html
  - API文档：https://echarts.apache.org/zh/api.html

### 原因如下

- 地图本身就分很多种画法，多种写法之间本就是不同的东西

- 可能会用到百度地图API，一生之敌
- 可能是json绘图，庞大的json和资源的难找是一个方面
- 地名的不同叫法，一字之差就是显示失败
- 小国家会tm改名，甚至会tm分裂，如果是N年之前的世界json，做数据匹配会很难受
- 涉及台湾问题，就要自己去改源码json

### 优秀地图资源

#### 阿里：https://datav.aliyun.com/portal/school/atlas/area_selector

### highcharts：https://www.highcharts.com.cn/

- 世界地图：https://code.highcharts.com.cn/mapdata/

### 百度地图+散点案例

`````js
      var chartDom = document.getElementById("demomap");
      var myChart = echarts.init(chartDom);
      var option;

      var data = this.ChinaData;
      var geoCoordMap = {
        散点地点: [经纬度], 
          //.....
      };
      var convertData = function (data) {
        var res = [];
        for (var i = 0; i < data.length; i++) {
          var geoCoord = geoCoordMap[data[i].name];
          if (geoCoord) {
            res.push({
              name: data[i].name,
              value: geoCoord.concat(data[i].nowConfirm),
            });
          }
        }
        return res;
      };

      function renderItem(params, api) {
        var color = api.visual("color");
        return {
          type: "polygon",

          style: api.style({
            fill: color,
            stroke: echarts.color.lift(color),
          }),
        };
      }
      option = {
        backgroundColor: "transparent",
        title: {
          text: "全国新冠分布-demo",
          left: "center",
          textStyle: {
            color: "#fff",
          },
        },
        tooltip: {
          trigger: "item",
        },
        bmap: {
          center: [104.114129, 37.550339],
          zoom: 5,
          roam: true,
       //mapStyle: 过长省略了 ，可以到Echarts案例查看
        series: [
          {
            name: "感染现有人数",
            type: "scatter",
            coordinateSystem: "bmap",
            data: convertData(data),
            encode: {
              value: 2,
            },
            symbolSize: function (val) {
              return Math.pow(val[2], 27 / 100) * 10;
            },
            label: {
              formatter: "{b}",
              position: "right",
            },
            itemStyle: {
              color: "#ddb926",
            },
            emphasis: {
              label: {
                show: true,
              },
            },
          },
        ],
      };
      option && myChart.setOption(option);
    },
`````

### 中国地图无坐标生成

`````js
    Chinamap() {
      var chartDom = document.getElementById("demomap");
      var myChart = echarts.init(chartDom);
      myChart.showLoading();
      myChart.hideLoading();
      var chinaJson = {} //此处json应该有上万行
      echarts.registerMap("China", chinaJson, {});
      //json数据转化
      var ReadData = this.ChinaData;
      var data = [];
      for (var i = 0; i < 34; i++) {
        var datali = { name: "name", value: "value" };
        datali.name = ReadData[i].name;
        datali.value = ReadData[i].confirm;
        data.push(datali);
      }

      data.sort(function (a, b) {
        return a.value - b.value;
      });
      const mapOption = {
        visualMap: {
          left: "right",
          min: 0,
          max: 2100,
          inRange: {
            color: [
              "#ffccd5","#ffb3c1","#ff8fa3","#ff758f","#ff4d6d",
              "#c9184a","#a4133c","#800f2f","#590d22",
            ],
          },
          text: ["High", "Low"],
          calculable: true,
        },
        tooltip: {
          trigger: "item",
          formatter: function (params) {
            var res = "";
            res += params["data"].name + "人数" + params["data"].value;
            return res;
          },
        },
        //鼠标放在地图上时，显示的内容及样式控制
        series: [
          {
            id: "population",
            type: "map",
            roam: true,
            map: "China",
            animationDurationUpdate: 1000,
            universalTransition: true,
            data: data,
            itemStyle: {
              normal: {
                borderColor: "#4c94b3",
                borderWidth: 0,
              },
            },
            scaleLimit: {
              //滚轮缩放的极限控制
              min: 1,
              max: 1,
            },
          },
        ],
      };
      const barOption = {
        xAxis: {
          type: "value",
          min: 0,
          max: 16700,
        },
        yAxis: {
          type: "category",
          axisLabel: {
            rotate: 30,
          },

          data: data.map(function (item) {
            return item.name;
          }),
        },
        series: {
          type: "bar",
          id: "population",
          color: "#ff4d6d",
          data: data.map(function (item) {
            return item.value;
          }),
          universalTransition: true,
        },
      };
      myChart.setOption(mapOption);
      myChart.on("click", function (params) {
        alert(params["data"].name);
        //此处写点击事件内容
      });
      //点击事件，此事件还可以用到柱状图等其他地图
    },
`````



## 模板适配问题

### 花裤衩在原有的模板，对element的兼容做出了一个解决方案

1. 单独封装了图表
2. 混入一个js使得可以调整尺寸
3. 监视器监视选项卡变量，触发4
4. 父组件调用子组件再调用js

- 并不好用

### 更简单的使用方案

1. 单独封装了图表
2. 选项卡绑定参数
3. v-if控制显示