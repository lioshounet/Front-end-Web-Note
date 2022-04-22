## 前端框架理论知识

VUE：占比80%左右，大项目，入侵性大，中国人的框架

React：占比10%-20%，中小项目，入侵性小



MVC模式：多次使用DOM

MVVM模式：虚拟DOM，一次打包操作



链接备注

vue：

```html
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```



学习目标/流程：![vue学习路线](D:\ZPan\HTMLcode\自助学习\2021大二下\vue\vue学习路线.png)



## 模板语法

### V指令

#### 插值

```html
    <div id="app">
        <h1> 减少DOM的操作，节约性能</h1>
        {{message}}
        {{info.school}}
        {{arr1[0]}}
    </div>
```

```javascript
var app = new Vue({
    // app会监听自己以实现数据实时更新
    el: "#app",
    // el俗称挂载点，核心内容是通过CSS选择器选择元素
    // 命中元素可以深入内部，子元素
    data: {
        message: "hi",
        info: {
            school: "yitong",
            mobile: "400-800"
        },
        arr1: ["app", "ass"],
        isShow: "true"
    }
})
```

#### v-html   &&   v-text   &&   v-cloak

```html
 <div id="app">
        <!-- v-text是一种vue指令，他可以覆盖全部文本 -->
        <!-- 并且支持字符串拼接 -->
        <!-- v-html 支持html的标签语法 -->
        <li v-text="arr1[1]+'2333'"></li>
        <li v-cloak>{{arr1[0]}}</li>
        <!-- v-cloak备胎属性 通过CSS的none解决网速闪烁问题 -->
</div>
```

```css
 [v-cloak] {
            display: none;
        }
```

```javascript
var app = new Vue({
    // app会监听自己以实现数据实时更新
    el: "#app",
    // el俗称挂载点，核心内容是通过CSS选择器选择元素
    // 命中元素可以深入内部，子元素
    data: {
        message: "hi",
        info: {
            school: "yitong",
            mobile: "400-800"
        },
        arr1: ["app", "ass"],
        isShow: "true"
    }
})
```

#### v-bind

```html
   <div id="vbind">
        <a v-bind:href="link1">连接测试 </a>
        <!-- vbind操作非CSS属性 -->
    </div>
```

```javascript
var vbind = new Vue({
    el: "#vbind",
    data: {
        link1: "https://cn.vuejs.org/v2/guide/syntax.html"
    },
    methods: {
        changecolor: function () {
            this.color1 = "black";
        }
    }
})
```



#### v-on

```html
    <div id="von">
        <p>显示测试</p>
        <input type="text" @click="logceshi('666666')">
        <br>
    </div>
```

```javascript
var von = new Vue({
    el: "#von",
    data: {
        isShow: "aaaaaaaaaaaaaa"
    },
    methods: {
        logceshi: function (p1) {
            alert(p1);
        }
    }
})
```

#### v-show

```html
  <div id="app">
        <button v-on:click='show'>v-on</button>
        <!-- v-on事件绑定 DOM简化   -->
        <br>
        <button @click='show'>v-on简写</button>
        <!-- v-on可以简写成@ -->

        <button v-show="isShow" @click='changeshow'>v-show</button>
        <!-- v-show 控制显示 -->
        <br>
    </div>
```

```javascript
var app = new Vue({
    // app会监听自己以实现数据实时更新
    el: "#app",
    // el俗称挂载点，核心内容是通过CSS选择器选择元素
    // 命中元素可以深入内部，子元素
    data: {
        message: "hi",
        info: {
            school: "yitong",
            mobile: "400-800"
        },
        arr1: ["app", "ass"],
        isShow: "true"
    },
    methods: {
        // 定义方法
        show: function () {
            // 访问自身的属性
            // 使用this
            alert(this.arr1[0]);
        },
        changeshow: function () {
            this.isShow = !this.isShow;
        }
    }
})
```



#### v-model

```html
    <div id="vmodel">
        <p>vmodel</p>
        <input type="text" v-model="ma">
        <p v-text="ma"></p>
    </div>
```

```javascript
var vmodel = new Vue({
    el: "#vmodel",
    data: {
        ma: "2333333"
    },
    methods: {
        logceshi: function (p1) {
            alert(p1);
        }
    }
})
```



#### v-if

```html
  <div id="viffor">
        <p>viffor测试</p>
        <p v-if="type==='xiaojiang'">2</p>
        <p v-else-if="type==='xiao'">3</p>
        <p v-else="type==='xia'">4</p>
        <!-- 这个结构的使用是控制标签渲染，show控制显示，而这个可以直接让其不渲染
        该ifelse结构必须使用（数量大于等于3）
        v-if
        v-else-if
        v-else-if
        v-else
        数量为2
        v-if
        v-else -->
        <br>
    </div>
```

```javascript
var viffor = new Vue({
    el: "#viffor",
    data: {
        type: "xia"
    },
    methods: {
        logceshi: function (p1) {
            alert(p1);
        }
    }
})
```



#### v-for

```html
    <div id="vfor">
        <button @click="del_li">添加</button>
        <li v-for="item in arr">
            vfor测试
        </li>
    </div>
```

```javascript
var vfor = new Vue({
    el: "#vfor",
    data: {
        arr: ["1", "2", "3"]
    },
    methods: {
        del_li: function () {
            this.arr.push("4");
        }
    }
})
```

#### 基础语法综合案例

学生案例

排除奇数年龄的学生



```html
  <div id="student">
        <ul>
            <li v-for="item,key in student" v-if="item.age%2==0">
                <p>{{key}}</p>
                <p>姓名:{{item.stu_name}}</p>
            </li>
        </ul>
    </div>
```

```javascript
  var stu = new Vue({
            el: "#student",
            data: {
                student: [
                    {
                        stu_name: "小明",
                        age: 16,
                        school: "清华",

                    },
                    {
                        stu_name: "小工",
                        age: 17,
                        school: "清大华",

                    },
                    {
                        stu_name: "小港",
                        age: 18,
                        school: "清小华",

                    },
                    {
                        stu_name: "小x",
                        age: 18,
                        school: "西南大学",

                    },
                    {
                        stu_name: "小桃",
                        age: 88,
                        school: "清小华",

                    },
                    {
                        stu_name: "清小华",
                        age: 10,
                        school: "清小华",

                    },
                ]
            }
        })
```

key的优化

VUE在没有key的时候

会直接修改标签里面的内容

而使用key的时候会重新生成标签

```html
     <li key="stu">
                <p v-once>{{key}}</p>
                <p>姓名:{{item.stu_name}}</p>
     </li>
```

tip：v-once使得值不可修改



## 生命周期

- 生命周期函数就是某时间点，vue实例会自动得执行的函数

- 以下全是生命周期函数，钩子函数，直接存放在实例中

  - beforeCreate:创造之前

  - Create:创造之后

  - beforeMount:即将挂载之前的函数

  - mounted:挂载之后的函数

  - beforeDestroy:销毁之前

    - Destroy函数：实例销毁函数

      ```javascript
      app.$destroy()
      ```

  - destroyed:销毁之后

  - beforeUpdata:数据改变时，重新渲染之前

  - updataed:数据改变时，重新渲染之后

## 钩子函数

## 计算属性

类似于 methods方法

- 区别

  - 一次计算进入缓存

  - 参数未发生改变不会再次计算

  - 性能更高

    ```html
     <div id="app">
            {{fullma}}
     </div>
    ```

    ```javascript
      var vm = new Vue({
                el: "#app",
                data: {
                    ma: "xia"
                },
                computed: {
                    fullma: function () {
                        return this.ma + "ba"
                    }
                }
    
            })
    ```

- get与set

  - 设置和返回值

    ```javascript
      var vm = new Vue({
                el: "#app",
                data: {
                    ma: "xia"
                },
                computed: {
                    fullma: {
                        get: function () {
                            return this.ma + "ba"
                        },
                        set: function (value) {
                            console.log(value);
                        }
                    }
                }
            })
    ```

    set的额外用法
    
    ```javascript
    //在console
    Vue.set(vm.family,"father","baba")
    ```
    
    

## 监听

类似于 methods方法

- 区别

  - 某一个元素改变触发函数

    ```html
     <div id="app">
            {{fma}}
        </div>
    
    ```

    ```javascript
      var vm = new Vue({
                el: "#app",
                data: {
                    ma: "xia",
                    ba: "ba",
                    fma: "233"
                },
                watch: {
                    ma: function () {
                        this.fma = this.ma + this.ba
                        console.log(this.fma);
                    },
                    ba: function () {
                        this.fma = this.ma + this.ba
                        console.log(this.fma);
                    }
                }
    
            })
    ```

## 组件

### 组件创建

- todoit：组件名称
- todoit：标签使用名称

```html
  <div id="zujian">
        <todoit></todoit>
  </div>
```

```javascript
Vue.component('todoit', {
    template: '<button>10</button>'
})

var zujian = new Vue({
    el: "#zujian",
})
```

**重点：组件申明在挂载之前**

### 绑定实例

- **一个组件的 `data` 选项必须是一个函数**，因此每个实例可以维护一份被返回对象的独立的拷贝

```html
  <div id="zujian">
        <todoit @click="nadd()"></todoit>
    </div>
```

```javascript
Vue.component('todoit', {
    data: function () {
        return {
            count = 0,
        }
    },
    template: '<button @click="cpp">{{count}}</button>',
    methods: {
        cpp: function () {
            this.count++
            // count++
        }
    }
})

var zujian = new Vue({
    el: "#zujian",
})

```

### is属性

- 使用组件之中，组件可能会出现在挂载点最近的地方
- 使用is可以解决
- is属性的附加标签记得匹配父子关系

```html
   <div id="app">

        <table>
            <tr is="row"></tr>
        </table>
    </div>
```

```javascript
  Vue.component('row', {
            template: '<tr><td>row</td></tr>'
        })
        var vm = new Vue({
            el: "#app",

        })
```

### ref属性

```html
  <div id="app">

        <table>
            <tr is="row" ref="one" @Change="handleChange"></tr>
        </table>
    </div>
```

- 组件配合ref调用方法
  - 使用ref属性，可以识别标签
  - methods方法同样可以在组件里面写
  - 方法调用四个步骤
    - Vue.component的methods中写空函数：this.$emit('change')
    - 组件本身写触发空函数
    - 在实例中写含有实际作用的方法
    - 在html中监听空函数，并且触发实际函数；即：@空函数=“实际函数”

```javascript
   Vue.component('row', {
            template: '<tr @click="Change"><td>{{con}}</td></tr>',

            data: function () {
                return {
                    con: '566666666'
                }
            },
            methods: {
                Change: function () {
                    // console.log(change);
                    this.$emit('change')
                }
            }
        })
        var vm = new Vue({
            el: "#app",
            methods: {
                handleChange: function () {
                    console.log(this.$refs.one)
                }
            }
        })
```

### props

- props特性

  ```html
     <div id="app">
          <conter :cont="1" @inc="handadd"></conter>
          <conter :cont="2" @inc="handadd"></conter>
          <conter :connt="2" @inc="handadd"></conter>
          <div>{{totle}}</div>
      </div>
  ```

  - 数据类型限制
    - 多种的：cont: [Number, String],
    - 单一的：type: Number,
  - 数据是否可以为空
    - required: true
  - 默认值
    -  default: 'defu',
  - validator：判断器(函数)
    - 代码中是判断字符长度是否大于5

  ```javascript
    Vue.component('conter', {
              props: {
                  cont: [Number, String],
                  connt: {
                      type: Number,
                      required: true,
                      default: 'defu',
                      validator: function (value) {
                          return (value.length > 5)
                      }
                  }
              },
              template: '<div @click="handclick">{{nmb}}</div>',
  
              data: function () {
                  return {
                      nmb: this.cont
                  }
              },
              methods: {
                  handclick: function () {
                      this.nmb = this.nmb + 2
                      this.$emit('inc', 2)
                  }
              }
          })
          var vm = new Vue({
              el: "#app",
              data: {
                  totle: 3
              },
              methods: {
                  handadd: function (step) {
                      this.totle += step
                  }
              }
          })
  ```

- props特性

  - 不使用props接受数据
    - 无法正常运行，也无法正常使用参数
    - 父组件的参数会直接插入子组件

- 直接使用方法

  - 两个步骤

    - 父组件@click.native
    - 在VM实例中写方法

    ```html
      <div id="app">
            <child @click.native="handclick"></child>
        </div>
    ```

    ```javascript
           Vue.component('child', {
                props: {
    
                },
                template: '<div>{{xiao}}</div>',
    
                data: function () {
                    return {
                        xiao: "xiaojiang"
                    }
                },
                methods: {
    
                }
            })
            var vm = new Vue({
                el: "#app",
                data: {
    
                },
                methods: {
                    handclick: function () {
                        alert("xioajing")
                        // this.$emit('inc')
                    }
                }
            })
    ```

### 赋值

- 组件的赋值分为父传子和子传父

  - 什么是父子？

    在HTML中的东西为父亲，利用VUE生成的标签是子

  - 父亲传子，子组件修改数据

    - 传递
      - 通过属性绑定的形式
      - 冒号是v指令，如果不实用，会导致只能加入字符串
      - props: ['cont']接收
    - 修改数据
      - VUE中有单向数据流用于保护父的对象不被修改
      - 在data中加入新的值，从而代替直接修改父的行为

    ```html
      <div id="app">
            <conter :cont="1"></conter>
    
      </div>
    ```

    ```javascript
       Vue.component('conter', {
                props: ['cont'],
                template: '<div @click="handclick">{{nmb}}</div>',
    
                data: function () {
                    return {
                        nmb: this.cont
                    }
                },
                methods: {
                    handclick: function () {
                        this.nmb++
                    }
                }
            })
            var vm = new Vue({
                el: "#app",
            })
    ```

  - 子亲传父

    - 通过$emit触发事件，携带参数
    - 参数的固定名称是step

    ```html
      <div id="app">
            <conter :cont="1" @inc="handadd"></conter>
            <conter :cont="2" @inc="handadd"></conter>
            <div>{{totle}}</div>
        </div>
    ```

    ```javascript
            Vue.component('conter', {
                props: ['cont'],
                template: '<div @click="handclick">{{nmb}}</div>',
    
                data: function () {
                    return {
                        nmb: this.cont
                    }
                },
                methods: {
                    handclick: function () {
                        this.nmb = this.nmb + 2
                        this.$emit('inc', 2)
                    }
                }
            })
            var vm = new Vue({
                el: "#app",
                data: {
                    totle: 3
                },
                methods: {
                    handadd: function (step) {
                        this.totle += step
                    }
                }
            })
    ```

  -  兄弟组件传值
  
    -  使用方法的名称如h1
  
    ```html
      <h1>
            BUS/总线/发布订阅模式/观察者模式
        </h1>
        <div id="app">
            <child content="lio"></child>
            <child content="shem"></child>
     </div>
    ```
  
    由于兄弟组件的结构一致
  
    所以会同时使用传值方法
  
    - 具体步骤
      - Vue.prototype.bus = new Vue()           bus实例
      - 子组件使用方法传出参数：this.bus.$emit('change', this.selfC)
      - 使用生命周期勾子函数
        - 使用bus自带的$on监控$emit，并接受参数
        - 触发相对应的事件
        - this的区分
      - 注意单向数据流，优化报错
  
    ```javascript
     Vue.prototype.bus = new Vue()
    
            Vue.component('child', {
                data: function () {
                    return {
                        selfC: this.content
                    }
                },
                props: {
                    content: String
                },
                template: '<div @click="handclick">{{selfC}}</div>',
    
    
                methods: {
                    handclick: function () {
                        this.bus.$emit('change', this.selfC)
                    }
                },
                mounted: function () {
                    var this_ = this
                    this.bus.$on('change', function (msg) {
                        this_.selfC = msg
                    })
                }
            })
            var vm = new Vue({
                el: "#app",
                data: {
    
                },
                methods: {
    
                }
            })
    ```
  

### 插槽


- 使用背景

  -  简化子组件的成分，可以用父组件的一部分去代替子组件
  - 基础使用

  ```html
  
      <h1>
          插槽基础
      </h1>
      <div id="app">
          <child content="lio">
  
          </child>
          <child content="shem">
              <p>jiang</p>
          </child>
      </div>
  ```

#### 使用规则

- 子组件中的<slot>标签，会一一对照并显示父组件中的内容
- 如果父组件内容缺少，那就会显示标签中的默认内容

  ```javascript
     Vue.component('child', {
              data: function () {
                  return {
                      selfC: this.content
                  }
              },
              props: {
                  content: String
              },
              template: "<div><slot>moren</slot></div>",
  
              methods: {
  
              },
  
          })
          var vm = new Vue({
              el: "#app",
              data: {
  
              },
              methods: {
  
              }
          })
  ```

#### 进阶使用

- name引用
- 步骤
  - 在父组件中使用slot属性
  - 在子组件中使用name属性
  - 两属性统一

```html
  <h1>
          插槽进阶
      </h1>
      <div id="app">
          <child content="lio">
              <div slot="header">小</div>
              <div slot="footer">shem</div>
          </child>
          <child content="shem">
  
          </child>
      </div>
```


```javascript

  Vue.component('child', {
              data: function () {
                  return {
                      selfC: this.content
                  }
              },
              props: {
                  content: String
              },
              template:
                  "<div>" +
                  "<slot name='header'>moren</slot>" +
                  "<slot name='footer'>moren</slot>" +
                  "</div>",
  
              methods: {
  
              },
  
          })
          var vm = new Vue({
              el: "#app",
              data: {
  
              },
              methods: {
  
              }
          })

```

### 动态组件

  - 巧妙运用is属性
  - 完成组件内容和结构的出现
  - 步骤
    - html的位子，使用component标签，绑定is属性
    - 实例添加切换函数

```html
  <h1>
        动态组件
    </h1>
    <div id="app">
        <component :is="type"></component>
        <button @click="hanclick">change</button>
    </div>

```


```javascript

  Vue.component('one', {

            template:
                "<div>" +
                "one" +
                "</div>",
        })

        Vue.component('two', {
            template:
                "<div>" +
                "two" +
                "</div>",
        })
        var vm = new Vue({
            el: "#app",
            data: {
                type: 'one'
            },
            methods: {
                hanclick: function () {
                    this.type = (this.type === 'one' ? 'two' : 'one');
                }
            }
        })

```

## 动画 

#### 基础使用

![VUE动画轴](D:\ZPan\HTMLcode\自助学习\2021大二下\vue\VUE动画轴.png)

- leave可替换成enter

- fade是动画名称

- 基本的理解

  -  <transition name="fade">标签嵌套，并且控制name
  - 通过class实现动画
  - 在不同的阶段，添加和去除class

- 粗糙的理解

  - 动画的控制分为三个点

    - 第一帧： .fade-enter
    - 最后一帧：.fade-enter-to
    - 过程：.fade-enter-active

  - 通过三个东西可以制作出一个完整的动画

    ```html
       <h1>
            动态组件
        </h1>
        <div id="app">
            <transition name="fade">
                <div v-if="show">comic</div>
            </transition>
            <button @click="handclick">切换</button>
    
        </div>
    ```

    ```css
       .fade-enter {
                opacity: 0;
            }
    
            .fade-enter-active {
                transition: opacity 1s;
            }
    
            .fade-leave-to {
                opacity: 0;
            }
    
            .fade-leave-active {
                transition: opacity 1s;
            }
    ```

    ```javascript
       var vm = new Vue({
                el: "#app",
                data: {
                    show: true
                },
                methods: {
                    handclick: function () {
                        this.show = !this.show
                    }
                }
            })
    ```

    