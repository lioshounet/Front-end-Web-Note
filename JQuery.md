# jQuery

## 基本信息

### 什么是jQuery

一个js封装库，用于简化DOM操作

### jQuery网站

- 官网首页   https://jquery.com/

- 下载需要的js   https://jquery.com/download/
- 中文文档   https://jquery.cuishifeng.cn/

## HelloWorld

``````html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<script src="jq.js"></script>
<script>
    //入口函数
    $(function () {
        alert("5555")
        $('div').hide();
    });
</script>

<body>
    <div class="pp">
        123
    </div>
</body>

</html>
``````

### 入口函数

等同于加载之后的函数

````js
  window.onload(){
  }
````

## 关于jQuery和Dom对象

jquery对象：用JQ获取的对象

````js
 $('div').hide();
````

Dom：document.get获取的对象

```js
  var sss = document.getElementById("55");
```

两者的方法不能互用

### 对象互换

Dom转jQuery

````js
var sss = document.getElementById("55");
$('sss')//jQ对象
````

jQuery转Dom

```js
//伪数组 
$('div')[0].hide();
```

## jQueryAPI

### 选择器

`````js
$('div')//标签
$('.ll')//class
$('#ddd')//id
$('.kk .dd')//父子
$('div').parent().css("color", "pink");//父亲
$('div').siblings("button").css("color", "blue");//全部兄弟
`````

### 从零开始遍历元素

`````js
$('div:eq(2)').css("color", "pink");

$('div').eq(2).css("color", "pink");
`````

### CSS操作

对所有元素统一的操作是隐式迭代

````js
//$('div').css("属性","内容");
$('div').css("color", "pink");


$('div:eq(2)').css({
     width: 200,
});
````

### 事件

``````js
 $('div:eq(2)').click(function () 
    {
       $(this).hide();
    })
``````

### 元素判断/数组位置获取

````js
 $('div:eq(2)').hasClass("dtft")  // 0或者1

  $('div').click(function () {
            var pp = $(this).index();
            alert(pp);
        })
````

### class变化

`````css
   .pp {
        color: rgb(206, 50, 50);
    }
`````

````js
 $(function () {
        $('div').click(function () {
            $(this).toggleClass("pp");
             $(this).removeClass('pp');
            $(this).addClass('pp');
        })
     
    });
````

### 动画

隐藏动画案例，并调用回调函数

`````html
    <div class="hi">
        隐藏
    </div>

    <div class="ll">
        123
    </div>
`````

````js
   $('.hi').click(function () {
            $('.ll').hide('1000', function () {
                alert(1)
            });
        })
````

下拉动画

````js
  $(function () {
        $('.ll').hide();
        $('.hi').click(function () {
            //第一个参数是时间
            $('.ll').slideDown('1000', function () {
                alert(1)
            });
        })
    });
````

下拉切换

````js
  $('.hi').hover(function () {
            $('.ll').slideToggle('1000', function () {
                // alert(1)
            });
        })
````

- 解决动画排队的问题

  - `````js
       $('.hi').hover(function () {
                $('.ll').stop().slideToggle('1000', function () {
                    // alert(1)
                });
            })
    `````

### 其他效果

- #### 基本

  - `````js
    show([s],[e],[fn])
    hide([s,[e],[fn])
    toggle([s],[e],[fn])
    `````

- #### 滑动

  - ```js
    slideDown([s],[e],[fn])
    slideUp([s,[e],[fn]])
    slideToggle([s],[e],[fn])
    ```

  #### 淡入淡出

  - ```js
    fadeIn([s],[e],[fn])
    fadeOut([s],[e],[fn])
    fadeTo([[s],o,[e],[fn])
    fadeToggle([s],[e],[fn])
    ```

  自定义

  - ````js
      $('.sh').hover(function () {
                $('.ll').animate({
                    left: 200
                })
            });
    ````

### 获取对象的属性

- 固有属性

`````js
 alert($('.sh').prop('class'));
`````

- 自定义属性

````js
$('.sh').attr('index','dss');//属性修改
````

### 获取值

input取得里面的内容

````js
$('input').val();//获取
$('input').val("55");//修改
````

div

````js
$('div').html("xxx")//标签以内加入内容
$('div').text("xxx")//只获取文本
````

### 获取元素

- 变量元素对象，并赋予不同的颜色
- 涉及到对象转化

`````js
  var aee = ['red', 'blue', 'green']

        $('div').each(function (i, domEle) {
            console.log(i);
            console.log(domEle);
            $(domEle).css('color', aee[i]);
        })
`````

### 添加或者删除元素

````js
   //添加在最前面
   $('div').prepend(li);
   //添加在前面
   $('div').before(li);
   //添加在后面
   $('div').after(li);
   //去除子元素
   $('div').remove(li);
   //去除子元素
   $('div').empty(li);
````

### 尺寸

获取

- 纯width

```js
$('.ll').width();
$('.ll').width(300); //修改
```

- Height + padding

`````js
$('.ll').innerHeight();
`````

- Height + padding + border

````js
 $('.ll').outerWidth();
````

- Height + padding  + border +  margin  

````js
 $('.ll').outerWidth(ture);
````

### 位置

- 以文档为标准

```js
        $('.sh').offset({
            top: 200,
            left: 200,
        })
```

- 获取定位

```js
        $('.sh').position({
            top: 200,
            left: 200,
        })
```

- 被卷曲的大小

`````js
$('.ll').scrollTop();
`````

- 案例：电梯导航

  - ````js
     // 2. 点击电梯导航页面可以滚动到相应内容区域
        $(".fixedtool li").click(function() {
            flag = false;
            console.log($(this).index());
            // 当我们每次点击小li 就需要计算出页面要去往的位置 
            // 选出对应索引号的内容区的盒子 计算它的.offset().top
            var current = $(".floor .w").eq($(this).index()).offset().top;
            // 页面动画滚动效果
            $("body, html").stop().animate({
                scrollTop: current
            }, function() {
                flag = true;
            });
            // 点击之后，让当前的小li 添加current 类名 ，姐妹移除current类名
            $(this).addClass("current").siblings().removeClass();
        })
    ````

### 多个事件的绑定

````js
        $('.sh').on({
            click: function () {
                $(this).css({
                    //css
                })
            },
            moveTo: function () {
                $(this).css({
                    //css
                })
            }
        })


        $('.sh').on({
            'cilck',
            'li',
            click: function () {
                $(this).css({
                    //css
                })
            },
        
        })
````

### 事件解除

```js
   $('.sh').off('click')
```

### 事件对象

#### $event

内容太多，log就好

### 对象拷贝

``````js
        var ll = {}
        var lio = {
            id: 1,
        }
        $.extend(ll, lio)
``````

#### 深浅拷贝

区别

- 浅拷贝
  - 拷贝地址
  - 一动全动
- 深拷贝
  - 复制数据
  - 分开进行

### 多库共存

- ````js
  jQuery.each();
  ````

- ````js
  var su = $.noConflict();
  su.('span')
  ````

## jq插件

jq插件库：https://www.jq22.com/

jq之家：http://www.htmleaf.com/

- 多看demo
- cv 三件套

### 常用插件

懒加载

瀑布流

全屏滚动

## 相关框架及其衍生

### bootstrapJS





