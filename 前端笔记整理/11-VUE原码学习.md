# 渐进式框架

- React: 用户界面-View视图层—>怎么把数据渲染到视图中，视图-库，状态中央管理Redux,路由(react-router)
  - 单向数据流
  - 数据绑定：单向绑定-通过event触发
  - 自主性更好
- Vue:  用户界面，View视图层—>怎么把数据渲染到视图中，视图->核心库，vuex 选择集成，vue-router选择集成
  - 单向数据流
  - 数据绑定：双向绑定 v-model
  - 强规范，大多数功能需要使用Vue 的API

# vue项目的创建方式

1. 纯cdn方式

2. vite+cdn   (适用于练习vue3/vue2)

   1. npm init -y  生成webpack.json 配置文件
   2. npm add vite@2.3.8 -D

   ```js
    "scripts": {
       "dev": "vite"
     },
   ```

3. npm init @vitejs/app vue-vite 创建一个vue3项目

   1. npm install 用于给vue-vite项目安装node_modules,
   2. vite创建的项目只能用于vue3

4. 脚手架创建方式：npm install @vue/cli -g  (项目常用)

   1. 安装完后直接：vue create vue-cli-dome   表示创建一个带脚手架的项目名字叫vue-cli-dome
   2. 选择手动：根据自己需求选择脚手架要带有那些功能包括版本， 方向键移动，空格键选择，回车确认
   3. 启动命令：npm run serve




## 创建vue2/vue3项目

1. 创建一个用于练习的项目(推荐)

   1. npm init -y

   2. npm i webpack@4.44.2 webpack-cli@3.3.12 webpack-dev-server@3.11.2 -D

   3. 更改脚本调式命令："dev": 'webpack-dev-server'

   4. 创建src 源码目录 和 public 目录

      1. public目录内创建一个index.html文件
      2. src 目录创建main.js 入口文件

   5. 引入cdn：

      ```js
      <script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
      
      // vue3
      <script src="https://unpkg.com/vue@next"></script>
      ```

   6. 安装处理html和vue文件的插件：vue2的vue-loader版本不要过高15.7.0最好,vue3则不能用过低版本

      1. npm install vue-loader@15.7.0 vue-template-compiler html-webpack-plugin@4.5.0 -D

   7. 创建 webpack.config.js 文件进行 webpack 配置

   8. Vue3项目则需要额外下载以下2个依赖

      1. npm add vue@next -D
      2. npm add @vue/compiler-sfc -D
      3. vue3的 vue-loader模块用新的方式导入：const { *VueLoaderPlugin* } = *require*('vue-loader')

   9. webpack.config.js配置

   10. ```js
       const {resolve} = require('path');
       
       const htmlWebpackPlugin = require('html-webpack-plugin');
       const VueLoaderPlugin = require('vue-loader/lib/plugin');
       // vue3的vue-loader插件引入方式：解构
       // const { VueLoaderPlugin } = require('vue-loader')
       
       module.exports = {
           mode:'development',
           entry:'./src/main.js',
           output:{
               path:resolve(__dirname,'dist'),
               filename:'main.js'
           },
           // 引用外部文件
           externals:{
               'vue':'Vue'
           },
           // 源码映射 (方便检查错误)
           devtool:'source-map',
           module:{
               rules:[
                   {
                       test:/\.vue$/,
                       loader:'vue-loader'
                   }
               ]
           },
           // 插件配置
           plugins:[
               new VueLoaderPlugin(),
               new htmlWebpackPlugin({
                   template:resolve(__dirname,'public/index.html')
               })
           ]
       }
       ```

   11. 开始练习



# vue

- Vue的核心：模板语法->核心库->模板编译_>渲染DOM
- 组件逻辑的本质:   就是一个对象，对象内部包含多种特定属性
- Vue将数据与 DOM 进行关联，并进行响应式依赖，开发人员只需要关注数据逻辑的操作

```vue
// 组件模板
<template>
    <div class='box'>
        <h1>{{title}}</h1>
    </div>
</template>

<script>
// 组件逻辑
export default {
  // 组件名称
  name: 'App',

  components: {},
  data () {
    return {
        title:'Hello Vue!!'
    }
  },
}
</script> 

// 组件样式
<style scoped>

</style>
```





## 组件化

- Vue组件化-> 核心 组件系统
- Vue利用ES模块化->完成Vue组件系统的构建
- 组件化：抽象小型-独立-可预先定义配置的-可复用的组件
  - 小型：每一个小单元尽可能都独立开发
  - 预先定义：小单元都可以先定义好，等待需要时导入使用
  - 预先配置：小单元可接受一些在使用时候需要的一些配置，根据可用性和可扩展性去决定可配置性
  - 可复用：一个抽象的小单元可以在多处使用，(复用性要适当考虑，不一定都要考虑复用性)
- 组件的最大作用是独立开发：预先配置，可以更好的维护和扩展



## 应用实例

- 用于注册全局组件，由Vue.createApp({})创建
- 组件实例暴露了很多方法如 fulter过滤器等，也可以进行链式调用因为组件实例也返回一个实例

```js
// 组件实例  main.js入口文件 vue3方式
const app = Vue.createApp({});

app.component('MyTitle',{
    data(){
        return{
            title:'I Love You'
        }
    },
    template:`<h1> {{title}} </h1>`
})

app.mount('#app');

// index.html页面
<div id='app'>
    <--- 组件-->
    <my-title> </title>
    </div>
```



## 根组件

- 根组件本质就是一个对象 {}
- createApp 执行时需要一个根组件 createApp({});
- 根组件是Vue渲染的起点
- createApp执行创建Vue应用实例时，需要一个HTML根元素 如<div id='app'></div>
- 而mount返回的是一个根组件实例
- 每个组件都有自己的组件实例，一个应用中的所有组件都共享一个应用实例
  - 当一个实例被创建时，它将data对象中的属性加入到Vue响应式系统中，该组件成员可共享应用该实例
  - vue自身也暴露了很多属性方法 大都以$开头，以便与自定义方法区别
- Vue并不是一个完整的MVVM

```js
// index.html
<div class='app'> </div>

// main.js
const RootComponent = {
    data(){
        return {
            a:1,
            b:2,
            total:0
        }
    },
    mounted(){
        this.plus()
    },
    methods:{
        plus(){
            this.total = this.a + this.b;
        }
    },
    template:`<h1> {{a}} +{{b}} = {{total}} </h1>`
}

// 返回应用实例
const app = Vue.createApp(RootComponent); 
// 返回根组件实例
const vm = app.mount('.app')

console.log(app,'app'); // 应用实例 挂载很多实例方法 
console.log(vm,'vm'); // 根组件实例 挂载根组件的所有属性
console.log(vm.a,vm.b,vm.total); // 1 2 3
```



## 生命周期函数

- 组件是有初始化过程的
- 在这个过程中 vue提供了特定的在，每个阶段运行的函数



## MVC

由后端启发而来

- M: Model  数据模型-> 操作数据库(对数据进行增删改查)
- V: View    视图层-显示视图或视频
- C：Controller  控制器层-
  - 逻辑层 数据和视图管理挂载和基本的逻辑操作，
  - API层，前端请求的API对应的是控制器中的方法
  - 前端请求URL ->控制器中的方法
  - Model层的方法 ->操作数据库->获取数据
  - 返回给控制器方法->响应回前端
- 前端MVC
  - Model: 管理视图所需数据 + 数据与视图的关联
  - View: HTML模板 + 视图渲染
  - Controller: 管理事件逻辑



## MVVM

- mvvm-> 驱动vm -> ViewModel
- M -> Model 数据保存和处理的层
- V-> 视图
- 案例没有写完，难懂，先不看



# 原理

## 模板语法

- template -> HTML字符串->Vue 特性->表达式/属性/指令
- Vue的模板语法遵循HTML特性
- 模板中直接写HTML都是能够被HTML解析器解析的
- 开发者编写的template ->分析HTML字符串->AST->表达式/自定义属性/指令->虚拟DOM-> render
- 插值{{}}, 插值表达式内部还可以直接放入函数
- attribute 属性,用于HTML属性中的插值(扩展特性)
- property 属性 ，在对象内部存储属性，用于描述数据结构

```js
// 练习使用js和html引入方式 所以一个js文件一个对象可以看成是不同的组件
// 组件x
const counts = {
    props:{
        title:String,
        author:Number
    },
    template:`
      <h1>{{title}}</h1>
      <h2>{{author}}</h2>
     `
}

const App = {
    data(){
        return{
            title:'This is Mustache',
            author:new Date(),
            content:'Hello xjj'
        }
    },
    // 注册组件x
    components:{
        counts
    },
    
    template:`
      <div>
        // 向组件X传递值 title和author
       <counts :title="title" :author="author" />
       <p :title="content">{{content}} </p>
      </div>
    `
}
```



## 内置指令directive

- Vue中 模板上的属性v-都是指令，指示-模板应该按照怎样的方式逻辑进行处理/渲染
- 另外开发者也可以自定义指令
- v-html,   可以解析元素标签 类似innerHTML，注意：不要使用v-html做子模版，也不要使用v-html去解析用户提供的内容，容易收到xss攻击
- {{xxx}}插值语法； 用于显示变量的值，也可接受简单的逻辑运算{{ xxx ? 'a' : 'b' }}
- v-bind: xxx='xxx';    用于绑定属性(变量)，这个属性变量可以是值或者url，简写 :xxx 一个单冒号
- v-on:click='方法名'；用于注册事件函数，简写：@click=''
- v-model='变量';   双向数据绑定
- v-for=' item of Array' key='index';   用于遍历数组或对象，v-for='item in obj' :key='item.id' 也可以，
- v-if='变量' / v-else if / v-else;  类似JS的fi /else 条件判断，true渲染节点，false不渲染节点
- v-show  隐藏/显示节点



## data深入了解

- Vue在创建实例的过过程中调用data函数
- 返回数据对象
- 通过响应式包装后存储在实例的$data中，并且实例可以直接越过$data访问属性，两者是相等的
- 内部可以通过this去访问实例,  $data本质就是一个响应式对象
  - vm.author = 'xiao mi';  通过点语法往vm挂载方法   无效
  - vm.$data.author = 'xiao mi'  ; 通过$data响应式对象添加方法 ， 添加成功(但是访问无效，需要到data内部去定义才行)

```js
const app = Vue.createApp({
    data(){
        return {
            title:'This is my title'
        }
    },
    template:`
      <h1>{{title}}</h1>
    `
})

const vm = app.mount('#app');
console.log(vm); // 挂载内置的API，开发者定义属性应尽量避免与这上面的方法属性重名
console.log(vm.$data.title);
console.log(vm.title);
vm.$data.title = 'Hello World';
console.log(vm.title);
vm.$data.author = 'xiaomi'
```



- data为什么一定要是一个函数，
  - 使其是一个独立的空间。每次返回的是一个新的对象
  - 避免组件之间的污染

```js
function Vues(options) {
    // data函数返回数据对象，将其包装后给$data
    this.$data = options.data();

    // 谁调用指向谁(data函数->数据对象)
    var _this = this;

    console.log(_this); // $data 响应式实例 a:1,b:2
    // 遍历这个对象数据
    for (let k in this.$data) {
        (function (k) {
            // 对其进行数据劫持 
            Object.defineProperty(_this, k, {
                // 外界访问/修改数据对象触发get/set
                get: function () {
                    return _this.$data[k];
                },
                set: function (newValue) {
                    _this.$data[k] = newValue;
                }
            })
        })(k);
    }
}
var vm = new Vues({
    data() {
        return {
            a: 1,
            b: 2
        }
    }
});

// vm.a = '99';
console.log(vm.a); // 1
// console.log(vm.b);
```



## methods深入

- Vue创建实例时，会自动为methods绑定当前实例this
- 保证在事件监听时，回调始终指向当前组件实例
- 外层要避免使用箭头函数 ，因为箭头函数没有this-不能正确指向Vue实例 (里层可以用箭头函数)，
- methods本质是一个容器，并没有被 Vue 暴露出来



# 模板引擎

- 安装：npm install mustache --save
- 或者使用 cdn加速连接 https://cdn.bootcdn.net/ajax/libs/mustache.js/4.0.1/mustache.min.js
- 使用：Mustache.render('模板', 数据)







































