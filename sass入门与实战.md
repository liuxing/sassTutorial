# Sass入门与实战

> **Sass is the most mature, stable, and powerful professional grade CSS extension language in the world.**打开[Sass](http://sass-lang.com/)官网就可以看见这样一句话。这话一点儿都不谦虚:Sass世界上最成熟、最稳定、最强大的专业级CSS扩展语言！他如此自信想必是有各种自信的理由，下面我们就一起来了解了解Sass，看看它在吹牛还在真的如此厉害。

## css预处理器

学过[CSS](http://zh.wikipedia.org/wiki/%E5%B1%82%E5%8F%A0%E6%A0%B7%E5%BC%8F%E8%A1%A8)的人都知道，它不是一种编程语言。网页开发离不开它，但它有发展缓慢。自然而然就有想出了解决方案：**css预处理器**

CSS 预处理器是什么？一般来说，它们基于 CSS 扩展了一套属于自己的 DSL，来解决我们书写 CSS 时难以解决的问题：

- 语法不够强大，比如无法嵌套书写导致模块化开发中需要书写很多重复的选择器；
- 没有变量和合理的样式复用机制，使得逻辑上相关的属性值必须以字面量的形式重复输出，导致难以维护。

所以这就决定了 CSS 预处理器的主要目标：提供 CSS 缺失的样式层复用机制、减少冗余代码，提高样式代码的可维护性。这不是锦上添花，而恰恰是雪中送炭。

css预处理器已发展多年，处理Sass外，还有[less](http://lesscss.cn/), [Stylus](http://stylus-lang.com/)...

*除了css预处理器，css后处理器也很流行如[postcss](https://github.com/postcss/postcss)*

## 开始使用Sass

- 完全兼容 CSS3

- 在 CSS 语言基础上添加了扩展功能，比如变量、嵌套 (nesting)、混合 (mixin)
- 对颜色和其它值进行操作的{Sass::Script::Functions 函数}
- 函数库[控制指令](http://sass.bootcss.com/docs/sass-reference/#control_directives)之类的高级功能
- 良好的格式，可对输出格式进行定制
- [支持 Firebug](https://addons.mozilla.org/en-US/firefox/addon/103988)

### Sass的两种语法

Sass 有两种语法。 

- 第一种被称为 SCSS (Sassy CSS)，是一个 CSS3 语法的扩充版本，也就是说，所有符合 CSS3 语法的样式表也都是具有相同语法意义的 SCSS 文件。 
- 第二种比较老的语法成为缩排语法（或者就称为 "Sass"）， 提供了一种更简洁的 CSS 书写方式。 它不使用花括号，而是通过缩排的方式来表达选择符的嵌套层级，I而且也不使用分号，而是用换行符来分隔属性。

*一般使用Scss多余Sass，不过二者除了上述不同外，基本在无差别，也许你和我一样，有时候写写Python后会突然想用Sass，有时有钟情于Scss，这些都无关紧要*

### Sass的安装

由于Sass的发展，Sass有两种实现，ruby-sass与lib-sass，前者采用`ruby`实现，后者采用`C/C++`实现。

- ruby-sass

你需要先安装ruby，不过mac已经自带了。然后gem install安装

```bash
　gem install sass
```

在命令行中使用如下命令编译

```bash
sass [options] <input> [output]
```

*通常你会出错，哈哈哈，如出错请自行搜索解决*

- node-sass

不用说你也得先安装nodeJs，不过现在的前端你不安装个node都不好意意思说自己是前端。

```bash
npm install --save-dev node-sass
```

通常你还是会出错，哈哈哈，不过用cnpm代替npm就好* 

```bash
cnpm install --save-dev node-sass
```
在命令行中使用如下命令编译
```bash
node-sass [options] <input> [output]
```

node-sass还可以编辑nodejs 文件进行配置编译

SASS提供四个[编译风格](http://sass-lang.com/docs/yardoc/file.SASS_REFERENCE.html#output_style)的选项

- nested：嵌套缩进的css代码，它是默认值。
- expanded：没有缩进的、扩展的css代码。
- compact：简洁格式的css代码。
- compressed：压缩后的css代码。

最后推荐大家使用gulp来编译Sass。关于gulp这里不多讲。下面给出一个简单gulp配置文件来编译sass

```javascript
const gulp = require('gulp')
const sass = require('gulp-sass')

gulp.task('sass', function () {
    return gulp.src('./sass/**/*.scss')
        .pipe(sass().on('error', sass.logError))
        .pipe(gulp.dest('./css'))
})

gulp.task('sass:watch', function () {
    gulp.watch('./sass/**/*.scss', ['sass'])
})
```

具体参见[GitHub](https://github.com/ogilhinn/sassTutorial)

下面我们将快速学习sass的语法

## sass语法快速入门

### 基本语法

#### 1.变量

`Sass`让人们受益的一个重要特性就是它为`css`引入了变量。你可以把反复使用的`css`属性值 定义成变量，然后通过变量名来引用它们，而无需重复书写这一属性值。或者，对于仅使用过一 次的属性值，你可以赋予其一个易懂的变量名，让人一眼就知道这个属性值的用途。
```scss
//scss style
//-----------------------------------
$fontStack:    Helvetica, sans-serif;
$primaryColor: #333;

body {
  font-family: $fontStack;
  color: $primaryColor;
}
//css style
//-----------------------------------
body {
  font-family: Helvetica, sans-serif;
  color: #333;
}
```
#### 2.嵌套

sass可以进行选择器的嵌套，表示层级关系，看起来很优雅整齐。

```scss
//scss style
//-----------------------------------
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li { display: inline-block; }

  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}

//css style
//-----------------------------------
nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

nav li {
  display: inline-block;
}

nav a {
  display: block;
  padding: 6px 12px;
  text-decoration: none;
}
```

#### 3. 计算功能

SASS允许在代码中使用算式：

```scss
body {
  margin: (14px/2);
  top: 50px + 100px;
  right: 80 * 10%;
}
```

#### 4.颜色

sass中集成了大量的颜色函数，让变换颜色更加简单。

```scss
//scss style
//-----------------------------------
$linkColor: #08c;
a {
    text-decoration:none;
    color:$linkColor;
    &:hover{ // &是父选择器的标识符
      color:darken($linkColor,10%);
    }
}

//css style
//-----------------------------------
a {
  text-decoration: none;
  color: #0088cc;
}
a:hover {
  color: #006699;
}
```

`&`是父选择器的标识符

#### 5. 注释

Sass共有两种注释风格。

- 标准的CSS注释 /* comment */ ，会保留到编译后的文件。
- 单行注释 // comment，只保留在SASS源文件中，编译后被省略。

在/*后面加一个感叹号，表示这是"重要注释"。即使是压缩模式编译，也会保留这行注释，通常可以用于声明版权信息。

```scss
/*! 
我是个傲娇的注释，不会被省略
*/
```

### 代码的重用

#### 1. 导入

sass中如导入其他sass文件，最后编译为一个css文件，优于纯css的`@import`
- 文件的名称约定以下划线开头
- 以下划线卡头的文件名称不会被编译

```scss
//scss style
//-----------------------------------
// _reset.scss

html,
body,
ul,
ol {
   margin: 0;
  padding: 0;
}

//scss style
//-----------------------------------
// base.scss 

@import 'reset';

body {
  font-size: 100% Helvetica, sans-serif;
  background-color: #efefef;
}

//css style
//-----------------------------------
html, body, ul, ol {
  margin: 0;
  padding: 0;
}

body {
  background-color: #efefef;
  font-size: 100% Helvetica, sans-serif;
}
```

#### 2. 扩展/继承

sass可通过`@extend`来实现代码组合声明，使代码更加优越简洁。

```scss
//scss style
//-----------------------------------
.message {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

.success {
  @extend .message;
  border-color: green;
}

.error {
  @extend .message;
  border-color: red;
}

.warning {
  @extend .message;
  border-color: yellow;
}

//css style
//-----------------------------------
.message, .success, .error, .warning {
  border: 1px solid #cccccc;
  padding: 10px;
  color: #333;
}

.success {
  border-color: green;
}

.error {
  border-color: red;
}

.warning {
  border-color: yellow;
}
```

#### 3. mixin

sass中可用mixin定义一些代码片段，且可传参数，方便日后根据需求调用。比如说处理css3浏览器前缀*(更好的解决方案：[autoprefixer](https://github.com/postcss/autoprefixer))*

```scss
//scss style
//-----------------------------------
@mixin box-sizing ($sizing) {
    -webkit-box-sizing:$sizing;     
       -moz-box-sizing:$sizing;
            box-sizing:$sizing;
}
.box-border{
    border:1px solid #ccc;
    @include box-sizing(border-box);
}
//css style
//-----------------------------------
.box-border {
  border: 1px solid #ccc;
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
```

mixin的强大之处，在于可以指定参数和缺省值。

### 高级用法

*高级用法可以翻译为“用的少”，但威力很大“*

#### 1.条件语句

`@if`可一个条件单独使用，也可以和`@else`结合多条件使用

```scss
//scss style
//-------------------------------
$lte7: true;
$type: monster;
.ib{
    display:inline-block;
    @if $lte7 {
        *display:inline;
        *zoom:1;
    }
}
p {
  @if $type == ocean {
    color: blue;
  } @else if $type == matador {
    color: red;
  } @else if $type == monster {
    color: green;
  } @else {
    color: black;
  }
}

//css style
//-------------------------------
.ib{
    display:inline-block;
    *display:inline;
    *zoom:1;
}
p {
  color: green; 
}
```

#### 2.循环语句

SASS支持for循环：

for循环有两种形式，分别为：`@for $var from <start> through <end>`和`@for $var from <start> to <end>`。$i表示变量，start表示起始值，end表示结束值，这两个的区别是关键字through表示包括end这个数，而to则不包括end这个数。

```scss
@for $i from 1 to 10 {
  .border-#{$i} {
    border: #{$i}px solid blue;
  }
}
```
也支持while循环：

```scss
$i: 6;
@while $i > 0 {
  .item-#{$i} { width: 2em * $i; }
  $i: $i - 2;
}
```

each命令，作用与for类似：

语法为：`@each $var in <list or map>`。其中`$var`表示变量，而list和map表示list类型数据和map类型数据。sass 3.3.0新加入了多字段循环和map数据循环。

```scss
//scss style
//-------------------------------
$animal-list: puma, sea-slug, egret, salamander;
@each $animal in $animal-list {
  .#{$animal}-icon {
    background-image: url('/images/#{$animal}.png');
  }
}

//css style
//-------------------------------
.puma-icon {
  background-image: url('/images/puma.png'); 
}
.sea-slug-icon {
  background-image: url('/images/sea-slug.png'); 
}
.egret-icon {
  background-image: url('/images/egret.png'); 
}
.salamander-icon {
  background-image: url('/images/salamander.png'); 
}
```

了解了上面这些内容，基本上就算入门了，再去看看这些链接：

- [Sass官网](http://sass-lang.com/)
- [Sass中文网](https://www.sass.hk/)
- [w3cplus](http://www.w3cplus.com/sassguide/)
- [gulp中文网](http://www.gulpjs.com.cn/)

## 实战

传送门： [GitHub](https://github.com/ogilhinn/sassTutorial)