# SASS:CSS蒸汽时代

## CSS预处理生态

### 常见的css预处理

- sass/scss
  - 2007年诞生
  - 通过不断地迭代，目前的社区和生态已经非常全面
  - Scss目前已经进化到全面兼容css的Scss
- less
  - 2009年诞生
  - 兼容css语法，后来被人拿去和Sass杂交，诞生了Scss
- stylus
  - 来自node社区，2010年诞生

### 野蛮生长

- css经过迭代，开始可以使用变量，但是一些逻辑语句还是没有

  ````css
  :root{
      --red:#00000
  }
  ````

- less和sass产生了更兼容的东西

- node自用的css预处理产生

### sass/scss的区别

- 两种语法，后者完全兼容css

## 不同环境的安装

- 不依赖编辑器
  1. node，node-sass
  2. node，dart-sass
  3. ruby，sass模块
  4. dart，sass模块

- 依赖编辑器
  - IDE:webstrom：插件
  - vscode：插件

## 正式学习

### 安装(管理员的cmd)

```bash
npm i -g node-sass

#可能的报错
gyp ERR! find VS **************************************************************
gyp ERR! find VS You need to install the latest version of Visual Studio
gyp ERR! find VS including the "Desktop development with C++" workload.
gyp ERR! find VS For more information consult the documentation at:
gyp ERR! find VS https://github.com/nodejs/node-gyp#on-windows
gyp ERR! find VS **************************************************************
#意思是，需要下载依赖，而且因为出错，依赖下载失败
#所以，github下载不下来，懂的都懂，建议直接cnpm
```

### 理解CSS和CSS预处理的关系，并投入生产

- sass更像一种工程文件，可以编辑这个工程文件，最后通过命令实现导出

  ```bash
  #node
  node-sass a.sass b.css
  ```

- vscode
  
  - 自动编译，生成css

### 语法

#### 变量

- 变量的申明以及使用

  ```scss
  $divcolor:rgb(238, 0, 0);
  div {
      color: $divcolor;
  }
  ```

- 跨作用域的使用

  ```scss
  $divcolor:rgb(238, 0, 0);
  div {
      $fffcolor:rgb(163, 83, 83) !global;
      color: $divcolor;
  }
  
  .ffff{
      color: $fffcolor;
  }
  ```

#### 数据类型

- 数字

  - 1，2.2，19，10px，5a

- 字符串

  - “sss”，sss

- 颜色

  - blue，#00000，rgb(238, 0, 0)

  - 一些特殊的写法

    ```scss
    $color1: lighten($color, 15%);
    $color3: saturate($color, 15%);
    $color4: desaturate($color, 15%);
    $color5: (green + red);
    ```

- 空

  ```scss
  $divc:null;
  ```

- 布尔

  ~~~scss
  $a: true;
  $b: false;
  
  // 注：只有自身是false和null才会返回false，其他一切都将返回true
  ~~~

- 数组

  三种不同的写法

  ~~~scss
  $list0: 1px 2px 5px 6px;
  $list1: 1px 2px, 5px 6px;
  $list2: (1px 2px) (5px 6px);
  ~~~

- maps

  - 相当于js的对象

    Maps必须被圆括号包围，可以映射任何类型键值对（任何类型，包括内嵌maps，不过不推荐这种内嵌方式）

    ~~~scss
    $map: ( 
      $key1: value1, 
      $key2: value2, 
      $key3: value3 
    )
    ~~~

- 查看数据类型的方法

  ```scss
  type-of($divc)
  ```

### 运算

### 1.数字运算符

SassScript 支持数字的加减乘除、取整等运算 (`+, -, *, /, %`)，如果必要会在不同单位间转换值

如果要保留运算符号，则应该使用插值语法

**不同的编译器会用不同的bug检查，node的更为严格，vscode更为宽松**

**下面写的是严格语法**

- `+(加号)`

  ~~~scss
  // 纯数字
  $add1: 1 + 2;	// 3
  $add2: 1 + 2px; // 3px
  $add3: 1px + 2; // 3px
  $add4: 1px + 2px;//3px
  
  // 纯字符串
  $add5: "a" + "b"; // "ab"
  $add6: "a" + b;	  // "ab"
  $add7: a + "b";	  // ab
  $add8: a + b;	  // ab
  
  // 数字和字符串
  //非规范使用
  $add9: 1 + a;	// 1a
  $adda: a + 1;	// a1
  $addb: "1" + a; // "1a"
  $addc: 1 + "a"; // "1a"
  $addd: "a" + 1; // "a1"
  $adde: a + "1"; // a1
  $addf: 1 + "1"; // "11"
  ~~~

  总结：
  a.纯数字：只要有单位，结果必有单位
  b.纯字符串：第一个字符串有无引号决定结果是否有引号
  c数字和字符串：第一位有引号，结果必为引号；第一位对应数字非数字且最后一位带有引号，则结果必为引号

- `-（减号）`

  ~~~scss
  $add1: 1 - 2;	// -1
  $add2: 1 - 2px; // -1px
  $add3: 1px - 2; // -1px
  $add4: 1px - 2px;//-1px
  
  $sub1: a - 1;  // a-1
  $sub2: 1 - a;  // 1-a
  $sub3: "a" - 1;// "a"-1
  $sub4: a - "1";// a-"1"
  ~~~

  ~~~scss
  // 总结：
  每个字段必须前部分为数字，且两个字段只能一个后部分是字符(因为此时后缀被当被单位看待了)。
  只要其中一个值首位不为数字的，结果就按顺序去除空格后拼接起来
  ~~~

- `*（乘号）`

  ~~~scss
  $num1: 1 * 2;    // 2
  $mul2: 1 * 2px;  // 2px
  $num3: 1px * 2;  // 2px
  $num4: 2px * 2px;// 编译不通过
  
  $num5: 1 * 2abc; // 2abc
  ~~~

  ~~~scss
  // 总结：
  每个字段必须前部分为数字，且两个字段只能一个后部分是字符(因为此时后缀被当被单位看待了)。其余编译不通过
  ~~~

- `/（除号）`

  ~~~scss
  // 总结：
  a.不会四舍五入，精确到小数点后5位
  b.每个字段必须前部分为数字，且当前者只是单纯数字无单位时，后者(除数)后部分不能有字符。其余结果就按顺序去除空格后拼接起来。
  (因为此时后缀被当被单位看待了)
  ~~~

- `%（余号）`

  ~~~scss
  // 总结：
  a.值与"%"之间必须要有空格，否则会被看做字符串
  ~~~

### 关系运算符

大前提：两端必须为`数字` 或 `前部分数字后部分字符`

返回值：`true` or `false`


- `>`

  ~~~scss
  $a: 1 > 2; // false
  ~~~

- `<`

  ~~~scss
  $a: 1 > 2; // true
  ~~~

- `>=`

  ~~~scss
  $a: 1 >= 2; // false
  ~~~

- `<=`

  ~~~scss
  $a: 1 <= 2; // true
  ~~~

  

### 相等运算符

作用范围：相等运算 `==, !=` 可用于所有数据类型

返回值：`true` or `false`

~~~scss
$a: 1 == 1px; // true
$b: "a" == a; // true
~~~

~~~scss
// 总结：
前部分为不带引号数字时，对比的仅仅是数字部分；反之，忽略引号，要求字符一一对应
~~~



### 布尔运算符

SassScript 支持布尔型的 `and` `or` 以及 `not` 运算。

~~~scss
$a: 1>0 and 0>=5; // fasle
~~~

~~~scss
// 总结：
值与"and"、"or"和"not"之间必须要有空格，否则会被看做字符串
~~~



### 颜色值运算

颜色值的运算是分段计算进行的，也就是分别计算红色，绿色，以及蓝色的值

- `颜色值与颜色值`

  ~~~scss
  p {
    color: #010203 + #040506;
  }
  
  // 计算 01 + 04 = 05 02 + 05 = 07 03 + 06 = 09，然后编译为
  // p {
    color: #050709; }
  ~~~

- `颜色值与数字`

  ~~~scss
  p {
    color: #010203 * 2;
  }
  
  // 计算 01 * 2 = 02 02 * 2 = 04 03 * 2 = 06，然后编译为
  // p {
    color: #020406; }
  ~~~

- `RGB和HSL`

  ~~~scss
  // 如果颜色值包含 alpha channel（rgba 或 hsla 两种颜色值），必须拥有相等的 alpha 值才能进行运算，因为算术运算不会作用于 alpha 值。
  
  p {
    color: rgba(255, 0, 0, 0.75) + rgba(0, 255, 0, 0.75);
  }
  
  // p {
    color: rgba(255, 255, 0, 0.75); }
  ~~~

### 运算优先级

0. `()`

1. `*`、`/`、`%`
2. `+`、`-`
3. `>` 、`<`、`>=`、`<=`

### 差值运用

##### 差值可以引用任何字符串，在选择的元素和属性中运用

```scss
$name: foo;
$attr: border;
p.#{$name} {
  #{$attr}-color: $name;
}

// 编译后：
p.foo {
  border-color: foo;
}
```

### 嵌套

更加有效的样式布局

```scss
$divcolor:rgb(238, 0, 0);
.yi{
    font-size: large;
    .er{
        background-color: $divcolor;
        .san{
            background-color: rgb(143, 174, 182);
        }
    }
}
```

```html
<body>
    <div class="yi">
        123
        <div class="er">
            abc
            <div class="san">wwe</div>
        </div>
    </div>
</body>
```

### 父选择器

最常见的用法是用&号去设定父元素

```scss
.yi{
    color: yellow;
    &:hover{
        background-color: rgb(143, 174, 182);
    }
}
```

### !default

优先级降低

```scss
$content: "First content";
$content: "Second content?" !default;
```

最后$content的值为First content



### 导入

@import

```scss
@import "foo.scss";
@import "foo";

//只能导入scss文件
```

### 继承

@extend

```scss
.error {
  border: 1px #f00;
  background-color: #fdd;
}
.seriousError {
  @extend .error;
    //.seriousError拥有.error的全部内容
  border-width: 3px;
}

```

### `!optional`

如果 `@extend` 失败会收到错误提示，比如，这样写

```scss
.seriousError {
  @extend .error;
  border-width: 3px;
}
```

.error不存在会导致报错，而使用`!optional`，类似python的try

```scss
.seriousError {
  @extend .error !optional;
  border-width: 3px;
}
```

### 三元运算

#### if

```scss
p{
    color:if(1-1==0,blue,yellow)
    //判断式正确执行第一个，反之第二个
}

p{
    color:blue
}
```

#### @if

```scss
p{
    @if $color == 0{
        border-width: 3px;
    } @else if{
        border-width: 4px;
    }
}
```

#### @for

表达式：`@for $var from <start> through <end>` 或 `@for $var from <start> to <end>`

through 和 to 的相同点与不同点：

- 相同点：两者均包含<start>的值
- 不同点：through包含<end>的值，但to不包含<end>的值

```scss
@for $i from 1 through 3 {
  .item-#{$i} { width: 2em * $i; }
}

// compile:
.item-1 {
  width: 2em; }
.item-2 {
  width: 4em; }
.item-3 {
  width: 6em; }
```

#### @while

*循环语句*

表达式：`@while expression`

`@while` 指令重复输出格式直到表达式返回结果为 `false`。这样可以实现比 `@for` 更复杂的循环，只是很少会用到

~~~scss
$i: 6;
@while $i > 0 {
  .item-#{$i} { width: 2em * $i; }
  $i: $i - 2;
}

// compile:
.item-6 {
  width: 12em; }
.item-4 {
  width: 8em; }
.item-2 {
  width: 4em; }
~~~

@each

这个指令的核心是结构体访问，遍历

例如列表，数组，map，二维数组

表达式：`$var in $vars`

`$vars` 只能是`Lists`或者`Maps`



- 一维列表

  ~~~scss
  @each $animal in puma, sea-slug, egret, salamander {
    .#{$animal}-icon {
      background-image: url('/images/#{$animal}.png');
    }
  }
  
  // compile:
  .puma-icon {
    background-image: url('/images/puma.png'); }
  .sea-slug-icon {
    background-image: url('/images/sea-slug.png'); }
  .egret-icon {
    background-image: url('/images/egret.png'); }
  .salamander-icon {
    background-image: url('/images/salamander.png'); }
  ~~~

- 二维列表

  ~~~scss
  @each $animal, $color, $cursor in (puma, black, default),
                                    (sea-slug, blue, pointer),
                                    (egret, white, move) {
    .#{$animal}-icon {
      background-image: url('/images/#{$animal}.png');
      border: 2px solid $color;
      cursor: $cursor;
    }
  }
  
  // compile:
  .puma-icon {
    background-image: url('/images/puma.png');
    border: 2px solid black;
    cursor: default; }
  .sea-slug-icon {
    background-image: url('/images/sea-slug.png');
    border: 2px solid blue;
    cursor: pointer; }
  .egret-icon {
    background-image: url('/images/egret.png');
    border: 2px solid white;
    cursor: move; }
  ~~~

- maps

  ~~~scss
  @each $header, $size in (h1: 2em, h2: 1.5em, h3: 1.2em) {
    #{$header} {
      font-size: $size;
    }
  }
  
  // compile:
  h1 {
    font-size: 2em; }
  h2 {
    font-size: 1.5em; }
  h3 {
    font-size: 1.2em; }
  ~~~


#### 定义和使用混合样式

- 无参数使用

```scss
@mixin large-text
{
    font: {
      family: Arial;
      size: 20px;
      weight: bold;
    }
    color: #ff0000;
}
p {
      @include large-text;
}
```

- 有参数使用

```scss
$shem:20px;
@mixin large-text ($shem)
{
    font: {
      family: Arial;
      size: $shem+20;
      weight: bold;
    }
    color: #ff0000;
}
p {
      @include large-text(20);
}
```

#### function(函数)

```scss
$color:(light:#000000,dark:#222222);
@function colorfun(@key){
    @return map-get($color,$key);
    // map-get:返回指定的属性
}

div{
    color:colorfun(dark);
}
```

