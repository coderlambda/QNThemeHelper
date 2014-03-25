# 千牛PC 换肤方案说明

## 准备工作
### 先要建立需要的文件，下面是建议的文档结构：
* index.html  千牛界面的页面文件
* base.css    页面的基本样式，不包括需要根据主题修改颜色的部分
* theme.less  页面的主题文件，里面最好只包含颜色信息

### 服务器的部署
如果您使用了cdn，资源文件存放的位置和页面的域不一样，则需要给你您的cdn服务器进行一些配置，在response header 里加上
`Access-Control-Allow-Origin:*`
以保证您的less文件可以通过ajax访问得到

## 各个文件的内容
### index.html
html里面需要引用到css，less文件以及支持换肤的js文件
页面示例如下：

    <!DOCTYPE html>
    <html>
    <head>
        <title></title>
        <link rel="stylesheet" href="base.css"/>
        <link rel="stylesheet/less" href="theme.less"/>
    </head>
    <body>
    <!-- your page content here ... -->
    <script type="text/javascript" src="http://g.tbcdn.cn/sj/qn/??jssdk.js,lib/js/less.js"></script>
    <script>
        window.onload = function(){
            QN.initTheme();
        }
    </script>
    </body>
    </html>

注意引用theme.less的方法，`<link rel="stylesheet/less" href="theme.less"/>` 这里后面的的less文件后缀名可以随意
js文件请从淘宝cdn上引用
在页面初始化过程中需要调用 QN.initTheme() 方法来初始化换肤功能

### base.css
没有什么特别的

### theme.less
主题文件，采用less的语法，具体的语法可以从 [less中国官网](www.lesscss.net) 上学习。
如果不熟悉less，可以完全按照css的写法写，只是将需要用到颜色的地方用变量代替

(二期改进)
在原有的 @color-base 变量基础上，生成了一系列的变量，命名分别为
@palette-color-1
@palette-color-2
...
@palette-color-25

![](http://gtms04.alicdn.com/tps/i4/T11MbnFy0cXXXAHRQe-1011-92.png)

分别是包括 @color-base 在内的亮度有亮到暗的一组颜色
使用这组颜色，可以配制出一直很亮或一直很暗的颜色，不用再使用 @base-color 为基础进行偏移。

另外，换肤机制提供了一个背景图的变量，如果有需要，可以灵活使用这张背景图
@theme-background-image

如果这些还不满足需求，还可以是用亮度调节函数
lightness(color, light) 例如:

    background-color : lightness(@color-base, 0.98);
    color: lightness(@color-base, 0.1);


theme.less 文件示例如下：

    @color-base: #ffffff;
    @palette-color-3: #ffffff;

    @color-sub-nav-background: lightness(@color-base, 0.98);
    @color-sub-tab-active: @palette-color-3;
    @theme-background-image: url(#);

    body {
        background: @theme-background-image @base-color 0px -30px no-repeat;
    }

    .page-header {
      background: rgba(255, 255, 255, 0.25);
    }

    .main-nav-tab {
      li {
        &:hover {
          background-color: rgba(255,255,255, 0.5);
        }

        &.active {
          background-color: rgba(0,0,0,0.3);
          color: white;
          box-shadow: inset 0 0 4px #666;
        }
      }
    }

    .content-area {
      background-color: @color-sub-nav-background;
      .content-wrap {
        border-left: 1px solid #f2f4f6;
        background-color: white;
      }
    }

    .sub-nav-tab {
      &:hover {
        background: lighten(@color-sub-tab-active, 4%);
      }

      &.active {
        background-color: @color-sub-tab-active;
      }
    }

先对less的变量做一点简单的讲解，上面的代码中 @开头的就是变量了
其中最重要的就是 `@color-base` 这个变量，在换肤功能中，我们会替换这个变量的值，达到换肤的效果
注意下面的一些变量定义：

       @color-base: #ffffff;
       @palette-color-3: #ffffff;

       @color-sub-nav-background: lightness(@color-base, 0.98);
       @color-sub-tab-active: @palette-color-3;
       @theme-background-image: url(#);

为了避免闪烁，建议都将变量的初始值设置为白色
除了直接使用 @color-base 、 @palette-color-x 之外还可以通过函数生成需要的颜色
目前可用的函数有less的全部标准函数（具体内容请查看less api），以及我们为了调颜色而添加的 lighten_hsv 、 darken_hsv 、 lightness 三个函数
后续还会根据需要添加一些必要的函数

在变量定义之后的less代码，定义了需要根据换肤设置来改变颜色的代码

这里有一个背景图的使用范例
效果是将页面的头和窗口的头融为一体，生成下图的效果：

![](http://gtms01.alicdn.com/tps/i1/T1LVvEFCJcXXbJCzQP-760-80.jpg)
图中红框内为页面内容

虽然可以直接将所有的样式都写在less里面，但是由于客户端编译less效率比较低，所以不建议这样做
最好是将于主题颜色无关的css代码和主题相关的less代码分开来写

## 可以使用的标准颜色
目前可使用的标准颜色有
@color-base
@palette-color-1
@palette-color-2
...
@palette-color-25

后续会加上几种参考颜色供用户使用

## 其它

刚写完的代码没弄到千牛的的话，可以在浏览器控制台里输入

    less.modifyVars({'@color-base':"#faba31"});

进行快速预览换肤



