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
    <script type="text/javascript" src="http://g.tbcdn.cn/sj/qn/??jssdk,lib/js/less.js"></script>
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
theme.less 文件示例如下：

    @color-base: #ffffff;

    @color-title: @color-base;
    @color-head: lighten_hsv(@color-base, 20%);
    @color-foot: lighten_hsv(@color-base, 50%);

    @color-border: lighten_hsv(@color-base, 45%);

    @color-btn-border: lighten_hsv(@color-base, 30%);
    @color-btn-pirmary: lighten_hsv(@color-base, 20%);


    .title {
      background-color: @color-title;
      border-top-color: @color-border;
    }

    .head {
      background-color: @color-head;
    }

    .foot {
      background: @color-foot;
    }

    .btn {
      border: 1px solid @color-btn-border;
      background-color: rgba(255, 255, 255, 0.6);;

      &:hover {
        background-color:rgba(255, 255, 255, 0.8);;
      }

      &.btn-primary {
        border-color: @color-btn-pirmary;
      }
    }

先对less的变量做一点简单的讲解，上面的代码中 @开头的就是变量了
其中最重要的就是 `@color-base` 这个变量，在换肤功能中，我们会替换这个变量的值，达到换肤的效果
注意下面的一些变量定义：

    @color-title: @color-base;
    @color-head: lighten_hsv(@color-base, 20%);
    @color-foot: lighten_hsv(@color-base, 50%);

    @color-text: darken_hsv(@color-base, 80%);
    @color-border: lighten_hsv(@color-base, 45%);

    @color-btn-border: lighten_hsv(@color-base, 30%);
    @color-btn-pirmary: lighten_hsv(@color-base, 20%);

这些变量都是基于 `@color-base` 变量来生成的，其中用到了 `lighten_hsv` `darken_hsv` 函数
目前可用的函数有less的全部标准函数（具体内容请查看less api），以及我们为了调颜色而添加的 lighten_hsv 和 darken_hsv 两个函数
后续还会根据需要添加一些必要的函数

在变量定义之后的less代码，定义了需要根据换肤设置来改变颜色的代码

虽然可以直接将所有的样式都写在less里面，但是由于客户端编译less效率比较低，所以不建议这样做
最好是将于主题颜色无关的css代码和主题相关的less代码分开来写

## 可以使用的标准颜色
目前可使用的标准颜色有
@color-base

后续会加上几种参考颜色供用户使用

## 其它

刚写完的代码没弄到千牛的的话，可以在浏览器控制台里输入

    less.modifyVars({'@color-base':"#faba31"});

进行快速预览换肤



