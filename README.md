# 千牛PC 换肤方案说明

## 准备工作
### 先要建立需要的文件，下面是建议的文档结构：
* index.html  千牛界面的页面文件
* base.css    页面的基本样式，不包括需要根据主题修改颜色的部分
* theme.less  页面的主题文件，里面最好只包含颜色信息

### 各个文件的内容
#### index.html
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

#### base.css
没有什么特别的

#### theme.less
主题文件，采用less的语法，具体的语法可以从 (less中国官网)[www.lesscss.net] 上学习。
如果不熟悉less，可以完全按照css的写法写，只是将需要用到颜色的地方用变量代替
theme.less 文件示例如下：




