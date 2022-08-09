# web要解决的问题

- webpack   就是模块打包工具, 提供懒加载，代码分割，摇树化Tree shaking
- import ...export 模块之间的依赖都要借助webpack来实现，之后进行代码打包，因为传统的直接导入导出，效率不高且难于维护
- webpack本身只能识别js和json，因此css等就需要借助第三方插件和包来实现
- loader用于特定模块类型，plugins插件可以用于更广泛任务
  - 提示：除rules和use和plugins是[]外，其他基本都是{}对象设置

```js
// 终端如果提示系统禁止运行脚本，退出-> 以管理员身份运行vc code -> 再输入下面命令即可成功运行webpack
Set-ExecutionPolicy RemoteSigned
```



## 模块化

- node的CommJS; 采用 require导入，exports.module导出
- ES6模块化； import 导入，export 或export default 导出



## 五大核心

1. Entry  : 入口文件 起点-> 分析构建依赖
2. Output ：输出，打包后输出到那个目录
3. Loader：处理非 JS 文件  (webpack只能处理 js&json文件)
4. Plugins：打包需要用到的专业插件 包括优化和压缩
5. Mode：设置webpack 使用相应模式，-> 开发/生成 模式    development / production



## 打包配置

注释:打包配置，npm默认下载最新版本，使用@跟上指定版本号可以下载指定版本,开发环境-D，生成环境-S

1. npm i webpack webpack-cli  -g   全局安装  (了解)

   1. webpack    全局使用

2. 因此应该使用开发环境配置，配置步骤如下：

   1. npm init -y   :生成脚本依赖管理文件package.json 记录脚本命令 版本号 入口js文件等
   2. npm install webpack  webpack-cli -D   : 生成node_modules文件夹与package-lock.json记录文件
      1. npx webpack  app.js：用于支持自动查找目录并打包(了解，正常开发不使用)
         1. 表示以app.js为入口进行依赖分析并打包
      2. -D 表示开发环境下使用，生产打包会删除该依赖
   3. 解析es6还需要安装babel+babel的插件，六件套
      1. babel-loader@7
      2. babel-core
      3. babel-preset-env
      4. babel-plugin-transform-runtime   转es6/7
      5. babel-plugin-transform-decorators 转es6/7
      6. babel-plugin-transform-decorators-leagacy 转es6/7
   4. package.json依赖管理文件说明:

   ```js
   // 改进npx命令：-> 将scripts调式对象内默认参数进行修改-如下:
   "script":{
       "build":"webpack" // build表示脚本名称 weppack表示脚本运行命令->当前配置文件
       // 脚本名称可以叫build/serve
   }
   ```

   

# webpack配置

- 由于npx webpack  app.js打包方式不方便，我们需要创建一个webpack.configs.js 自定义配置文件，
- 使用module.exports导出对象，不要使用es6的export default

```js
const path = require('path'); // 引入node内置路径模块

module.exports = {
 // 打包入口js文件 webpack会以这个文件为入口进行依赖分析，要打包的文件需import引入到这个js文件内
 // entry是对象形式，如果只有一个入口文件直接如下面简写即可
    entry:'./src/main.js', 
    // 打包输出配置
    output:{
       // resolve函数读取输出路径->生成文件目录名build
       path:path.resolve(__dirname,'build'), 
       // 定义打包生成的文件名，将其存入build目录下
       filename:'js/buildle.js',  
    }
}
```



- 运行命令：npm run build ；打包，打包解析后才能被浏览器识别运行
- 注意入口路径，入口文件必须在根目录，webpack.json和webpack.config.js也是



## 打包JSON

注释：webpack默认支持js和json打包

```js
// src目录下创建JSON文件 
{
    "name": "hello",
    "age": 18
}

在JS文件内引入JSON 然后运行打包命令
import data from './data.json'
// 可以成功打包

// 引入css文件 报错 无法打包成功 可以在main.js查看错误
import './index.css'
```



## 打包CSS

注释：webpack.config.js 配置文件作用：指示webpack 干那些活

​           所有的构建工具都是基于node环境运行的，模块化默认采用commonjs

```js
// 1: 在webpack.config.js 配置文件 注：与src同级
// 2: 下载 对应文件的 loader包，注意版本不能过高 最好style-loader@2.0.0,css-loader@5.0.1
npm i css-loader style-loader -D
// 3: 将打包后生成的js文件引入到html即可
// 4：如果要打包less 需要安装两个包 scss同理 参考就行
npm i less -D 以及 npm i less-loader -D
```

webpack.config.js打包配置

```js
// 引入内置路径模块path, resolve 是用来拼接绝对路径的方法
const {resolve} = require('path');

module.exports = {
    // 入口
    entry: './src/index.js',
    // 输出
    output: {
        // 输出文件名
        filename: 'built.js',
        // 输出路径  __dirname表示当前文件的目录绝对路径 即webpack.config.js
        path: resolve(__dirname, 'build')
    },
    // loader 配置 允许配置多个loader
    module: {
        // rules数组详细配置
        rules: [{
                test: /\.css$/, // 正则匹配以.css结尾的文件
                // 匹配后要使用那些loader进行处理
                use: [
                    // use数组中的loader执行顺序: 从右向左 即从下往上
                    // 创建style标签,将js中的样式资源插入进去，添加到head中生效
                    'style-loader',
                    // 将css文件转成commonjs模块加载到js中，内容为样式字符串
                    'css-loader'
                ]
            },
            {
                // 利用正常匹配less文件 记得下载less和less-loader
                test: /\.(less|css)$/, // 也可以less和css写一起
                use: [
                    'style-loader',
                    'css-loader',
                    'less-loader' // 将less文件编译成css文件
                ]
            },
        ]
    },
    // plugins 插件配置
    plugins: [
        // plugins详细配置 需要在文件内引入模块 再在这里 new 一个实例出来
    ],
    // 开发环境模式
    mode: 'development',
    // 生产环境模式
    // mode: 'production'
}
```



## postcss插件(了解)

注释：用于帮助css添加浏览器前缀和样式适配，(了解，使用较少)

1. npm i postcss-cli -D,   安装
2. npm i autoprefixer -D, 借助浏览器前缀插件
3. npm i postcss-loader -D, 安装插件

```js
const patch = require('path');
module.exports = {
    entry:'./src/main.js',
    output:{
        path:path.resolve(__dirname,'dist'),
        filename:'bundle.js'
    },
    rules:[
        {
            test:/\.css$/,
            use:[
                'style-locader',
                'css-loader',
                // 以对象集合形式设置，也可以抽离出去创建postcss.config.js
                {
                    loader:'postcss-loader',
                    // 配置选项 
                    options:{
                        postcssOptions:{
                            // 执行时使用authoprefixer插件
                            plugins:[
                                require('authoprefixer')
                            ]
                        }
                    }
                }
            ]
        },
    ]
}

// 使用： css或者less文件内需要设置 user-select
    .title{
        color:red;
        font-size:20px;
        
        user-select:none; // 添加浏览器前缀
    }
```



## postcss-preset-env插件

注释：也是css类插件，用法同上，功能更强大可以适配新代浏览器特性

1. npm i postcss-preset-env -D  安装



## 打包html文件

注释：html打包需要下载插件plugins

1. npm i html-webpack-plugin -D
2. 引入插件
3. 打包后会生成一个html文件 并自动引入相应的js依赖
4. 注意：需要手动在打包后生成的html文件内手动写入元素节点
   1.  如果提供html模板的话就不需要手动写入 (推荐)

```js
const {resolve} = require('path');
// 引入html插件
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.export = {
    entry:'./src/index.js',
    output:{
        filename: 'built.js',
        path: resolve(__dirname, 'build')
    },
    module:{
        rules: [
            
        ]
    },
    plugins:[
      // 这个插件是个构造函数需要new 默认创建一个空html并自动引入打包输出的资源(js&css)
          // 需求：需要有结构的html文件
        new HtmlWebpackPlugin([
            // 提供一个html模板,/src/index.html文件 并自动引入打包输出的资源(js&css)
            template,'./src/index.html' 
        ])
    ],
    // 开发环境
    mode:'development';
}
```



## 打包图片

1. npm i url-loader file-loader -D  下载2个包
   1. url-loader 将较小的文件转成base64编码
   2. file-loader  打包图片并创建输出文件夹
2. npm i html-loader -D    下载html包
3. 注释：如果无法显示图片就使用webpack5的asset配置即可

```js
// 处理图片
rules:[
     {
     test:/\.(jpg|png|gif)$/,
     // 如果只使用一个loader就不需要写use了
         use:{
          loader:'url-loader',// 如果想对较小图片进行处理可以用这个
          loader:'file-loader', // 全部打包用这个
          options:{
          outputPath:'img', // 输出文件夹 会生成img文件夹到打包输出文件夹内
          name:'[name]_[hash:6].[ext]',// 采用图片原有名字+哈希值防止重复,并保留图片原有后缀格式名
         // 图片小于8kb 就会被base64编码处理
          limit: 8 * 1024,
          esModule: false // 关闭es6模块采用node的commonjs模式
        }
     }
  }
]
// 使用图片 导入将变量赋值给变量给src属性即可
import tupian from '../imgs/zzz.jpg'
```



## 打包字体图标

- npm install file-loader -D
- 还有一个设置html标题title的包叫 npm install  ejs -D
  - 可以设置ejs语法：<title><%= HtmlWebpackPlugin.options.title %></title>
  - 然后在webpack.config.js的插件配置->html模板对象内设置title

```js
{
    test:/\.(eot|svg|ttf|woff)/,
    loader: 'file-laoder'
}
```



## 清理文件

- 每次重新打包时之前的打包文件需要手动删除，
- 利用插件实现自动清理，且会自动生成依赖文件
- 安装：npm  install --save-dev clean-webpack-plugin@3.0.0

```js
// 导入 需要以对象方式导入
const {CleanWebPackPlugin} = require('clean-webpack-plugin');
plugin:[
    new CleanWebPackPlugin()
]
```



## 错误映射

- 打包文件出现错误时提示源代码错误处,属于内置属性
- 默认就是开启的

```js
module:{
    devtool:'none', // 不提示错误
    // devtool:'eval-source-map' 映射源代码错误
}
```



## WebpackDevServer

- 每次更改代码后需要重新打包。利用webpackDevServer可以实现更新，而不需要每次都重新打包
  - 开启服务器后运行在dist目录是看不到打包文件的，而是将其存放到了内存中以提升速度
- 安装: npm install webpack-dev-server --save-dev@3.11.2
- webpack内置的watch：webpack-watch ; 方法也能实现类似效果 (不推荐)

```js
module:{
    // 生成一个服务器
    devServer:{
       // port:3000, 还可以更改服务器端口 让它运行在3000端口而不是 8080
        // 指定输出目录 与output的输出目录要一致
        contentBase:'./dist'，
        // 服务器开启成功后自动打开网页
        open:true
    }
}

---package.json文件也需要添加一个调式命令
"script":{
    "build":'webpack',
     // 添加一个webpackdevServer调式命令
     "dev":"webpack-dev-server"
}
```



## 代理转发-跨域

- 用于前端跨域代理 (推荐)
- 由于本地的端口跟服务器的url不一致导致本地请求被浏览器拦截，
- 这时可以利用webpack的proxy实现代理跨域
  - 服务器的任何配置改动都需要重新启动下
- 如果是多个路径转发就可以放入数组，context: ['/api', '/auth']
- 更多详情查看官方文档API

```js
// 应用页面
import axios from 'axios';

// 访问本地方式 127.0.0.1:8080或者 localhost:8080
axios.get('http://study.jsplusplus.com/Yixiantong/getHomeDatas').then((result) =>{
    console.log(result.data);
})

------改造 利用pxory代理------------------------

// 应用页面
import axios from 'axios';

// 把真实的服务器url, http://study.jsplusplus.com 抽离出去
axios.get('/Yixiantong/getHomeDatas').then((result) =>{
    console.log(result.data);
})

// webpack 配置
module:{
    devServer:{
        contentBase:'./dist',
        open:true,
       // 利用pxory对象实现接口的代理转发
       proxy:{
         // 只要基地址或者说请求地址是/Yixiantong的接口
         '/Yixiantong':{
           // 就拼接到真实项目的后台接口，让代理进行转发且允许跨域changeOrigin=true
           // 这样我们在本地还是localhost:8080访问，代理会帮我们转发请求，
           // 这样项目无论是开发还是生产阶段 都能保持接口地址的一致性
           target:'http://study.jsplusplus.com/',
           changeOrigin:true, // 是否需要跨域
           secure:true  // 是否接收是https的接口
         }      
      }
    }
}

-------如果有的接口有api前缀----那么就需要重写路径-----如下----
// 应用页面
import axios from 'axios';

// 把真实的服务器url, http://study.jsplusplus.com 抽离出去
axios.get('/api/Yixiantong/getHomeDatas').then((result) =>{
    console.log(result.data);
})

// webpack 配置
module:{
    devServer:{
        contentBase:'./dist',
        open:true,
       // 利用pxory对象实现接口的代理转发
       proxy:{
         // 只要基地址或者说请求地址是/api/Yixiantong的接口(这里直接/api也可以)
         '/api/Yixiantong':{
           // 就拼接到真实项目的后台接口，让代理进行转发且允许跨域changeOrigin=true
           // 这样我们在本地还是localhost:8080访问，代理会帮我们转发请求，
           // 这样项目无论是开发还是生产阶段 都能保持接口地址的一致性
           target:'http://study.jsplusplus.com/',
           changeOrigin:true,
           // 重写路径 将以api开头的接口，api三个字符置空
           pathRewrite:{
             '/api':''
               // 如果api前面还有其他前缀可以采用 '^/api':''
           }
         }      
      }
    }
}
```



![](D:\05-code-word汇总\4-重学前端-文档笔记\js基础深入.assets\9-proxy.jpg)





## 热更新

- 指在修改文件时如更改css某个元素的样式颜色，保存后webpackdevServer，自动更新后页面状态会丢失
- 使用热更新，页面状态不丢失，只是局部更新
- 热更新属于webpackDevServer对象的属性，同时需要引入webpack内置的包

```js
// 导入支持热更新的模块
const webpack = require('webpack');
module.exports:{
    devServer:{
        hot:true // 开启热更新
    },
        
 plugins:[
     // 调用热更新
     new webpack.HotModuleReplacePlugin()   
  ]
}
```



## JS的热更新

- JS的热更新与css不同，需要进行额外的设置
- 因为css的style-loader已经内置了逻辑单元，vue 也是内置了类似的功能vue-loader,React则是react babel-preset
- 而JS则需要手动处理

```js
// counter页面
function counter(){
    const div = document.createElement('div');
    div.innerHTML = 1;
    div.setAttribute('id', 'counter');
    document.body.appendChild(div);
    div.addEventListener('click',function(){
        div.innerText = +div.innerText + 1;
    })
}

// number 页面
function number(){
    const div = document.createElement('div');
    div.innerText = 22;
    div.setAttribute('id','number');
    document.body.appendChild(div);
}
export default number

// app.js页面
import number from './src/number.js';
import counter from './src/counter.js'

counter();
number();
// 此时假设webpack的热跟新hot 和new webpack.HotModuleReplacementPlugin()已开启

// 问题:当couter点击自增1时 ，去number页面更改div.innerText的值 22-> 33
// 保存后页面状态不会丢失，但是会导致旧的元素节点依旧存在body内，页面出现两个相同id名的节点
// 解决办法: 更改 app.js的处理逻辑：如下
if(module.hot){
    // 监听范围-> src/number.js
    module.hot.accept('./src/number.js',() =>{
        // 获取旧的节点 删除它
        const oDiv = document.getElementById('number');
        document.removeChild(oDiv);
        // 删除后重新调用 number
        number();
    })
}
```



![](D:\05-code-word汇总\4-重学前端-文档笔记\js基础深入.assets\10-js热更新.jpg)





## babel编译

- 将ES6代码转换到ES5
- babel官网->设置手册查看-选择webpack->根据步骤进行设置即可,如下
  - 安装babel：npm install ---save-dev babel-loader @babel/core
  - 配置：rules
  - 继续安装：npm install @babel/preset-env --save-dev    将ES6语法进行转换-ES5
- 设置完毕后再进行打包：ES6旧被转换成ES5了
- 或者最好手动创建一个babel.config.json文件将有关es6转es5的配置放进去

```js
// index3.js页面
const promiseArray = [
    new Promise(() => { }),
    new Promise(() => { })
]

promiseArray.map(promise => {
    console.log('promise', promise);
})

// app.js 页面
// 打包后的文件是不进行转换的 写的ES6语法 打包后还是ES6
import index3 from './src/index3.js';


// 那么为了兼容低版本浏览器就需要利用babel进行语法转换
配置：
module.exports = {
    module:{
        rules:[
            {
                test:/\.js$/,
                loader:'babel-loader',
                options:{
                    presets:['@babel/preset-env']
                }
                // 排除依赖文件夹
                exclude:/node_modules/
            }
        ]
    }
}
```



![](D:\05-code-word汇总\4-重学前端-文档笔记\js基础深入.assets\11-babel.jpg)



## babel兼容(不推荐)

- 低版本浏览器可能没有实现类似 map这样的方法，那么也可以解决
- 官网文档->@babel/polyfill (用于实现promise和map这样现金特性的缺失)
- 安装：npm install --save @babel/polyfill
- app.js入口文件导入：import require("@babel/polyfill"); 注意：这个是再入口文件导入而不是再webpack.config.js文件
- 由于兼容低版本浏览器缺失语法特性，会增加大量代码，导致打包文件体积增大，所以不推荐使用，现代浏览器不需要再考虑过多的兼容性
- 如果一定要使用可以对webpack进行配置。最号将es6转es%代码放入单独的babel.config.json文件

```js
module.exports = {
    module:{
        rules:[
            {
                test:/\.js$/,
                loader:'babel-loader',
                options:{
                    // 重写设置 告诉babel 在入库口文件引入 import "@babel/polyfill";时
                    // 检查页面用到的ES6特性，而不是一股脑的将所有ES6都兼容完，这样可以减少打包体积
                    presets:[['@babel/preset-env',{
                        useBuiltIns:'usage',
                        // 指定一个版本号 需要与下载的本本匹配，否则可以不写该项
                        corejs:3
                    }]]
                }
                // 排除依赖文件夹
                exclude:/node_modules/
            }
        ]
    }
}
```



```js
// 将es6转es5以及react或者vue等相关代码放入单独的babel.config.json文件
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "useBuiltIns": "usage"
      }
    ],
      "@babel/preset-react"
  ]
}
```



## 打包React文件

- 安装react包：npm install react react-dom
- 安装打包编译reac的对对应包：npm install --save-dev @babel/preset-react
- 

```js
// 创建js文件写入react的jsx语法
import React, { Component } from 'react';
import ReactDOM from 'ract-dom';

class App extends Component {
    render() {
        // jsx语法
       return <div>Hello from React</div>;
    }
}


ReactDOM.render(<App />, document.getElementById('app'));

// 打包编译失败 因为webpack无法识别jsx语法，
// 解决：babel下载相关包

module.exports = {
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    'style-loader',
                    'css-loader'
                ]
            },
            {
                test: /\.js$/,
                loader: 'babel-loader',
                options: {
                    presets: [['@babel/preset-env', {
                        useBuiltIns: 'usage'
                    }],
                    // 用于打包编译React的jsx语法，没有其他设置，直接放入即可
                     "@babel/preset-react"
                    ]
                },
                // 排除依赖文件
                exclude: /node_modules/
            }
        ]
    },
}
```



## 打包Vue文件

- 安装包：npm install vue-loader   npm install vue
- 安装模板编译器包：npm install vue-template-compiler
- 导入vue-loader包：const { *VueLoaderPlugin* } = *require*('vue-loader') 
- css或者scss配置参照官网
  - 如果是vue脚手架则直接打包即可因为已经集成了webpack

```js
// App.vue 页面
<template>
  <div id="app">
      <div>Hello Vue</div>
  </div>
</template>
<script>
console.log('vue-Script!');
export default {}
</script>
<style>
#app{
    background-color: orange;
}
</style>

// index.js页面
import {createApp} from 'vue'; // 反正这里练习打包时这样导出vue的，实际项目不知道是否有用
import App from './App.vue';

// 就这样创建实例->渲染,用 new Vue会爆破错，不知道实际项目是否有用
crateApp(App).mount('#app');

// app.ja 入口文件
import index from './src/index';

// webpack.config.js 配置
// 按照官网最新方式导入包，不要用旧的require('vue-loader/lib/plugin)
const { VueLoaderPlugin } = require('vue-loader')
module.exports = {
    rules:[
        // 编译vue的css暂时用style-loader，用vue-style-loader需要版本对应上才行
        {
            test:/\.css$/,
            use:[
                'style-loader',
                'css-loader'
            ]
        },
        {
           test: /\.vue$/,
           loader: 'vue-loader'
        }
    ],
    // 插件
    plugins:[
        // new 一个vue模板编译器实例
        new VueLoaderPlugin()
    ]
}
```



# 优化

## 摇树化Tree Shaking

- 使webpack只打包调用到的代码，如某个文件只导入了一个函数使用，那就暂时只打包这一个用到的函数
- 只支持ES Module 模块化，不支持Node的ComonJS
  - ES Module  静态引入 ，编译时引入， import / export 一开始就需要确定是否要引入
  - CommonJS 动态引入 ，执行时引入,  require  module.export
- 摇树化在开发环境依旧会打包全部，这是为了方便调式，生产环境就会只打包用到的内容或者函数

```js
// min.js页面
export const add = (number1,number2) =>{
    console.log(number1 + number2);
}

// index.js页面
import {add} from './min'
const result = add(10,20)

// app.js入口文件
import index from './src/index'

// webpack 配置
module.exports = {
    // 优化：只有使用到的内容才进行打包,生产模式会自动配置不需要这样显式写明
    optimization:{
        usedExports:true
    }
}

// package.json文件第一项下面添加一个配置描述
// 表示那些文件不需要去检查是否有使用到，false默认效果
// 如果有导入import "@babel/polyfill"和css可以使用数组描述 表示不要去管这2个包文件
"sideEffects": false
// 比如这样
// "sideEffects":['@babel/polyfill','*.css']
```



## 不同的打包模式

- 实际开发中会将生产环境与开发环境单独放入不同的文件
- 开发环境：创建 webpack.dev.js
  - 需要代理proxy和热更新，转es5
- 生产环境：创建 webpack.prod.js
  - 不需要代理proxy和转es5，也不需要热更新
- 最后webpack.json 文件的调式命令也要更改下：

```js
"script":{
    // 打包：使用生产环境文件的配置 会额外多生成一个映射文件xxx.js.map用于映射错误
    "build":"webpack --config webpack.prod.js",
    // 打包->热更新：使用开发环境的配置文件
    "dev":"webpack-dev-server --config webpack.dev.js",
    // 如果在开发环境下也想查看打包后的文件(即不使用热更新启动)，可以如下配置
    "dev:build":"webpack --config webpack.dev.js"
}
```



## 解决webpack配置重复问题

- 生成环境的配置文件和开发环境的配置文件有很多代码是重复的，可以使用插件解决
- 安装：npm install webpack-merge -D
- 创建文件：webpack.base.js, 将重复的代码也就是2种环境都有用到的代码块提取到webpack.base.js中
- 示例：以下三个代码框
- 提取开发&&生产配置，的重合部分
- 安装一个合并代码的包：npm install webpack-merge@5.7.3  注意版本的导出名称可以查看官网
- 最后可以将这三个配置文件放入一个文件夹：起名 build

```js
// 提取重复代码后的 webpack.dev.js 开发环境
// 导入热更新包
const webpack = require('webpack');
// 导入合并包
const {merge} = require('webpack-merge');
// 获取公有(重合部分的代码)
const baseConfig = require('./webpack.base');

// 配置对象
const devConfig = module.exports = {
    mode: 'development',
    devtool: 'cheap-module-eval-source-map',
    devServer: {
        contentBase: './dist',
        // 热更新
        hot: true,
        open: true,
        proxy: {
            '/api/Yixiantong': {
                target: 'http://study.jsplusplus.com',
                changeOrigin: true,
                pathRewrite: {
                    '/api': ''
                }
            }
        }
    },
    // 插件 
    plugins: [
        // 热跟新
        new webpack.HotModuleReplacementPlugin(),
    ],
};

// 将公有部分和当前配置文件内的业务代码合并后->导出->供调式命令使用
module.exports = merge(baseConfig, devConfig);
```



```js
// 提取重合代码后的 webpack.prod.js 生产环境
// 导入合并包
const {merge} = require('webpack-merge');
// 导入公有(重合部分的代码)
const baseConfig = require('./webpack.base');


// 配置对象
const prodConfig = module.exports = {
    mode: 'production',
    devtool: 'cheap-module-source-map',
}

// 将公有部分和当前配置文件内的业务代码合并后->导出->供调式命令使用
module.exports = merge(baseConfig, prodConfig)
```



```js
// 公有(重合部分的代码) webpack.base.js 
// 导入路径模块
const path = require('path');
// 打包html插件
const HtmlWebPlugin = require('html-webpack-plugin');
// 自动清理插件
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

module.exports = {
    // 入口文件
    entry: './app.js',
    // 配置loader
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    'style-loader',
                    'css-loader'
                ]
            },
            {
                test: /\.js$/,
                loader: 'babel-loader',
                // 排除依赖文件
                exclude: /node_modules/,
                // 转ES5
                options: {
                    presets: [['@babel/preset-env', {
                        useBuiltIns: 'usage'
                    }],
                        // "@babel/preset-react"
                    ]
                }
            },
            {
                test: /\.vue$/,
                loader: 'vue-loader'
            }
        ]
    },
    // 插件 
    plugins: [
        new HtmlWebPlugin({
            template: './src/index.html'
        }),
        // 文件清理
        new CleanWebpackPlugin()
    ],
    // 摇树化
    optimization: {
        // 如果有使用到引入的内容才打包它
        usedExports: true
    },
    // 输出
    output: {
        filename: '[name].js',
        // 如果要将三个webpack配置文件都放入一个文件夹内的话，路径需要改写成../dist
        path: path.resolve(__dirname, '../dist')
    },
}
```



```json
// webpack.json文件 的调式命令预览 分别是：生产，开发，开发(不使用热更新功能)
  "scripts": {
    "build": "webpack --config ./build/webpack.prod.js",
    "dev": "webpack-dev-server --config ./build/webpack.dev.js",
    "dev:build": "webpack --config ./build/webpack.dev.js"
  },
```



## 代码分割一

- 也是一种优化手段，就是让第三方库不要与业务代码打包混肴在一个文件
- 安装JS第三方工具库用于测试：npm install lodash

```js
// index.js 页面， 导入第三方工具库
import _ from 'lodash';

const result = _.join(['test1', 'test2', 'test3'],'-');
console.log(result,'result');
// 结果发现 假设indedx.js = 1mb, lodash库 = 1mb, 1mb + 1mb = 2mb 导致打包文件体积增大
```



- 改进：代码分割 让业务代码和工具代码分离，这样第一次访问式加载2个文件，之后访问则只需要下载业务代码

```js
// index.js 页面 
console.log('Hello JS++');

const result = _.join(['test1', 'test2', 'test3'],'-');
console.log(result,'result');

// 创建lodash.js 专门用于存放lodash工具库
import _ from 'lodash';
window._ = _; // 将库挂载到window

// webpack.base.js 配置
module.exports = {
    // 修改下入口配置 打包后也会生成2个文件main.js和lodash.js
    entry:{
        main:'./src/index.js',
        lodash:'./src.lodash.js'
    },
    output:{
        filename:'[name].js',
        path:path.resolve(__dirname,'../dist')
    }
}
```



![](D:\05-code-word汇总\4-重学前端-文档笔记\js基础深入.assets\12代码分割.jpg)

- 这样打包生成后的html也会同时引入这2个js包




## 代码分割二

- 手动配置实现代码分割,不再单独创建lodash.js专门存放lodash工具库
- 打包后还是会生成2个文件一个业务代码，一个vendors~main.js 存放有第三方库代码
- 懒加载这种异步加载第三方库的方式，webpack也会自动实行代码分割，且不需要配置optimization对象
- 配置详情参照官网

```js
// index.js 页面
import _ from 'lodash';
import test from './test.js' //如果引入的文件不是第三方库,不会分割到分割后的文件，而是跟业务代码在一起

function createElement(){
   return import('lodash').then(result =>{
        return result;
    })
}
const createElementPromise = createElement();
createElementPromise.then(res =>{
    console.log('res',res.default);
    console.log('res.default()',res.default());
    console.log('res.default().join',res.default().join);
})

// webpack.base.js 文件 配置改动
module.expots = {
    entry: main:'./src/index.js',
    // 优化
    optimization: {
        // 如果有使用到引入的内容才打包它
        usedExports: true,
        splitChunks:{
            // all 同步异步代码都进行分割 async只对异步代码分割
            chunks:'all',
           // 如果第三方库大于20kb就进行分割
            minSize:20000,
            // 缓存组
            cacheGroups:{
                // 给分割后的第三方库文件起名vendors~这里会跟着入口文件如vendors~main.js
                vendors:{
                   // 引用第三方库是否来自node_modules
                    test:/[\\/]node_modules[\\/]/,
                    // 优先级
                    priority:-10,
                    // 复用已有模块
                    reuseExistingChunk:true,
                   // 如果不想生成上面默认的vendors~main.js文件名,这里可以自行设置一个文件名
                   filename:'vendors.js',
// 表示一个公共模块在入口文件内被使用了多少次，默认1 如果引入了2次就要写2，打包后才能自动对应引入
// 如果引用了2次，那么插件new HtmlWebPlugin内所提供的html模板需分别设置：且入口文件对象也要设置对应路径
    // 如：第一个模板chunks:['vendor','other']，第二个模板 chunks:['vendor','index']
                  minChunks:1,
                },
              // 如果还不满意，就默认分割后的代码叫test.js这个名
              default:{
                  // 以上面的-10优先
                  priority:-20
                  filename:'test.js'
              }
            }
        }
    },
}
```



## 代码分割三

- 测试引用2次

```js
// index.js 页面
import _ from 'lodash';

const result = _.join(['test1','test2','test3'],'-')
console.log(result);

// index.html 页面
<div id="app"></div>

// other.js页面
import _ from 'lodash';
console.log('to.js');

// other.html页面
<div id="app"></div>

// webpack.base.js配置
module.exports = {
    // 入口文件
    entry: {
        index: './src/index.js',
        other:'./src/other.js'
    },
    // 插件 
    plugins: [
        // 设置对应的模板
        new HtmlWebPlugin({
            template: './src/index.html',
            filename:'index.html',
            // 第一个引入第三方包的 这个文件要引入那2个文件
            chunks:['vendor','index']
        }),
        new HtmlWebPlugin({
            template: './src/other.html',
            filename:'other.html',
            // 第二个引入第三方包的 这个文件要引入那2个文件
            chunks:['vendor','other']
        }),
        // 文件清理
        new CleanWebpackPlugin()
    ],
     // 优化
    optimization: {
        // 如果有使用到引入的内容才打包它
        // usedExports: true,
        splitChunks: {
            // all 同步异步代码都进行分割 async只对异步代码分割
            chunks: 'all',
            // 如果第三方库大于20kb就进行分割
            minSize: 20000,
            // 缓存组
            cacheGroups: {
                vendors: {
                    // 引用第三方库是否来自node_modules
                    test: /[\\/]node_modules[\\/]/,
                    priority: -10,
                    // 复用已有模块
                    reuseExistingChunk: true,
                    filename: 'vendors.js',
                    minChunks:1,
                },
                // 优先以上面的-10为准
                default:{
                    priority:-20,
                    filename:'test.js',
                    minChunks:2,
                }
            }
        }
    },
}
```



## 懒加载

- 按需加载，需要某个触发条件，而不是页面载入就加载所有资源

```js
// 异步 懒加载
function createElement() {
    // 魔法注释 /*webpackChunkName: */ 可以设置文件名
    return import(/*webpackChunkName: */ 'lodash').then(({ default: _ }) => {
        const result = _.join(['test1', 'test2', 'test3'], '-');
        const div = document.createElement('div');
        div.innerText = result;
        return div;
    })
}

然后这里可以设置一个事件   触发事件才加载
```





# webpack5(重要)

注释：之前打包资源需要用到各种loader插件，webpack5开始，可以直接使用资源模块类型 asset module typelai 替代

无需下载包，

1. asset/ resource   发送一个单独文件并导出url 替代file-loader
2. asser/inlilne   导出一个资源的data URL,  替代url-loader
3. asser/source  导出资源代码 替代raw-loader
4. asset 在导出data URL和 发送单独文件之间自动选择，替代url-loader

## 打包图片

```js
// 打包图片 asset 替代 url-loader
{
    test:/\.(jpg|png|gif)$/,
    type:'asset',
        generator:{
           // 采用图片原有名字+哈希值防止重复,并保留图片原有后缀格式名
            filename:'img/[name]_[hash:6].[ext]'
        },
        // 解析配置
        parser:{
            // 针对指定图片情况 转码base64
            dataUrlCondition:{
                maxSize: 100 * 1024
            }
        }
}
```



## 处理字体图标

注释：

```js
// webpack 4
{
    test:/\.(eot|ttf|woff2?)$/, // 匹配字体图标格式
        use:{
            loader:'file-loader',
                options:{
                     // 采用图片原有名字+哈希值防止重复,并保留图片原有后缀格式名
                    name:'font/[name]_[hash:6].[ext]'
                }
        }
}
// webpack5
{
    test:/\.(eot|ttf|woff2?)$/,
    type:'asset/resource',
        generator:{
             filename:'font/[name]_[hash:6].[ext]'
        }
}
```



## 打包清理插件plulgins

注释：每次打包清理前一次遗留文件 ，插件完成: CleanWebpackPlugin

1. npm i clean-webpack-plugin -D   安装

```js
// webpack.config.js 内导入插件
const {CleanWebpackPlugin} = require('clean-webpack-plugin');
plugins:[
    new CleanWebpackPlugin()  // 可以自动清除文件内不相关的文件
]
```



## 打包html插件

注释：可以自动生成html文件并导入打包输出的js作为依赖引用，无需手动建立index.html作为显示模板再导入js

1. npm i html-webpack-plugin -D   安装

```js
// webpack.config.js 内导入插件
const HtmlWebpackPlugin = require('html-webpack-plugin');
plugins:[
    new HtmlWebpackPlugin()
]
```

实际项目中一般手动建立一个html模板，

1. 创建public文件夹
2. 创建index.html文件
3. 到webpack.config.js进行设置

```js
const HtmlWebpackPlugin = require('html-webpack-plugin');
const {DefinePlugin} = require('webpack')  // 解构内置插件
plugins:[
    // 插件方法可以传入参数
    new HtmlWebpackPlugin({
        template: './public/index.html' // 指定打包模板
    })
    new DefinePlugin({
    BASE_URL: './'
    })
]
```



更多api参考官网文档

```js
npm i -D babel-loader@7 babel-core babel-preset-env babel-plugin-transform-runtime babel-plugin-transform-decorators babel-plugin-transform-decorators-legacy sass-loader node-sass css-loader style-loader ejs-loader html-webpack-plugin
```



# 常见错误类型

- Error ：所有错误的父类型
- ReferenceError: 引用错误
- TypeError:  类型错误
- RangeError:  数据值不在允许范围
- SyntaxError: 语法错误
- 捕获错误：
  - try...catch...finally
- 抛出错误；
  - throw ...new Error()

# AJAX篇

- XMLHttpRequest 对象可用于与后端进行数据交互，
- 可以在不刷新页面 (不影响用户体验) 的情况下请求特定 URL，获取数据。更新页面的局部内容
- 可接收http请求 包括file:// 和FTP 文件上传等操作

```js
// 在调用XMLHttpRequest上的任何方法之前都必须先通过构造函数初始化一个实例对象
let xhr = new XMLHttpRequest();
```



## 属性

下列xhr 都是指代 XMLHttpRequest()

- xhr.readState
  -  属性返回一个 XMLHttpRequest 代理当前所处的状态

| 0    | unsent           | 代理被创建,尚未调用open() 方法              |
| ---- | ---------------- | ------------------------------------------- |
| 1    | opend            | open() 方法已被调用                         |
| 2    | headers_received | send() 方法已被调用,可以获得头部状态        |
| 3    | loading          | 下载中；`responseText` 属性已经包含部分数据 |
| 4    | done             | 下载操作已完成，数据传输已彻底完成/失败     |



xhr.response 

- 属性返回响应的正文, 返回的数据类型由 xhr.responseType决定，可以是json、ArrayBuffer、Blob

xhr.responseText

-  在一个请求被发送后，从服务器端返回文本

xhr.responseURL

- 返回响应的序列化 URL,会删除描点部分 也就是#后面的部分

xhr.status

- 返回短正数 0 未发送/失败， 200 载入/完成成功

xhr.statusText

- 返回请求中由服务器返回的一个字符类型的文本信息，信息包含了响应的数字状态码。由后端指定，输出OK或Not Found

xhr.timeout

- 表代表请求耗费时间 默认0  表示没有超时，可手动设置一个超时时间 单位毫秒

xhr.upload

- 用来表示上传进度, 需要通过给 upload 绑定事件来监测状态 



## 方法

xhr.abort

- 用来终止XMLHttpRequest 请求

xhr.load

- 请求完成时触发load  通过回调函数处理

xhr.open

- 初始化一个请求，参数包括：method ，url ，async默认true(即异步) 

xhr.send

- 用于发起请求, 默认异步



## 封装一个Promise的ajax

```js
function ajax(url) {
    return new Promise((resolve, reject) => {
        let xhr = new XMLHttpRequest();
        xhr.open('GET', url, async = true);
        xhr.responseType = 'json';
        xhr.onload = function() {
            if (this.status === 200) {
                // 响应正文
                resolve(this.response)
            } else {
                reject(new Error('出错拉'))
            }
        }
        xhr.send();
    })
}

// 调用  -> 从json文件获取数据
ajax('/xxx.js').then((res) => {
    console.log(res)
})
```





# FormData

- FormData 接口API 提供了一种表示表单数据的键值对 `key/value` 的构造方式，
- 可通过AJAX或Promise或axios与后端进行户数交互
- FormData对象的方法
  - append  往实例对象添加键值
  - delete  删除实例对象上的指定键值
  - entries  返回实例对象上的键值对
  - 其它方法参考MDN文档

```js
// 常用于传参，与ajax一样需要先通过构造函数实例化一个对象
let f = new FormData()

// 往实例对象添加值(以键值对形式)
f.append('username', 'ldh')
```



# Promise篇

- 安装node自动更新包到package.json调式配置：npm install nodemon



## Promise的状态

- 异步编程的解决方案之一，避免回调地狱，
- 实际编程中 已被 async await 替代

- pending 待决：Promise定义时的状态，表示还未开始执行回调处理

- 传递给 new Promise的函数称为 executor执行器函数：由JS内置并自动执行且产生结果-调用如下2个回调：

  - resolve (value); 如果任务完成,调用resolve且带有结果value
    - resolve被调用时内部属性PromiseState: 变为已兑现 fuliflled
    - 内部属性PromiseResult: 呈现 允诺结果：value
  - rejected(error) , 如果任务失败或错误调用reject且带有结果error
    - reject被调用时内部属性ProsmiseState: 变为已拒绝 rejected
    - 内部属性PromiseResult: 给出错误理由： error

- then: 期待得到前面promise的结果对其进行处理，接收2个回调函数作为参数，第一个是成功结果，第二个错误结果

- catch: 捕获promsie失败的结果，如果then的第二个回调未处理错误，则会流入到catch对其进行处理

- finally: 无论promise允诺是否成功，调用该方法都可以执行一次自定义逻辑

- 最终结果都只能是resolve或reject之一，且一旦状态完成 无法更改

  ```js
  // Promise属于微任务 在没有赋值变量成为表达式时，优先级低于同步任务
  const p = new Promise(function(resolve,reject){
      setTimeout(function(){
          // 让 promise处于成功状态
          resolve('success');
          // reject('error)
      },1000)
  })
  
  // then接收2个回调第一个处理成功value, 第二个处理err错误(等同于catch)所以一般另外使用catch，
  p.then(function(value){
      console.log(value); // success
  },function(err){
      // 这里可以处理错误，不过一般不这么做 而是另外使用catch去捕获
      // 或者只对错误感兴趣 then的第一个参数写成null即可，第二个参数设置回调
  })
  
  // 如果只对错误结果有兴趣 直接catch
  p.catch(function(err){
      console.log(err); // error
  })
  ```

  

  ![](D:\05-code-word汇总\4-重学前端-文档笔记\js基础深入.assets\8-promise.jpg)

## Promise的链式调用

- promise可以链式调用 每个then内部return 一个处理过后的新值即可
- 那么就能形成值的传递，也称链，因为每个对then的调用都会创建并返回一个新的Promise
- 错误或成功的结果都是沿着链条传递的，只有下一个then的回调才能接收到上一个Promise的对应结果(而如果上一个回调并未对数据进行处理那么该数据会一直沿着链条进行穿透)
- 如果是错误穿透的话，则统一使用catch进行捕获处理

```js
let promise = new Promise((resolve,reject) =>{
    setTimeout(() =>{
        resolve('success处理成功');
        // reject('error处理失败')
    },1000);
})
promise.then((value) =>{
    return value + ':xjj';
}).then((val) =>{
    return val + ':htllo';
}).then((res) =>{
    console.log(res); // success处理成功: xjj : hello 
}).catch((earson) =>{
    // 如果前面任意then的第二个回调对reject错误进行了处理,则错误不会再流入此处
    console.log(earson,'前面没有处理错误 穿透至此')
})
```



## Promise的静态方法

静态方法可以直接 Promise.调用

- Promsie.all();   并行执行任务,且按照固定的先后顺序返回,一旦其中一个出现失败则全部失败
- Promise.allSettled(), 并行执行任务，若其中一个错误，其它任务照样会返回结果以供接收
- Promise.race();  与Promise类似，区别在于它只在乎第一个完成的请求(无论对错)，即那个请求是最快返回结果的
- Promise.resolve/reject，几乎不用 有async和await替代
- Promise.any(); 与Promsie.race类似，区别在于它只在乎第一个是fulfilled的结果(u而就是成功的结果)

```js
// 三个请求 全部成功则按照顺序全部返回(与延迟时间无关)，反之只要其中一个出现错误 则全部错误
// 实际开发中如果我们需要请求多个接口 可以将其依次或者存入一个数组放入Promise.all()即可
Promise.all([
    // 这里第一个即使延迟5秒或更多， 它也是返回结果的第一个
    new Promise((resolve,reject) =>{setTimeout(() =>{resolve(1)}),3000}),
    new Promise((resolve,reject) =>{setTimeout(() =>{resolve(2)}),2000}),
    new Promise((resolve,reject) =>{setTimeout(() =>{resolve(3)}),1000}),
]).then((value) =>{
    console.log(value); // [1,2,3]
}).catch((err) =>{
    // 错误处理逻辑
})
```



```js
Promise.race([
    new Promise((resolve,reject) =>{setTimeout(() =>{resolve(1)},2000)}),
    new Promise((resolve,reject) =>{setTimeout(() =>{reject(2)},1000)})
]).then((value) =>{
    console.log(value)
}).catch((err) =>{
    console.log(err); // err生效 因为它返回结果比value要快
})
```



## 微任务

- 所有的Promise都属于微任务，会被添加到微任务队列中去(先进先出)，

- 但其内部不属于Promise属性/方法的同步代码会照常执行

- 所以JS中分为同步与异步，

  - 而异步也分为宏任务和微任务，宏任务如setTimeout等，

  - 优先级：同步 > 微任务 > 宏任务

  - 轮询，JS执行时还会按照词法环境(类似嵌套)来进行轮询，

  - 注意：若一个兄弟函数A的延迟时间大于另一个兄弟函数B的嵌套时间总和，轮询机制不变, 但会先执行函数B的内层嵌套函数

    ```js
     function foo() {
       setTimeout(() => {
         console.log("111");
         setTimeout(() => {
           console.log("333");
         });
       }, 1000);
     }
    
      foo();
      function test() {
        setTimeout(() => {
          console.log("222");
        }, 1000);
      }
      test();
    
    打印结果：
       111
       222
       333
    1: 全局环境 foo函数，test函数,确定作用域和词法环境
    2：执行 foo, setTimeout 放入异步宏任务队列
    3：执行 test, setTimeout 放入异步宏任务队列
    4：执行栈：foo: 111 .foo出栈 但未销毁 因为还被嵌套定时器引用着
    5: foo函数的setTimeout有嵌套setTimeout 继续丢入异步宏任务队列
    6：执行栈：test：222 ，完毕出栈 test函数销毁
    7：执行栈：foo 第二个嵌套的setTimeout出列-执行栈 ：333
    8：foo出栈销毁
    ```




## async/await

- async 关键字可以让函数具有异步特性， 确保函数返回一个 promise，也会将非 promise 的值包装进去
  - 可以在外部处理也可以直接在内部处理Promise
- await  只在async内部工作，表示等待结果，(生成器函数的语法糖,在暂停的await处等待Promise的结果，暂停函数的执行，但不会影响其它代码执行)，然后await尝试解析对象的值传递给表达式，再恢复异步函数的执行
- class类 也可以使用async 和await
- async 内部同样可以使用 try....catch来捕获错误
  - 因此async 可以搭配 try..catch来使用，而try块作用域内同样可以使用await，前提是try块有被包含在async内

```js
 async function foo(){
      // return await 'success';
      // 也可以显式的返回一个Promise
      // return Promise.resolve('success')
      return Promise.all([
        new Promise((resolve,reject) =>{setTimeout(() =>{resolve('success1')}),2000}),
        new Promise((resolve,reject) =>{setTimeout(() =>{resolve('success2')}),2000}),
      ])
 }

 //  foo().then((value) =>{console.log(value);})
 let res = foo();
 res.then((value) =>{
   console.log(value);
 })
```



```js
// 一般async不需要像Promise一样去额外then
function test(){
    setTimeout(() =>{
        console.log('我是2秒后响应的网络请求')
    },2000)
}

async function foo(){
    console.log('one')
    // 利用await请求数据
    const res = await test();
    console.log('two')
}
foo(); // one two 我是2秒后响应的网络请求
```



## try..catch

- try...catch可以捕获程序执行抛出的错误
- 但是它无法捕获Promise抛出的异步错误
- Promise的错误捕获需要用 Promise自身的方法 cath

```js
try{
    throw new Error('foo')
}catch(e) {
    console.log(e); // 正确捕获错误， Error: foo，
}

// 无法正确捕获错误
try{
    Promise.reject(new Error('bar'))
}catch(e) {
    console.log(e); // 无法正确捕获,Error: bar
}
```



# webSocket

- web双向通信协议(请求头不再是http或https，而是 wss)

- 项目准备

  - npm init -y
  - npm install express --save   开发与生产都需要
  - npm install nodemon --save-dev   只在开发环境使用
  - npm install socket.io --save   webSocket库
  - https://cdn.bootcdn.net/ajax/libs/socket.io/4.4.1/socket.io.js ；websocket    cdn加速

  ```js
  // 安装好nodemon后 配置调式脚本命令  index.js为入口文件  
  "scripts": {
      "start": "node node_modules/nodemon/bin/nodemon.js index.js"
    },
        
  //  运行命令  npm run start
  ```





```js
devServer:{
    contentBases;'./dist',
        proxy：{
            '/api/xx':{
                target:'http://xxx',
                changeOrigin:true,
                    pathRewrite:{
                        '/api':""
                    }
            }
        }
}
```

















