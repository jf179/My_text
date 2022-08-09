- 浏览器中的javascript运行环境
- 依赖于V8引擎和浏览器内置API(DOM,BOM,AJAX,Canvas,JS内置对象)

# NodeJs

- Node.js是一个基于谷歌Chrome 浏览器V8引擎的Javasctipt 运行环境
- 浏览器时JS的前端运行环境
- Node是JS的后端运行环境
  - 因此Node环境下无法调用DOM和BOM等浏览器内置API，包括ajax

# Node的作用

Node.js作为一个JS的运行环境，仅仅提供了基础功能和API，依赖这些东西催生了很多的框架和工具

- 基于Express /Koa2 / egg 框架，可以快速构建Web应用
- 基于Electron框架 可以构建跨平台的桌面应用
- 基于restify 框架可以快速构建API接口项目
- 可以读写和操作数据库，创建实用的命令行工具辅助前端开发 如etc



## Node.js学习路径

- JavaScript 学习路径：
  - 基础语法，浏览器API (DOM,BOM), JQuery,以及常用的第三方库，组件化，模块化，常用的布局和应用如(Tab栏切换，搜索，前端缓存技术，放大镜，BOM坐标系列应用，拖拽功能应用)，ajax,Promise,axios,Canvas
  - 数据结构，设计模式，函数式编程，插件
- Node.js学习路径：
  - JS基础，Node.js内置API模块(fs,path,http等), 第三方API模块(Express,mysql,Koa,egg等)
  - 此次学习使用Node版本12.16.1，如果目前版本使用有错再切换这个学习版本，没出错就不换
  - 查看Node 版本，node-v
- node终端快捷键
  - 上箭头 可以执行上一次命令
  - node 文件名开头字符 + Tab，可以补全路径，避免文件名称过长的输入麻烦
  - clear 清空终端 cls 简写形式也可以
  - cd .\切换文件夹路径



## Postman

- 调用post请求且携带请求体：选择Body 点击单选框raw, 利用 json格式设置请求体发给服务器
- 服务器使用req.body接收请求体数据(前提是设置了中间件)

# 模块



## fs文件系统模块

模块的使用都需要引入对应的包，这里笔记只记录重点，详细案例在02network-> 03-NodeJs内

- fs模块是官方提供的用来操作文件的模块，常用属性方法如下：更多参考官网文档
- fs.readFile()        读取指定文件中的内容
- fs.writeFile()        用来向指定文件中写入内容(注：只能写入文件不能写入路径)，且每次写入会覆盖之前内容

```js
const fs = require('fs'); 或者 import {readFile} from 'fs';

// 读取
fs.readFile('./1.txt','utf8',function(err,dataStr){
    console.log(err,'读取失败')
    console.log(dataStr,'读取成功')
})

// 写入
fs.writeFile('./1.txt','hello xjj','utf-8', function(err){
    if(err){
        return console.log('文件写入失败',+err.message);
    }else{
        console.log('写入成功');
    }
})
```



## path路径模块

- 官方提供的内置模块用于操作路径
- path.join(); 拼接路径
- path.basename();  获取路径中的文件名,
- pth.extname(); 获取路径扩展名(就是后缀名)

```js
const path = rquire('psth');
const fs = require('fs');

// 采用path模块进行路径拼接
fs.readFile(path.joion(__dirname,'./xxx'), 'utf-8',function(err,data){
    console.log('读取成功',data)
})

-----------------------------------------------------
cosnt ftpi = '/a/b/c/index.html'; // 定义一个路径
const pathr = path.basename(ftpi,'.html'); // 加入第二个参数表示去掉后缀名
console.log(pathr); // index

-------------------------------------------
 cosnt fpt = '/a/b/c/index.html';
const res = path.extname(fpt); // html
```



## 其他辅助

- exec();   正则方法 返回一个匹配成功的数组结果或null
- test();   正则方法 返回一个 布尔值 true/ false
- match();  正则方法 返回匹配到的字符



# 服务器概念

- IP地址：ip地址就是每台计算机的唯一地址，通常采用 点分十进制 表示(a.b.c.d)的形式，其中a.b.c.d独使0-255之间的十进制整数(如：192.168.1.1)
  - ping 跟上网址，可以查看对应网站的服务器地址
  - 本都服务器地址 127.0.0.1 代表自己电脑服务器的ip地址，使用localhost也可以
- 域名和域名服务器
  - IP地址不便于记忆，因此域名对应IP地址，使用 DNS解析即可
- 端口号
  - 类似现实中的门牌号，端口号不能被多个web服务占用



## HTTP模块

- http模块是官方提供的，用于创建web服务器的模块，通过http模块提供的http.createServer(); 方法能创建一个服务器，从而对外提供web资源



## web服务器

- 导入 http模块
- 创建web服务器实例 http.createServer():
- 监听端口
- 启动服务器 即可通过端口号访问

```js
const http = require('http');

const server = http.createServer();

// 监听端口
server.on('request',function(req,res){
    console.log('Somone visit web server')
})

// 启动服务器
server.listen(8080,function(){
 console.log('server runing ar http://127.0.0.1:8000');
})
```



## 响应客户端

- 向客户端获取请求对象和响应数据
- 解决乱码问题: res.setHeader() 调用该方法

```js
const http = request('http');
const server = http.createServer();

server.on('request',function(req,res){
    // 获取客户端req请求对象
    const url = req.url;
    const method = req.method;
    const str = `url is ${url}, method is ${method}`;
    
    // 解决中文乱码
    res.setHeader('Content-Type','text/html; charset=utf-8')
    // 响应数据给客户端
    res.end(str);
})
```



## 根据URL响应内容

根据不同的url响应不同的内容

- 获取请求的rul地址
- 设置默认的响应内容
- 判断用户请求的是否为/或/index.html首页

```js
const http = require('http');

const server = http.createServer();
// req 客户端请求对象，res响应客户端对象
server.on('request',function(req,res){
    const url = req.url;
    let content = '<h1>404</h1>';
    
    if(url === '/' || url === '/index.html'){
        content = '<h1>首页</h1>'
    }else if(url === '/about.html'){
        content = '<h1>关于页面</h1>';
    }
    
    // 告诉客户端以utf-8解码
    res.setHeader('Content-Type','text/html; charset=utf-8');
    res.end(content); // 响应内容给客户端
})
```



## 响应服务器内容

- 根据客户端请求对象 响应服务器内容

```js
const fs = require('fs');
const path = require('path');
const http = require('http');

const server = http.createServer();

server.on('request',function(req,res){
    const url = req.url;
    
    let fpath = '';
   
    // 拼接路径 便于用户搜索
    if(url === '/'){
        // index.html文件在服务器dome1目录下，这里先拼接路径，客户端就不需要多输入deme1了
        fpath = path.join(__dirname,'/dome1/index.html');
    }else{
        fpath = path.join(__dirname,'domo1',url);
    }
    
    // 映射服务器文件
    fs.readFile(fpath,function(err,dataStr){
        if(err){
            return res.end('404')
        }
      res.setHeader('Content-Typpe','text/html;charset=utf-8')
      res.end(dataStr);
    })
})

server.listen(8080,function(){
    console.log('启动成功:http://127.0.0.1:8080')
})
```



# 模块化

- node提供的模块化 commJS,  即: 每个模块内部的module变量代表当前模块
- 模块具有模块作用域，无法被外界访问到，但是可以向外提供访问权限(即暴露接口)，类似export default
  - node有module对象 用于提供向外暴露接口的功能
  - node中暴露接口方式：module.expprts {接口1，接口2} 或者exports
- 模块化就是对功能的封装和调用，便于对代码进行组合和拆分，从而有利于维护和开发

```js

```





## 加载模块

- Node中的模块又分为三大类
  - 内置模块 如 fs path http
  - 自定义模块： 用户创建 的js文件 都时自定义模块
  - 第三方模块： 由第三方平台或库提供，需要下载
- 加载模块的方式：
  - require();   
  - import{ }
  - 自定义模块的加载需要填写路径，内置模块直接写上模块名即可，第三方模块需要先下载再写入模块名
  - 使用require() 方法导入的模块永远以 module.exports或者exports指向的对象为准

```js
const custom = require('./demo/inde.js'); // 加载自定义模块

const fs = require('fs');  // 加载内置模块

const must = requrie('musts'); // 记载第三方模块，但是要先下载
```



```js
// A 模块
exports.username = 'xjj';

// 如果一个模块有2种暴露方式，那么module.exports 优先
module.exports = {
    name:'ldh',
    age:27
}

// B模块
const objUser = require('./A/index.js')
指向module对象: name:'ldh', age:27

------------
// A 模块
module.exports.username = 'xjj';
module.expore.age = 22; // 这里不会覆盖上面 而是同时生效

// B模块
const re = require('./A/index.js');
console.log(re); // username:'xjj',age:22

------------------------
exports = {
    name:'ldh',
    age:27
}

module.exports.username = 'ldh'
module.exports = exports;
同样 哪怕将exports对象挂载到module.exports暴露出去，只要后面有用module.exports暴露接口。那么后面的生效
```



# 包

- 第三方模块简称：包。 开源，免费
- 为什么需要包：因为Node内置的模块仅提供了一些底层API，因此需要更多丰富的第三方包来提高开发效率
  - 包也是基于Node开发，类似 JQery 与 JS的关系，
- 包的下载平台：https://www.npmjs.com/  用于查询，和下载方式
  - 包的下载来源：https://registry.npmjs.org  通过工具命令下载的资源独使这个服务器而来的
- 内部提高了丰富的资源包，和使用说明 下载方式



## Node-包的使用示例

- 时间格式化
- 指定包的版本号 如：npm i moment@2.22.2
- 包的版本规范：如2.22.2，  第一位2表示大的版本号，22表示小更新，2表示修复的BUG
- 包管理配置文件：npm init -y  自动生成包管理文件，开发成员共享代码时直接根据包管理文件直接 npm i就可以重新下载项目所需包
- 卸载包：npm uninstall moment 
- 开发环境：npm i moment -D, 表示安装的包只在开发环境下使用，会记录到devDependencies中

```js
npm i moment // 安装包
const moments = require('moment'); // 导入时间格式化包

const dates = moments().format('YYY-MM-DD HH:mm:ss'); // 调用格式化函数 设置参数
console.log(dates); // 2022-04-06 08:38:04
```



## 淘宝镜像

- 查看当前下包的镜像源：npm config get registry
- 切换到淘宝镜像：npm config set registry=https://registry.npm.taobao.org/
- 检查镜像源是否下载成功：npm config get registry



## 包的分类

- 项目包：被安装到node_modules目录中的包，都时项目包
- 项目包又分两类：
  - 开发依赖包：记录到 devDependencies 
  - 核心依赖包：记录到dependencies  开发和项目上线都会用到
- 开发依赖下载 npm i xxx -D,    这里大写D表示开发环境下使用的包
- npm i xxx  -g ;   全局包  会被安装到C盘， 卸载全局包：npm uninstall xxx -g
-  核心依赖下载：npm i xxx,     
- 如果向开发自己的包，详细以后需要时再学习



## 模块的加载机制

- 内置模块加载机制：使用require加载模块时--> 优先从缓存中加载
  - 使用require加载模块时：如果第三方模块与Node内置模块重名，内置模块优先，
- 使用 require() 加载自定义模块:  需以./标识路径，否则Node会当作内置或者第三方模块进行加载
- 第三方模块的加载机制：从当前模块的node_modules文件夹加载，如果没有就会跳到上一级目录查找
- 目录作为模块：在被加载的目录下加载package.json文件 查找main属性，作为require的加载入口
  - 如果目录没有package.json文件，或者main属性入口不存在无法解析，则会尝试加载目录下的index.js
  - 如果以上两步都失败了，则node.js会在终端打印错误信息 Error:Cannot find module 'xxx'

```js
// package.json 文件
{
    "main":"./a.js"
}
```



# Express框架

- Express本质就是一个NPM上的第三方包，提供了快速创建Web服务器的便捷方式。类似JS与jQuery的关系
- 专门用于快速创建web网站服务器或对外提供API接口的服务器



## 基本用法

- 安装：npm i express@4.17.1
- 导入express包---> 创建服务器实例---> 调用端口
  - 监听get请求：app.get('求url'，处理函数(请求对象，响应对象))
  - 监听POST请求：app.post('求url'，处理函数(请求对象，响应对象))

```js
const express = require('express'); // 导入express

const app = express();  // 创建web服务器实例

// 监听客户端请求 并响应内容给客户端
app.get('/user',function(req,res){
    // 调用res的send方法 响应一个对象给客户端
    res.send({
        name:"xjj",
        age: "20",
        sex:"男"
    })
 })

app.post('/user',function(req,res){
    res.send('请求成功')
})

// 启动
app.listen(8080,function(){
    console.log('端口启动成功:http://127.0.0.1:8080')
})
```



## 获取URL请求参数

- 通过请求对象req，的query方法可以获取到客户端携带的查询参数
  - query本身是一个空对象，用于获取客户端的请求参数
- req.body则可以获取post请求的请求体信息

```js
const express = rquire('express');
const app = express();

arr.get('/', function(req,res){
    console.log(req.query); // 可以获取客户端请求携带的参数
})
```



## 获取URL动态参数

- 通过req请求对象的params() 可以访问到URL中通过 ：单冒号匹配到的动态参数
- 动态参数可以是多个

```js
const express = require('express');
cont app = express();

// : 冒号是固定写法 id不是
app.get('/user/:id', function(req,res){
    console.log(req.params); // id:"1"
    res.send(req.params);
})

// 多个动态参数
app.get('/user/:id/:name', function(req,res){
    console.log(req.params); // id:1  name:xjj
    res.send(req.params)
})

// 客户端请求
http://127.0.0.1:8080/user/1
http:/127.0.0.1:8080/user/1/xjj
```



## 托管静态资源

- express.static();  express提供的函数，用于创建一个静态资源服务器，可以将指定目录下的图片css和js文件对外开放

```js
const express = require('express');
cosnt app = express();

// 通过app.use函数 传入express.static函数 暴露public静态资源目录给外界
app.use(express.static('./public')); 

app.listen(8080,function(){
    console.log('启动成功;http://127.0.0.1:8080')
})
```



## 托管多个静态资源

- express.static()函数会根据目录的添加顺序查找所需文件

```js
// 多次调用 app.use(express.static(目录路径)) 即可
app.use(express.static('./public'));
app.use(express.static('./files'))
```



## 挂载路径前缀

```js
app.use(express.static('./public')); // 未挂载路径前缀

// 挂载路径前缀 访问需要加上demo前缀，如 http://127.0.0.1:8080/demo/index.js
app.use('/public',express.static('./public')); 
```



## nodemon自动启动

- 工具，node代码修改后自动启动端口

```js
npm i -g nodemon // 全局安装
```



## Express路由

- 在Express中，路由指的是客户端的请求与服务器处理函数之间的映射关系
- 由三部分组成：请求类型，请求URL地址，处理函数
- 每次有客户端请求到达服务器后 需要先经过路由的匹配(匹配请求类型和URL)，匹配成功才调用处理函数

```js
// app.get 为请求类型，'/'为求url地址， 跟上处理函数
app.get('/',function(req,res){})
```



## 路由模块化

- 在app里面写路由有点臃肿

```js
// router.js
const express = require('express');
const router = express.Router();

// 路由请求
router.get('/user/list',function(req,res){
    coosole.log('GET 请求');
})

module.exports = router; // 暴露路由接口

// 测试.js
const express = require('express');
const app = express();

const router = require('./router.js');
// 注册路由 可以添加路由前缀, 这里api就是路由前缀
app.use('/api', router);

app.listen(8080,function(){
    console.log('启动')
})
```



## 中间件

- 指业务流程的中间处理环节，下一级的流入就是上一级的流出
- 当一个请求到达Express服务器之后，可以连续调用多个中间件，从而达到预处理的效果，最后再返回数据给客户端
- 中间件也是一个函数有三个参数 req,res,next,
  - next函数表示将流转关系转继到下一个中间件或路由
- 注意点：中间件一定要在路由之前定义，使用时一定要写上next(),使其流转到下一步

```js
const express = require('express');
const app = express();

let mw = function(req,res,next){
    console.log('一个简单的中间件');
    
    next(); // 流转给下一个中间件
}

 // 注册  中间件的函数也可以直接写在这里
app.use(mw); 

app.get('/',function(req,res){
    res.send('经过mw中间件，然后到这里啦哈哈')
})

app.listen(80,function(){
    console.log('启动;http://127.0.0.1')
})
```



## 中间件的作用

- 多个中间件之间，共享一份req和res, 基于这样特征，可以在上游中间件设置或自定义属性和方法，供下游中间件或路由使用
- 中间件可以定义多个，按照顺序依次执行，
  - app.use(中间件1)； app.use(中间件2)

```js
// 中间件的作用
const express = require('express');
const app = express();

const mw = function(req,res,next){
    const time = Date.now();
    req.newTime = time;
    next();
}

// 为了方便一般中间件函数直接写在app.use里面
app.use(mw);

app.get('/', function(req,res){
    res.send('中间件经过1：' + req.newTime);
})

app.get('/user',function(req,res){
    res.send('中间件经过2:' + req.newTime)
})

app.listen(80,function(){
    console.log('启动成功')
})
```



## 局部生效的中间件

- 不使用app.use()注册的中间件，就是局部生效的中间件
- 局部中间件同样也可以定义多个，根据需要作为参数放入指定路由函数即可
- 且同一个路由函数还可以放入多个局部中间件

```js
const express = require('express');
const app = express();

const mu1 = function(req,res,next){
    console.log('调用局部中间件1');
    next();
}

const mu2 = function(req,res,next){
    console.log('调用中间件2');
    next();
}

// 将中间件作为参数放入路由 其处理完会将结果交由路由函数
app.get('/',mu1,mu2,function(req,res){
    res.send('Home page');
})

// 这里没有放入中间件 该路由不会触发中间件
app.get('/user',function(req,res){
    res.send('Home User')
})

app.listen(80,function(){console.log('OK')})
```



## 中间件的分类

- 中间件分为5类
  - 应用级别的中间件:  指绑定到app.use(),实例上的中间件
  - 路由级别的中间件： 指绑定到express.Router()实例上的中间件如：router.use(中间件)
  - 错误级别的中间件：用于捕获错误，必须有4个参数，且必须是定义在所有路由之后
  - Express 内置的中间件
  - 第三方提供的中间件

```js
// 应用级别中间件
app.use(中间件函数); // 应用级别

// 路由级别中间媒介
const express = require('express');
const app = express();
const router = express.Router();

router.use(function(rew,res,next){
    console.log('Time');
    next();
})

app.use('/',router);
```



```js
// 错误级别中间件 防止项目崩溃
const express = require('express');
const app = express();

app.get('/',function(req,res){
    throw ne Error('抛出错误');
    res.send('Home page')
})

// 错误中间件必须有4个参数
app.use(function(error,req,res,next){
    console.log('发生了错误 捕获');
    res.send('Error:' + error.message); // Error:抛出错误
})

app.listen(80,function(){
    console.log('OK')
})
```



## Express内置中间件

Express内置了3个中间件

- express.static();  托管静态资源(html,css,js 图片等)
- express.json();  解析json格式的请求体，
- express.urlencoded();  只能解析application/x-www-form-urlencoded格式的表单数据

```js
const express = require('express');
const app = express();

// express.static()静态托管中间件
app.use('/api',express.static('./public')); 
// public表示服务器指定文件夹，托管后客户端可以访问该文件下的文件
```



```js
// 解析json格式请求体
// 调用express.json()内置中间件
app.use(express.json());
app.post('/useer',function(req,res){
    console.log(req.body); // 打印请求体数据
    res.send('OK')
})

app.listen(80,function(){console.log('OK')})
```



```js
// express.urlencoded格式请求
app.use(express.urlencoded({extended:false})); // 固定写法
app.post('/user',function(req,res){
    console.log(req.body); // req.body获取客户端请求
    res.send('OK')
})
```



- req.body可以获取客户端json格式请求体和x-www-form-urlencoded编码格式的数据



## 第三方中间件

- 安装：npm i body-parser
- 导入：require(xxx)
- 注册：app.use(xxx)

```js
const express = require('express');
const app = express();

// 导入第三方中间件插件(先下载) 将获取的客户端数据解析成对象
const parser = require('body-parser');

// 注册中间件
app.use(express.urlencoded({extended:false}))

app.post('/user',function(req,res){
    console.log(req.body);
    res.send('OK')
})
app.listen(80,function(){
    console.log('OK')
})
```



## 自定义中间件

- 自定义中间件监听req.data,和req.end事件，获取客户端请求数据
- Node内置模块querystring提供的parse()函数专门用于处理查询字符串，解析成对象
- 将解析出来的数据对象挂载为req.body，供下游中间件/路由使用

```js
const express = require('express');
const app = express();
const qs = require('querystring');

// 自定义中间件
app.use(function(req,res,next){
    // 声明一个变量用于接收客户端请求数据，防止请求数据过大切割多次接收,这里进行拼接
    let str = '';
    // 监听req.data事件
    req.on('data',function(chunk){
        str += chunk;
    })
    req.on('end',function(){
        // 调用query.parse函数将数据转换成对象
        const body = qs.parse(str);
        next();
    })
})

app.post('/user',function(req,res){
    // 下游中间件/路由可以拿到挂载的body因为是共享req的
    res.send(req.body);  // { name: 'zs', age: '20', sex: '男' }
})
```



## 自定义中间件模块化

- 将自定义的中间件模块化

```js
// 模块化 A
const qs = require('querystring');

const hanle = function(req,res,next){
    let str = '';
    req.on('data',function(context){
        str += context;
    })
    req.on('end',function(){
        req.body = qs.parse(str);
        next();
    })
}

// B 页面 调用模块
const express = require('express');
const app = express();

// 导入模块
const handles = require('./模块化A');
app.use(handles);

app.post('/user',function(req,res){
    res.send(req.body); // 
})
```



## 使用Express写接口

```js
// 路由模块
const express = require('express');
cosnt router = express.Router();

router.get('/get',function(req,res){
    const query = req.query;
    res.send({
        status:0,
        msg:'GET请求成功',
        data:query
    })
})

router.post('/post',function(req,res){
    const body = req.body;
    res.send({
        status:0,
        msg:'POST请求成功',
        data:body
    })
})

module.exports = router;
```



```js
// 接口页面
const express = require('exportss');
const app = express();

// 调用内置中间件处理POST请求数据格式
app.use(express.urlencoded({extended:false}))

// 导入路由模块
const router = require('./路由模块');

// 模块注册
app.use('/api',router);

app.listen(80,function(){
    console.log('ok')
})
```



## 接口跨域

- jsonp  只支持get请求
- cors 跨越资源共享，由后端进行配置， 推荐  (主流方案)
  - 安装：npm install cors 

```js
const cors = require('cors'); // 导入
app.use(cors()); // 注册cors
```



## 常用字段请求头

- Access-Control-Allow-Origin   允许跨域
- Access-Control-Allow-Header, 如果请求头不在九种默认模式下，可以自行声明
- Access-Control-Allow-Methods，允许设置除get post head请求之外的请求方式
- 简单请求的定义：
  - 请求方式包括：get,post,head,任意一种
  - http头部信息，不超出默认九种(没记，需要查文档)
- 预检请求：指浏览器向服务器发送预检OPTION 已获知服务器是否允许该实际请求
  - 请求方式：除get,post,dead 之外的请求方式
  - 请求头种包含自定义字段
  - 向服务器发送application/json格式的数据

```js
// cors响应头部字段 只允许来自http://itcast.cn的请求，如果要允许所有跨域请求 直接 * 星号
res.setHeader('Access-Control-Allow-Origin', 'http://itcast.cn')

// 声明额外的2个请求头(不常用)
res.setHeader('Access-Control-Allow-Header','Content-Type, X-Custom-Header')

// 默认情况下 cors只支持get post head请求，如果席伟发送其他请求可以设置methods, *星号则表示允许所有请求
res.setHeader('Access-Control-Allow-Methods','POST,GET,DELETE,HEAD')
```



# MySQL数据库

- 传统型数据库：Excel的数据组织结构，每个Excel中 数据的组织结构分别是工作簿，工作表，数据行，列四个部分
- 实际开发中，每个项目对应一个数据库，不同的数据要存储到不同的表中，如用户信息存储到user表格
- 每个表中具体存储那些很信息，由字段来决定，如可以为usser设计id,username,password这3个字段
- 表中的行，代表每一条的具体数据
- 安装MySQL：MySQL数据存储,  MySQL Workbench可视化管理工具(用于操作数据库数据)
  - D:\03-66期就业班学习资料\09-1-node+后台\17-前后端交互+数据库\前后端交互阶段资料新\day5（第7章2小节-4小节）\day5\素材\MySQL for Windows
  - 安装教程如上 密码账号admin123



## 基本使用

- 使用MySQL Workbench操作MySQL
- 切换到界面的Schemas
  - query1 编辑区
  - Action Qutput输出区
- 创建数据库：自定义命名
- 创建数据表 Tables --> 右键--> Create Table ->给表格命名-Comments备注表的信息

![](D:\05-code-word汇总\4-重学前端-文档笔记\js基础深入.assets\2.jpg)

- 设计表的字段和数据类型
  - Column Name: 点击直接设计字段如 id
  - Datatype:  数据类型
  - 每个字段都可以在Comments中进行备注
  - Default/Expressiion 默认值根据需要设置，如status状态可以给它默认为0表示用户状态正常
  - 设置完毕点击Apply保存，Tables下面就会字段生成一张设置好的表

![./](D:\05-code-word汇总\4-重学前端-文档笔记\js基础深入.assets\3.jpg)

![](D:\05-code-word汇总\4-重学前端-文档笔记\js基础深入.assets\4.jpg)



## 写入数据

- 右键点击users表->选择 Select Rows -limit 1000--> 在Result Grid编辑即可

![](D:\05-code-word汇总\4-重学前端-文档笔记\js基础深入.assets\5.jpg)



## SQL管理数据库

- sql是结构化查询语言，可以编程的方式操作数据库的数据
- 只能在关系型数据库使用SQL
- SQL作用：
  - 查询数据
  - 插入新的数据
  - 更新数据
  - 删除数据
  - 创建新的数据库和表

## select语句

- SELECT 语句用于查找数据，执行结果被存储在一个结果表中(点击保存 闪电符号执行)

```mysql
-- 通过星号查询指定表的所有数据
select * from users

-- 查询username和password数据
select username,password from users

-- 从users表格中查询username等于周杰伦这条数据
select * from users where username='周杰伦'
```

![](D:\05-code-word汇总\4-重学前端-文档笔记\js基础深入.assets\6.jpg)



## insert语句

- 用于向表格中插入数据

```mysql
-- 向users表中插入username和password
insert into users (username,password) values ('张学友', '098123')
```



## update语句

- 用于修改该表格数据,where 表示限定要修改那里

```mysql
-- 修改users表格中id为1的password密码,和状态
update users set password='888888',status=1 where id=1
```



## delete语句

- 用于删除表格数据.注意where 否则会删除整个表格

```mysql
-- 删除users表格中id为3的用户
delete from users where id=3
```



## where子句

- 用于限定选择标准由如下运算符号
  - = 等于
  - <> 不等于 简写！=
  - 大于 >
  - 小于<
  - 大于等于>=
  - 小于等于<=
  - between 某个范围内
  - like  搜索某种模式

```mysql
-- 从sers表中查询id不等于1的所有数据
select * from users where id!=1
```



## AND和OR运算符

- 可在where 语句中把两个或多个条件连接起来
  - AND表示必须同时满足多个条件，类似JS的 && 逻辑与
  - OR表示满足任意条件，类似JS的 || 逻辑或

```mysql
-- 从users表中查询 status等于0且id小于7的数据
select * from users where status=0 and id<7

-- 查询status等于1 或者 username等于周杰伦的数据
select * from users where status = 1 or username = '周杰伦'
```



order by语句

- 用于对苏韩剧进行排序 asc默认升序， desc降序

```mysql
-- 对users表的id进行排序 降序
select * from users order by id desc
```



- 多重排序，以逗号分割即可

```mysql
-- 对users的status进行降序排序 后 再按照字母升序排序
select * from users order by status desc, username asc
```



## count函数

- 用于统计总数

```mysql
-- 从users表中查询状态status等于1的数据的总数
select count(*) from users where status=1
```



## AS关键字

- 用于给查询结果设置别名，不会改变源数据

```mysql
-- 从users表中将状态status为1的查询结果设置total别名
select count(*) as total from users where status=1

-- 对username 进行别名设置
select username as myname from users
```



## 在项目中操作MySQL数据库

- 安装第三方模块  : npm install mysql -D  该模块提供了连接和操作mysql的功能
- 通过安装的模块进行连接数据库，
- query返回的是一个数组形式的查询结果

```js
// 导入模块
const mysql = require('mysql');
// 连接mysql
const db = mysql.createPool({
    host:'127.0.0.1',  // 数据库IP地址
    user:'root',   // 数据库账号
    password:'admin123', // 数据库密码
    database:'my_db_01', // 指定要操作的数据库名
})
// 成为是模块是否能正常工作
db.query('select 1',function(err,result){
    if(err){
        return console.log(err.message);
    }
    console.log(result)
})
```



```js
// 查询数据库users表中所有数据
db.query('select * from users',(err,result){
     if(err){
       return console.log(err.message)
    }
   console.log(result)
})

// 插入数据
const sqlStr = insert into user(username,password) values(?,?); // 如果不想直接赋值就问号先占位
// 第二个参数[]为占位赋值
db.query(sqlStr,['张学友','pdd123'])

// 便捷插入方式
const user = {username:'乔峰',password:'pjj123'}
const sqlStr = 'insert into users set ?'; // 便捷方式

db.query(sqlSql,user,(err,result){
     if(err){
        return console.log(err.message)
   }
if(result.affectedRows === 1){
    console.log('OK')
   }
})
```



## 删除数据

- 通过node操作数据库更新数据

```js
const mysql = require('mysql');
const db = mysql.createPool({
    host:'127.0.0.1',
    user:'root',
    password:'xxx',
    database:'my_db-01'
})

const sqlStr = 'delete from users where id=?';
db.query(sqlStr,9,(err,result) =>{
    if(err){return console.log(err.message)}
    if(result.affectedRows === 1){
        console.log('id=9的用户信息删除成功')
    }
})
```



## 更新数据

```js
const mysql = require('mysql');
const db = mysql.createPool({
    省略
})
const user = {username:'和黑',password:'345ddd'}
const sqlStr = 'update users set ? id=?';
db.query(sqlStr,[user,user.id],(err,result) =>{
    if(err){return console.log(err.message)}
    if(result.affectedRows === 1){
        console.log(result)
    }
})
```



## 标记删除

- 为了保险起见，使用update标记数据状态status, 而不是delete直接实际删除数据，

```js
const sqlStr = 'update users set status = ? where id = ?';
// 将id为6的用户状态status 标记为1
db.query(sqlStr,[1,6],(err,result) =>{
    if(err){return console.log(err.message);}
    if(result.affectedRows === 1){
        console.log('标记成功');
    }
})
```



# Web开发模式

- 基于服务端渲染的传统web开发模式
  - 服务端完成拼接、动态生成HTML内容，浏览器只要负责渲染即可，有利于SEO
  - 缺点：占用服务器资源，无法前后端分离
- 基于前后端分离的新型web开发模式
  - 依赖ajax技术，后端负责接口，前端负责接口调用
  - 用户体验好，同时减轻服务器压力
  - 缺点：不利于SEO，可以利用Vue React前端框架的SSR渲染解决SEO问题
- 如何选择Web开发模式
  - 企业级网站 强调展示而不是复杂交互 且不考虑seo ,可以考虑服务端渲染
  - 二类似后台管理项目 交互性强不需要考虑seo，就采用前后端分离模式
  - 或者采用平衡策略：首屏服务端渲染，其他页面采用分离模式



## 身份认证

- Authentication 又称身份验证或 鉴权：指通过一定手段，完成对用户身份确认
  - 如：短信验证，二维码或者邮箱验证等
- 不同模式下的身份认证
  - 服务端渲染：推荐使用 Sessiion 认证机制
  - 前后端分离：推荐使用 JWT 认证机制



## Session 认证机制

- http协议的无状态性，服务器不会主动保存http请求的状态
- 突破http无状态的限制：
  - cookie: 不超过4KB，由键值形式存储在浏览器中，拥有有效期 安全性 使用范围的可选属性组成
  - 不同域名下的cookie各自独立，每当客户端发起请求会自动将当前域名下未过期的cookie一并发送到服务器
- cookie认证的作用
  - 客户端第一次请求服务器的时候，服务器通过响应头的形式，向客户端发送一个身份认证的cookie，客户端会自动将cookie保存到浏览器中
  - 之后，当客户端浏览器每次请求服务器的时候，浏览器会自动将身份认证相关的cookie通过请求头的形式发送给服务器，服务器即可验证客户端身份
  - cookie 不具有安全性，因为cookie是存储在浏览器且浏览器提供了读写的api



## Express中Session认证

- 下载：npm install sessiion
- 导入：express-session
- cookie不支持跨域

```js
const express = require('express');
const app = expess();
const session = require('session');
app.use(session({
     secret:'itheima',
     resave:false,
     saveUninitialized:true
}))
```



## JWT认证

- 需要跨域请求的时候 推荐使用JWT身份认证机制
- 原理：
  - 客户端提交账号密码给服务器，
  - 服务器验证后生成加密的token字符串，发给客户端
  - 客户端通过localStorage或SessiionStorage存储
  - 客户端再次发起请求通过http请求头的Authorization字段，将token携带给服务器
  - 服务器将token还原成用户信息对象，认证成功后，再响应内容给客户端
- token组成的三大部分：
  - Header:安全相关
  - Signature: 安全相关
  - Payload: 用户的加密信息
- 安装包：npm install jsonwebtoken 用于生成jwt字符串
- 安装包：npm install express-jwt  将jwt字符串解析成json对象

```js

```

























