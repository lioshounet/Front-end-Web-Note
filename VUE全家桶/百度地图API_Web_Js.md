# 百度地图API

### vue-cli2   &&   Js 3.0



## AK的申请

百度官方的开发平台

https://lbsyun.baidu.com/apiconsole/center

应用管理

- 我的应用
  - 创建应用
    - 服务类型-----浏览器
    - 启用服务-----全选
    - 白名单--------*

## 官方文档

### vue-baidu-map的官方文档

- 多多少少带点草率

- github，可能要过墙
- https://dafrok.github.io/vue-baidu-map/#/zh/control/geolocation

### 百度的js文档

- https://lbsyun.baidu.com/index.php?title=jspopularGL



## 多种使用方式

API的使用包括以下方式

- JS（多版本）

- vue组件（已经停止更新，不是很完善）
- 安卓SDK
- IOS

本笔记主要记录   ***JS   3.0***    以及vue的相关使用

## 安装---vue-baidu-map

```bash
 npm install vue-baidu-map
```

### main.js写入

````js
import BaiduMap from 'vue-baidu-map'
import { BmlMarkerClusterer } from 'vue-baidu-map'

Vue.component('bml-marker-cluster', BmlMarkerClusterer)

Vue.use(BaiduMap, {
  // ak 是在百度地图开发者平台申请的密钥 详见 http://lbsyun.baidu.com/apiconsole/key */
  ak: 'qDVDRiXUGrhxna8baDytwuDY00j5rhAU'
})
````

## 在浏览器里面展示(HTML5)----Hello World

`````vue
    <baidu-map class="map" center="北京">
    </baidu-map>
`````

- 这个标签会显示一个地图
- 基本属性
  - center：中心位置    Point或者String    尽量使用字符串
  - :double-click-zoom="false"      禁止双击放大
  - :scroll-wheel-zoom="true"       允许滚轮放大
- 但配上一个css，不然会没有长度或者是宽度

````css
.map {
  width: 100%;
  height: 600px;
}
````

## VUE控件

### 定位按钮

```vue
 <bm-geolocation
        anchor="BMAP_ANCHOR_BOTTOM_RIGHT"
        :showAddressBar="true"
        :autoLocation="true">
</bm-geolocation>
```

- anchor : 控件停靠位置
- offset  :  控件偏移值
- autoLocation : 添加控件时是否进行定位

### 比例尺

```vue
 <bm-scale anchor="BMAP_ANCHOR_TOP_RIGHT"></bm-scale>
```

| 属性名 | 类型          | 默认值 | 描述         |
| :----- | :------------ | :----- | :----------- |
| anchor | ControlAnchor |        | 控件停靠位置 |
| offset | Size          |        | 控件偏移值   |

### 缩放和移动组件

```vue
 <bm-navigation anchor="BMAP_ANCHOR_TOP_RIGHT"></bm-navigation>
```

| 属性名            | 类型                  | 默认值 | 描述                 |
| :---------------- | :-------------------- | :----- | :------------------- |
| anchor            | ControlAnchor         |        | 控件停靠位置         |
| offset            | Size                  |        | 控件偏移值           |
| type              | NavigationControlType |        | 平移缩放控件的类型   |
| showZoomInfo      | Boolean               |        | 是否显示级别提示信息 |
| enableGeolocation | Boolean               | false  | 控件是否集成定位功能 |

### 城市列表

```vue
 <bm-city-list anchor="BMAP_ANCHOR_TOP_LEFT"></bm-city-list>
```

| 属性名 | 类型          | 默认值 | 描述         |
| :----- | :------------ | :----- | :----------- |
| anchor | ControlAnchor |        | 控件停靠位置 |
| offset | Size          |        | 控件偏移值   |

## VUE覆盖物

### 点

```vue
 <bm-marker :position="{ lng:116.404 , lat:39.915 }" 
            :dragging="true"
            animation="BMAP_ANIMATION_BOUNCE">
      <bm-label content="我爱北京天安门" 
                :labelStyle="{color: 'red', fontSize : '24px'}" 
                :offset="{width: -35, height: 30}"/>
    </bm-marker>
```

| 属性名    | 类型    | 默认值 | 描述                                    |
| :-------- | :------ | :----- | :-------------------------------------- |
| position  | Point   |        | 标注的位置                              |
| offset    | Size    |        | 标注的位置偏移值                        |
| icon      | Icon    |        | 标注所用的图标对象                      |
| massClear | Boolean | true   | 是否在调用map.clearOverlays清除此覆盖物 |
| dragging  | Boolean | false  | 是否启用拖拽                            |
| clicking  | Boolean | true   | 是否响应点击事件                        |
| label     | Label   |        | 为标注添加文本标注                      |
| animation | String  |        | 动画效果                                |
| zIndex    | Number  | 0      | 设置覆盖物的zIndex                      |

#### 点击出点，同时记录坐标等信息

```vue
 <bm-marker
        :position="my_position"
        v-show="bm_show"
        animation="BMAP_ANIMATION_BOUNCE"
      >
        <bm-label
          content="我的位置"
          :labelStyle="{ color: 'red', fontSize: '24px' }"
          :offset="{ width: -35, height: 30 }"
        />
      </bm-marker>
```

````js
 showposition(e) {
      // console.log(this.mainz);
      // 坐标获取
      console.log(e.Ag.lng);
      this.clicklng = String(e.Ag.lng);
      console.log(e.Ag.lat);
      this.clicklat = String(e.Ag.lat);

      //标记点
      this.bm_show = true;
      this.my_position.lng = String(e.Ag.lng);
      this.my_position.lat = String(e.Ag.lat);
    },
````



## 检索

#### 地区检索(基础使用)

````vue
 <bm-local-search
        :keyword="keyword"
        :panel="false"
        :auto-viewport="true"
        :location="location"
        @searchcomplete="searchcomplete($event)"
>
</bm-local-search>
````

data

```js
  location: "重庆",
  keyword: "救助站",
```

自带的信息表的屏蔽与利用

屏蔽语句：

```vue
:panel="false"
```

通过@searchcomplete触发事件

使得$event中的地点对象可以被读取

```js
searchcomplete(arr) {
      this.isShowPanel = true;
      console.log(arr.Ir);
      this.tableData = arr.Ir;
 },
```

## 个性化的多方位实现

### vue-baidu-map

1. 组件可以通过 `@ready`去操作bmap的dom对象

2. 标签实现

   `````html
   <baidu-map @ready="showBmap">  </baidu-map>
   `````

3. js函数实现

   ```````js
   //个性化实现    
   showBmap({ BMap, map }) {
         map.setMapStyle({ styleJson: mapStyleJson });
       },
   ```````

### echarts

如果把json直接以非变量的形式写入option，可以减少很多加载的bug

````js
      option = {
        backgroundColor: "transparent",
        title: {
          text: "全国新冠人数分布",
          left: "center",
          textStyle: {
            color: "#fff",
          },
        },
        tooltip: {
          trigger: "item",
        },
        bmap: {
          center: [108.114129, 34.550339],
          zoom: 6,
          roam: true,
          mapStyle: {
            styleJson: mapStyleJson,
          },
        },
````

