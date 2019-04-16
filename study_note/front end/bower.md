@[TOC]()

# Bower 学习总结

[原文地址，有删改](http://javascript.ruanyifeng.com/tool/bower.html)

## 什么是bower

> Bower是一个客户端技术的软件包管理器，它可用于搜索、安装和卸载如JavaScript、HTML、CSS之类的网络资源。其他一些建立在Bower基础之上的开发工具，如YeoMan和Grunt

## 准备工作

1. 安装`node`环境:[node.js](http://nodejs.org/)
2. 安装`Git`，bower从远程git仓库获取代码包:[git简易指南](http://www.bootcss.com/p/git-guide/)

## 安装bower

使用npm，打开终端，输入：

```javascript
npm install -g bower   //其中`-g`命令表示全局安装
```

### bower初始化

命令行进入项目目录中，输入命令如下：

```javascript
bower init
```

会提示你输入一些基本信息，根据提示按回车或者空格即可，然后会生成一个`bower.json`文件，用来保存该项目的配置，如下：

```javascript
{
  "name": "bb_boot",
  "version": "0.0.1",
  "authors": [
    "savokiss <jaynaruto@qq.com>"
  ],
  "moduleType": [
    "amd"
  ],
  "license": "MIT",
  "ignore": [
    "**/.*",
    "node_modules",
    "bower_components",
    "js/lib",
    "test",
    "tests"
  ],
  "dependencies": {
  }
}
```

### 包的安装

bower install命令用于安装某个库，需要指明库的名字。

```javascript
bower install jquery --save
```

然后`bower`就会从远程下载`jquery`最新版本到你的`js/lib`目录下
其中`--save`参数是保存配置到你的`bower.json`，你会发现`bower.json`文件已经多了一行：

```javascript
  "dependencies": {
    "jquery": "~2.1.4"
  }
```

Bower会使用库的名字，去在线索引中搜索该库的网址。某些情况下，如果一个库很新（或者你不想使用默认网址），可能需要我们手动指定该库的网址。

```javascript
bower install git://github.com/documentcloud/backbone.git
 
bower install http://cdnjs.cloudflare.com/ajax/libs/backbone.js/1.0.0/backbone-min.js

bower install ./some/path/relative/to/this/directory/backbone.js
```

上面的命令说明，指定的网址可以是github地址、http网址、本地文件。

默认情况下，会安装该库的最新版本，但是也可以手动指定版本号。

```javascript
bower install jquery-ui#1.10.1   //指定安装jquery-ui的1.10.1版。
```

如果某个库依赖另一个库，安装时默认将所依赖的库一起安装。比如，jquery-ui依赖jquery，安装时会连jquery一起安装;

安装后的库默认存放在项目的bower_components子目录，如果要指定其他位置，可在.bowerrc文件的directory属性设置。

### 库的搜索和查看

bower search命令用于使用关键字，从在线索引中搜索相关库。

~~~javascript
bower search jquery
~~~



bower info命令用于查看某个库的详细信息。

```javascript
bower info jquery 
```

查看结果会列出该库的依赖关系（dependencies），以及可以得到的版本（Available versions）。

会看到`jquery`的`bower.json`的信息，和可用的版本信息

可以看到`jquery`最新的兼容版版本为`1.11.3`

### 包的更新和卸载

上面安装的是最新版的高版本`jquery`，假如想要兼容低版本浏览器的呢？
已经查到兼容低版本浏览器的`jquery`版本为`1.11.3`，下面直接修改`bower.json`文件中的`jquery`版本号如下：

```javascript
  "dependencies": {
    "jquery": "~1.11.3"
  }
```

然后执行如下命令：

```javascript
bower update  //如果不给出库名，则更新所有库。
```

卸载包可以使用uninstall 命令：

~~~javascript
bower uninstall jquery
~~~

注意，默认情况下会连所依赖的库一起卸载。比如，jquery-ui依赖jquery，卸载时会连jquery一起卸载，除非还有别的库依赖jquery。

### 列出所有库

bower list 或 bower ls命令，用于列出项目所使用的所有库。

```javascript
Bower list
Bower ls
```

### 配置文件.bowerrc

项目根目录下（也可以放在用户的主目录下）的.bowerrc文件是Bower的配置文件，它大概像下面这样。

```javascript
{
  "directory" : "components",
  "json"      : "bower.json",
  "endpoint"  : "https://Bower.herokuapp.com",
  "searchpath"  : "",
  "shorthand_resolver" : ""
}
```

其中的属性含义如下。

- directory：存放库文件的子目录名。
- json：描述各个库的json文件名。
- endpoint：在线索引的网址，用来搜索各种库。
- searchpath：一个数组，储存备选的在线索引网址。如果某个库在endpoint中找不到，则继续搜索该属性指定的网址，通常用于放置某些不公开的库。
- shorthand_resolver：定义各个库名称简写形式。

### 库信息文件bower.json

bower.json文件存放在库的根目录下，用于保存项目的库信息，供项目安装时使用，以及Bower的在线索引读取。

下面是一个典型的bower.json文件。

~~~javascript
{
  "name": "app-name",
  "version": "0.1.0", 
  "main": ["path/to/app.html", "path/to/app.css", "path/to/app.js"],
  "ignore": [".jshintrc","**/*.txt"],
  "dependencies": {
    "sass-bootstrap": "~3.0.0",
    "modernizr": "~2.6.2",
    "jquery": "latests"
  },
  "devDependencies": {"qunit": ">1.11.0"}
}
~~~

在项目的根目录下，运行bower init命令，通过回答几个问题，就会自动生成bower.json文件。

```javascript
bower init
```

有了bower.json文件以后，就可以用bower install命令，一下子安装所有库。

```javascript
bower install
```

根据bower.json文件，还可以向Bower的在线索引提交你的库。

```javascript
bower register <my-package-name> <git-endpoint>
// 比如
bower register jquery git://github.com/jquery/jquery
```

注意，如果你的库与现有的库重名，就会提交失败。