



# Node.js初步学习

## 一、Node.js 简介

 Node.js® 是一个基于 [Chrome V8 引擎](https://v8.dev/) 的 JavaScript 运行时；

Node.js 是一个基于Chrome JavaScript 运行时建立的一个平台。

Node.js是一个事件驱动I/O服务端JavaScript环境，基于Google的V8引擎，V8引擎执行Javascript的速度非常快，性能非常好。

## 二、Node 安装

### 1、安装地址

+ https://nodejs.org/zh-cn/下载安装

### 2、查看安装是否成功

+ 在控制台 输入 **node -v** 查看版本

<img src="../../AppData/Roaming/Typora/typora-user-images/1586865916005.png" alt="1586865916005" style="zoom:50%;" />

+ 运行实例：直接输入1+2 得到结果3

<img src="../../AppData/Roaming/Typora/typora-user-images/1586865956472.png" alt="1586865956472" style="zoom: 50%;" />

### 三、创建Node.js第一个应用

### 1、Node.js 应用的模板组成

#### 1. 引入required模块：通过使用require指令来载入Node.js模块

```js
var http = require("http");
```

	#### 2.创建服务器

+ 使用 http.createServer() 方法创建服务器，并使用 listen 方法绑定 8888 端口。 函数通过 request, response 参数来接收和响应数据。

+ 创建一个**01.js**文件,

```js
var http = require("http");
http.createServer(function(request,response){
    // 发送 HTTP头部
    // HTTP状态值:200
    // 内容类型：text/plain
    response.writeHead(200,{'Content-Type':'text/plain'})
    // 发送响应数据：‘hello world!’
    response.end('hellos world!\n')
}).listen(8888)
console.log('Server running at http://127.0.0.1:8888/')
```

+ 在终端运行 **node .\01.js**

  ![1586938076949](../../AppData/Roaming/Typora/typora-user-images/1586938076949.png)

+ 在网页上输入网址：http://127.0.0.1:8888/

![1586938128564](../../AppData/Roaming/Typora/typora-user-images/1586938128564.png)

**分析Node.js 的 HTTP 服务器：**

- 第一行请求（require）Node.js 自带的 http 模块，并且把它赋值给 http 变量。
- 接下来我们调用 http 模块提供的函数： createServer 。这个函数会返回 一个对象，这个对象有一个叫做 listen 的方法，这个方法有一个数值参数， 指定这个 HTTP 服务器监听的端口号。



## 四、Node文件打开方式

### 1、NPM使用介绍

NPM是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题，常见的使用场景有以下几种：

- 允许用户从NPM服务器下载别人编写的第三方包到本地使用。
- 允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
- 允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。

### 2、查看版本是否安装成功

+ **npm -v**

![1586941124567](../../AppData/Roaming/Typora/typora-user-images/1586941124567.png)

### 3、查看已经安装的包

+ **npm list -g** 

<img src="../../AppData/Roaming/Typora/typora-user-images/1586941257508.png" alt="1586941257508" style="zoom:50%;" />

### 4、npm 安装包

	#### 1. 安装的两种方式

```js
npm install express -g # 全局安装
npm install express # 默认本地安装
```

#### 本地安装

- 1. 将安装包放在 ./node_modules 下（运行 npm 命令时所在的目录），如果没有 node_modules 目录，会在当前执行 npm 命令的目录下生成 node_modules 目录。
- 2. 可以通过 require() 来引入本地安装的包。

#### 全局安装

- 1. 将安装包放在 /usr/local 下或者你 node 的安装目录。
- 2. 可以直接在命令行里使用。

+ 例如安装 bootstrap

  + 如果bootstrap后不写版本号，则默认安装npm里面的

  

<img src="../../AppData/Roaming/Typora/typora-user-images/1586867050389.png" alt="1586867050389" style="zoom:50%;" />

+ 使用淘宝镜像  以**cnpm** 来安装模块

```js
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

<img src="../../AppData/Roaming/Typora/typora-user-images/1586867825359.png" alt="1586867825359" style="zoom:50%;" />

![1586867511397](../../AppData/Roaming/Typora/typora-user-images/1586867511397.png)

## 五、 创建模块

+ 创建模块，package.json 文件是必不可少的。我们可以使用 NPM 生成 package.json 文件，生成的文件包含了基本的结果   npm  init  =>package.json生成
+ **npm init -yes**   -yes是所有安装默认

<img src="../../AppData/Roaming/Typora/typora-user-images/1586867124858.png" alt="1586867124858" style="zoom:50%;" />![1586942331965](../../AppData/Roaming/Typora/typora-user-images/1586942331965.png)<img src="../../AppData/Roaming/Typora/typora-user-images/1586867124858.png" alt="1586867124858" style="zoom:50%;" />![1586942331965](../../AppData/Roaming/Typora/typora-user-images/1586942331965.png)

<img src="../../AppData/Roaming/Typora/typora-user-images/1586865088983.png" alt="1586865088983" style="zoom:50%;" />

## 六、打开一个Node项目

+ 点击项目按住shift 在powershell里面打开
+ npm（项目管理包）install 安装所需资源包

<img src="../../AppData/Roaming/Typora/typora-user-images/1586864964400.png" alt="1586864964400" style="zoom:50%;" />

+ npm run serve 在服务器上面运行

<img src="../../AppData/Roaming/Typora/typora-user-images/1586865018783.png" alt="1586865018783" style="zoom:50%;" />

+ 开始运行 进入输入端口号

