vue基础2.6.14

注释：vue只关注视图层，无需直接操作DOM ，模型model ->View(视图) ->VM视图模型(事件监听)

```js
<div id="app"> 
    {{msg}} // v-Text 简写->插值语法 自动渲染data内的指定数据
</div>
<script src="../js/vue.js"></script>
<script>
    const app = new Vue({
        // el 是根组件实例挂载节点，(查询id为app的DOM,确定vue实例的使输出)
        el: '#app', 
        // 数据管理，所有data内部的数据会自动加入到Vue响应式系统
        data: {
            msg: 'hello vue'
        },
    })
</script>
```



## vue 指令集

-  Vue提供用于html元素的指令,以v开头，而以$开头的则是Vue提供的API

```js
/*
 v-cloak    在实例未完成挂载(渲染)前,隐藏Mustache插值语法
            [v-cloak]{display:none;} 以css样式书写，最后直接应用到元素上即可
 v-html     解析完整的元素标签包括内容，不推荐使用
 v-text     解析纯文本，->推荐模板语法Mustache，简写 {{}}
 v-model    双向数据绑定，用于input, textarea , select, checkbox,也可用于父子组件绑定数据同步
 v-bind     绑定元素自有属性如url等或data数据或样式类，简写 : 一个单冒号
            :class 绑定css样式类，可以是对象或数组,也可以是一个或多个，是否执行取决于条件是否成立
 v-on       用于注册事件处理函数，简写 @，注册时可以回调方式传参
 v-once     只渲染元素和组件一次，随后元素和组件将被Vue视为静态内容跳过,可用于不再更新的数据
 v-for      用于遍历数组/对象集合
 v-if       条件判断->元素是否创建并渲染-> 取决于 条件是否成立，可以与v-else if/v-else搭配
 v-show     用于隐藏/显示元素(DOM依旧存在 只是切换display:none/block;)是否执行取决于条件是否成立
 v-slot     插槽，简写 #
*/

// class绑定示例 以下写法都生效
<div :class="{'box1 box2':true}">{{title}}</div>
<div :class="{box1:true, box2:true}">{{title}}</div>
// class绑定数组方式 直接操作值， 不推荐
<div :class="{active}">{{title}}</div>
new Vue({
    el:'#app',
    data(){
        return {
            // 需要将css样式类 绑定到指定数据后使用
            active:'box1'
        }
    }
})

// v-for 分别遍历 对象 和 数组 以及 对象转数组
<p v-for='(value,key,index) in obj' :key='index'> {{value}}:{{key}}:{{index}}</p>
<p v-for='(item,index) in list' :key='index'> {{item}}</p>
<p v-for='(value,index) in Object.values(obj)' :key='index'> {{value}}:{{key}}:{{index}}</p>
// 对象转数组可以使用 Object.vlues/Object.keys/Object.entries

// v-for 遍历对象 假设是一个多层的对象数据
v-for='item in obj'   {{item.xxx}}
```



## 修饰符

```js
以.点语法调用，用于设置一个指令应该以何种方式绑定
/*
  .prevent        用于阻止事件默认行为，如a元素的自动跳转 submit的提交行为
  .stop           用于阻止事件冒泡行为
  .capture        指定事件应该以捕获方式运行
  .enter          用于指定键盘回车事件，搭配keyup
  .sync           双向绑定父子组件的数据，配合v-bind,常用于子组件同步修改父组件传来的数据,vue3已废弃
  .delete         按Del键时触发 搭配keyup
  e.keyCode       配合keyup查询键码 如:回车键 = 13
  .number         表单修饰符 转化为Number数值型
  .trim           去掉首尾看空格
  .lazy           将input事件切换到change事件
  .self           只有当前元素自身才能触发指定事件
*/

Vue可以自定义按键修饰符
@keyup.aaa='handler'
Vue.config.keyCodes.aaa = 65;
```



## Vue常用API

- 更多API参考官网 -> https://cn.vuejs.org/v2/api/#Vue-nextTick



## vue中的this

- Vue使用 this访问实例上的数据和方法，实例返回的模型对象 使用vm.$data.xxx可以查看Vue原型

- 1.不应该使用箭头函数来定义一个生命周期方法
  2.不应该使用箭头函数来定义 method 函数
  3.不应该使用箭头函数来定义计算属性函数
  4.不应该对 data 属性使用箭头函数
  5.不应该使用箭头函数来定义 watche 函数

- 原因：箭头函数绑定了父级作用域的上下文，this 将不会按照期望指向 Vue实例。也就无法访问实例上的方法属性

- 注意：axios中由于嵌套了层数，里层的this将不能正确指向实例，解决办法：外层用变量对this进行转存，另外一种就是直接使用箭头函数，因为箭头函数没有this 所以它指向的是运行时的上下文环境

  



## computed计算属性

- computed计算属性用于复杂计算求值，它依赖一个或多个值并产生一个新的值(结果)，并对结果进行缓存，除非依赖的值发生变动，否则使用之前缓存的值

```vue
<div id='app'>
   <!-- 将计算属性直接作为表达式使用 -->
    {{ myComputed }}
</div>

<script>
    const vm = new Vue({
        el: '#app',
        data() {
            return {
                a: 3,
                b: 5
            }
        },
        // 计算属性
        computed: {
            myComputed() {
                return this.a + this.b
            }
        }
    })
</script>
```





## watch侦听

- watch用于侦听一个目标数据的变化，一旦其发生修改变动，就会执行指定回调
- 当需要进行一个开销较大或数据发生变化时执行异步操作时 使用 watch

```js
<div id='app'>
     {{title}}
    </div>

const vm = new Vue({
    el:'#app',
    data(){
        return {
            title:'Hello World',
            firstName:{
                a:1
            }
        }
    },
    watch:{
        // 简写
        title(newVal,oldVal){
            console.log('新值' + newVal + '旧值' + oldVal)
        },
        
        // 完整写法
        firstName:{
            handler(newVALUE,oldValue){
                console.log('触发')
            }，
            immediate:true,  // 不必等数据变动，定义侦听目标时首次就触发
            deep:true // 深度监听 针对对象层级深时使用，数组无需使用深度监听
        }
    }
})
```



## 全局自定义指令

- 内置指令如果不能满足需求，则可以自定义指令directive
- binging对象包含属性：常用 binding.name表示指令名不包含前缀v-, binging.value表示指令绑定的值
- 方法函数
  - bind 只调用一次，指令第一次绑定到元素时调用，在这里可以进行一次性初始化设置
  - inserted: 被绑定元素插入父节点时调用(仅保证父节点已存在，不保证其已插入文档)
  - update:组件DOM更新世调用，指令的值可能发生改变也可能没有，需要进行新旧值比较
- 详情参考官网

```js
// 使用自定义指令
<p v-oot='{color:'orange'}'> Hello World </p>

// 注册指令名focus
Vue.directive('oot', {
    // 配置项 inserted插入，el(元素本身)，binding(对象) 指令传入的参数(颜色: orange)
    inserted:function(el,binding){
        // 根据指令参数设置元素的背景色
        el.style.background = binding.value.color
    }
})
```



## 自定义局部指令

- 注册在组件内，只有该组件内能使用

```js
// 使用自定义指令
<p v-oot='{fontSize:'22px'}'> Hello World </p>

// 对象形式 与Vue其他选项配置同级
directives:{
    oot:{
        inserted:function(el,binding){
            el.style.fontSize = binding.value.fontSize
        }
    }
}
```



## filter过滤器

- 过滤器可以传参也可以不传参，

```js
// 使用过滤器 uper 最终页面显示 hel123
<p> {{msg | uper('123')}} </p>

new Vue({
    el:'#app',
    data(){
        return {
            msg:'Hello World'
        }
    },
    // 局部注册
  filters：{
     uper:function(val,arg1){
       // val为过滤器绑定元素内的值(msg:'Hello World'), arg1为携带参数('123')
       return val.slice(0,3).toLowerCase() + arg1
    }
  } 
})

// 全局注册方式
Vue.filter('myfilter', function(val,arg1){
    return val.slice(0,3).toLwerCase() + arg1;
})
```



## 变更数组方法

- 变更数组与非变更数组都是Vue对原生数组做了一些功能包装的变异数组, 使其能够触发视图更新(响应式)
  - push();    向数组尾部追加一个元素
  - pop();    删除数组最后一位
  - shift();    删除数组最第一位
  - sort();       排序
  - splice();    删除数组指定位置的元素，并可以追加一个填充元素
  - reverse();   反转数组 



## 非变更数组方法

- 指不改变原始数组 而是根据原数组处理后返回一个新的数组，
  - filter();   过滤
  - concat();  连接
  - slice();   截取

```js
data(){
    return {
        msg:['xjj','heelo','nice'],
        good:['Good']
    }
}
methods:{
    change(){
        // 对于 非变更数组它返回一个新的数组，只能用其替换原来数组
        this.msg = this.msg.concat(this.good); //  ['apply', 'xjj', 'hello', 'Good']
    }
}
```



## 生命周期(重要)

注释： Vue 实例在被创建时都要经过一系列的初始化过程，在这个过程中 会调用一系列生命周期钩子函数，(即回调函数)

因此用户可在不同的阶段添加自己所需的代码方法，总结：使用户能够知道当前vue实例处在那一阶段

vue里面也是分同步异步的

1. beforeCreate ： 创建前 --> data与methods还未初始化; (实际开发几乎不用)
2. created() ：
   - 实例创建完成-->能够访问data与methods中的数据；还不能访问vue渲染后的DOM比如读取/更改数据，因为属性虽已绑定但是DOM还未生成   (常用:如用于进入页面时接口请求)，只执行一次，后面不刷新不执行
3. beforeMount  ： 模板编译/挂载前 绑定el确定了渲染范围，但还没有渲染DOM，还拿不到DOM内部的内容
4. mounted : 
   - 模板编译/挂载后(可以拿到DOM渲染后的同步元素)  (常用) 要操作DOM最早要在这个函数中进行
   - 兄弟组件的事件总线也需要在这个生命周期函数获取
5. beforeUpdate ： 更新前 检测到修改，data数据已是最新，但还未更新到页面,不用,易引发问题
6. updated      ： 更新后 (可以拿到DOM的异步元素) 页面数据发生修改时获取,不用容易引发问题
7. beforeDestroy：
   - 实例 销毁前，常用于一些善后工作 即销毁前要做些什么，比如定时器和hashchange事件还有滚动事件等无法自动销毁，就需要用到这个,内部数据和方法依然可用
8. destroyed :
   - 实例 销毁后，还是能够访问data与methods，一样能进行销毁操作，只不过它不能访问vue渲染后的DOM，相当于使用它就是中断渲染
   - v-if和v-show 也会触发这两个销毁函数
9. ![](D:\05-code-word汇总\5-项目详细笔记\前端知识整理\js基础深入.assets\17生命周期.png)



## ref获取DOM

- ref  指令 用于给元素或子组件注册引用信息, 引用信息将会注册到父组件的 $refs对象上
- 用于普通标签元素是获取DOM，用于组件标签则是获取子组件实例，相当于绑定了子组件的this

```js
父组件
<template> 
    <p ref='pp'>我是p标签 </p>
    <son ref='box' />
  <button @click="btnk">点击</button>
   </template>

import son from './components/son.vue'
export defaults {
    components: {
        son
    },
    methods: {
        btnk() {
           // 获取组件标签DOM
            console.log(this.$refs.pp) 
          // 获取子组件实例 假设子组件data数据源有title 属性
           console.log(this.$refs.box.title)
        }
    }
}

```



## $nextTick异步

注释：setTimeout的升级版   实际还是异步函数

- $nextTick(){};  在它前面的所有同步处理的数据在页面渲染完成后执行



## 单元测试Jest

- 官网：https://www.jestjs.cn/docs/getting-started
- 第一步：安装 npm install jest -D 开发环境
- 脱裤子放屁多此一举，用到再学

```js
// 要测试的函数
function add(a, b) {
    return a + b;
}

// test启动的条件 为true即可，这算是一个测试用例
test("1+1 = 2", function() {
    // 测试所需数据
    var a = 1;
    var b = 2;
    
    // 测试动作(调用要测试的函数)
    const result = add(a, b);
    
    // jest 断言
    expect(result).toBe(3);
})

// 运行测试
node jest
```



## Vue中的单元测试

- vue/cli创建项目时可以选择集成 Unit Testing 再选择测试方式 jest
- 直接安装方式：vue add @vue/unit-jest
  - 安装完毕会自动生成一个 test 文件夹 并创建一个js文件，用于测试我们的Vue组件
  - 启动测试命令：npm run test:unit

```js
import { shallowMount } from '@vue/test-utils'
import HelloWorld from '@/components/HelloWorld.vue'

describe('HelloWorld.vue', () => {
  it('renders props.msg when passed', () => {
    const msg = 'new message'
    const wrapper = shallowMount(HelloWorld, {
      propsData: { msg }
    })
    expect(wrapper.text()).toMatch(msg)
  })
})

// 这里要测试HelloWorld.vue组件是否有 props['msg'] 属性
```



------

# axios的使用

- axios基于Prmise用于浏览器和Node的http客户端，由如下特性：
  - 支持浏览器和node.js
  - 支持Promise
  - 支持请求拦截和响应
  - 自动转换JSON数据
  - 响应结果会被包装成一个Promise对象，数据存在data内

```js
// get请求 传递参数方式
  通过url拼接
  await axios.get('http://localhost:3000/list?num=2')
  通过params对象 选项传递参数
  await axios.get('http://localhost:3000/axios',{params:{id:789}})
```



## axios的方法

```js
axios.default.timeout = 3000;  // 超时设定
axios.default.baseURL = 'http://localhost:3000/app'; // 设置默认基地址
axios.default.headers['mytoken'] = 'xxx';  // 设置请求头
```





## axios基地址配置与调用

- 普通全局注册

```js
---main.js内注册
import axios from 'axios' //导入
Vue.prototype.$axios = axios // 在vue原型上添加axios
axios.default.baseURL = 'https://autumnfish.cn' //配置了基地址
-----组件内使用
//不需要再导入axios
this.$axios({
    url:'只需要跟参数就行了，域名不需要再写main.js已配置好基地址'
  //比如：https://autumnfish.cn/personalized/newsong 只需要personalized/newsong 即可，
})
```



## axios的单独封装

注释：实际开发中用这种比上面的全局注册还要好

- 在src中创建一个utils工具文件夹 --> 并创建一个request.js文件
- request.js专门封装用来处理网络请求,如果项目比较大，可以继续拆分成多个不同的网络块
- 将axios的创建单独放到这个文件内
  - 自定义一个实例副本 并暴露出去
  - 组件内导入/main.js导入，--使用导入名发送axios请求即可，axios内部的 url不用再进行基地址拼接了 只需要参数就行，注意:组件内使用基地址赋值的变量保持不变：可以直接用导入名进行赋值或者采用开发环境配置的基地址赋值都行
- 然后在main.js中导入

```js
import _fetch from 'axios'

```





## axios拦截器

- 拦截请求器：发起请求前要做些什么
- 响应拦截器：指数据响应成功后要做些什么

```js
// 请求拦截器
axios.interceptors.request.use(function(config){
    // 请求发起前做些什么
    config.headers.mytoken = 'Hell world'
      return config;
}),function(err){
    console.log(err)
}

// 响应拦截器
axios.interceptors.response.use(function(res){
    // 对响应结果做些什么
    return res.data
})
```



## async与await

- 异步编程的终极方案,结合Promise/axios使用
  - async让一个函数成为异步函数，await 异步等待响应结果

```js
async function queryDate(){
    let res = await axios.get('http://xxx');
}
```



## async处理多个请求

```js
async function queryDate(){
    var res1 = await axios.get('http://xxx');
    // 第二个请求依赖第一个请求返回的结果作为参数值
    var res2 = await axios.get('http://xxx?info' + info.data )：
    return res2.data
}
```



# 单元素动画

注释：单元素动画就是只有一个标签的动画，没有兄弟节点.   多元素动画差不多 使用选择器进行操作就行

vue提供了transtion的组件功能，可以给任何以下元素添加进入和离开动画

- 条件渲染v-if，  
- 条件显示 v-show,  if或者show二选一都可以
- 动态组件
- 组件根节点

```vue
 <style>
  .box {
      width: 100px;
      height: 100px;
      background: pink;
  }
  /* 进入动画 */
  .xxx-enter-active,
  /* 离开动画 */
  .xxx-leave-active {
      transition: opacity 1.5s;
  }
  /* 进入和离开时的透明度 */
  .xxx-enter,
  .xxx-leave-to {
      opacity: 0;
  }
  </style>
</head>

<body>
 <div id="app">
   <button @click="bol = !bol">点击切换</button>
   <transition name='xxx'> //必须使用transition标签包裹，并起个name名
       <div class="box" v-show="bol"></div>
   </transition>
 </div>
 <script src="../js/vue.js"></script>
 <script>
    new Vue({
       el: '#app',
       data: {
           bol: false
       },
       methods: {
       }
   })
</script>
```



# 组件

注释：封装一段功能代码， 可复用 易于维护

1. 组件尽量避免使用驼峰命令，如果使用了驼峰命令 使用时需要用短横线分割一下，或则直接采用短横线命名



## 安装组件依赖

- npm install -g @vue/cli 

- 如果安装失败 就以管理员身份运行cmd或者window自带的命令工具 shift+右键

- npm cache clean -f    ： 如果失败 清除缓存继续安装

- set-ExecutionPolicy RemoteSigned  ： 如果电脑系统禁止脚本 就输入这句命令

- npm install @vue/cli-service-global -g ：安装单文件依赖包

  ------

  注释：使用淘宝镜像国内服务安装

- npm install -g cnpm --registry=https://registry.npm.taobao.org 淘宝镜像

  - cnpm install -g @vue/cli   淘宝镜像安装好后就使用cnpm安装
  - cnpm install @vue/cli-service-global -g

- 安装成功后使用：vue serve 文件名    : 单文件运行命令



## 组件注册

- 全局注册 Vue.component('xxx',{ } )
- 局部注册  components:{}

```js
// 全局注册 一般放一个专门的文件内，导入-> 注册 -> 使用

import Home from '../xxx/home.vue'
Vue.component('Home',Home)

// 注册好的组件可以在任意组件内使用
<Home> </Home>

-----------------------------------
    
// 局部注册 导入-> 注册 -> 使用
import xxx from './xxx'
export defaults {
    components: {
        xxx
    }
}
// 使用:只能在当前页面使用
<template> 
    <xxx> </xxx>
   </template>
```



# 组件传值

- 组件本身就是一个Vue实例，当我们向组件传递属性对象时。它会成为组件实例上的属性
- 而组件有多种传值方式(父子，兄弟，子父)，可以获取到这些传递过来的值

## 父传子props

- 组件传值方式有很多种，
- 传值规则：单向流，
- 传值类型可以是如下几种：
  - Number,  Boolean, Array, Object, String Function, Promise, 
  - 字符串数组形式，对象描述形式
- 当前组件接收到的 props 对象时。Vue 实例代理了对其 属性特性的访问
  - 因此我们可以对 Props进行设置，描述我们期望得到的特性

```js
// 父传子 动态传值
<son :title='msg' />
data(){
    return {
        msg:'Hello World'
    }
}

------子组件接收-------
// 数组字符串形式接收
props:['title']

// 对象形式
props: {
   // 简单描述：title属性 期望一个字符串类型
    title: String, 
    // 对象形式描述 
    title: {
       type: Number, // title属性 期望一个Number类型
       default: 0，// 如果参数未传, 给它一个默认值:为0
       required: true, // 该参数必传
        // 检测值 是否符合预期
       validator: function(value) {
          return value >= 0
      }
   }
}
```



## 子传父emit

- 为了保持数据单向流动，子组件不允许直接修改父组件数据，只可传递值，由父组件接收并进行变更处理

```js
// 子组件menu-item 传值
this.$emit('事件名xxx',参数)

// 父组件页面-子组件实例@并利用$event接收传递来的值,不写$event也可以默认就是$event
<menu-item @事件名xxx='change($event)' />
// 父组件使用传递的值
 methods:{
    change(val){
      // 处理逻辑
   }}
```



## v-model 与 .sync

- 父传子还可以通过 v-model 或 修饰符 .sync进行双向绑定
- 区别：父组件无需再 @子组件传递的事件去进行处理, 直接由子组件 this.$emit() 完成修改
- 如果是需要往数组追加数据 v-model 就不太适用了
- .sync 与 vue3已废弃，参考官网这里就不例举了

```js
父组件
<template>
    <Son v-model='msg'> </Son>
   </template>
import Son from './components/son.vue'
export defaults {
data() {
   msg: 'Hello Vue'
 }
}

子组件
<template>
    <button @click='handler'> this.$emit </button>
   </template>
export defaults {
    props: ['msg'],
   // model设置
    model:{
        prop:'msg',
        event: 'change'
    },
   methods: {
       handler() {
          // 直接调用事件event 并传递参数=完成修改，无需在父组件进行操作
           this.$emit('change','You is Vue')
       }   
   }
}
```



## 兄弟组件通信

- 采用事件总线 eventBus 的模式处理非父子组件的通信传值
- 传 xxx.$emit 触发,    接  xxx. $on  监听实例上的自定义事件
- 销毁；xxx.$off('组件名')
- vue3  已废弃，直接使用vuex

```js
// 创建事件中心 一般放一个专门的文件
import Vue from 'vue'
var hub = new Vue();
export default hub;

组件导入 事件总线 文件
// 第一步：组件一向组件二传值
hub.$emit('组件二的组件名',参数)

组件导入 事件总线 文件
// 第二步：组件二 在mounted函数勾子阶段接收值
hub.$on('组件二自身组件名',(val) =>{ // 处理逻辑})
    
// 根组件销毁事件中心
    methods:{
        handle(){
            hub.$off('要销毁的组件名1');
            hub.$off('要销毁的组件名2');
        }
    }
    
--------------------------------------
全局事件总线 挂载到Vue 实例 main.js入口js文件
Vue.prototype.$hub_v = new Vue()

组件一 传值 给组件二, 假设组件二是一个全局组件叫 List
this.$hub_v_$emit('List',this.msg)
    
组件二 mounted 接收
this.$hub_v.$on('List',function(val) {
    console.log(val)
})
methods:{
    btns() {
     // 关闭事件总线
     this.$hub_v.$off('List')
    }
 }
```



## 多层级组件通信 provide

- 采用注入方式： provide 注入， inject 接收
- 用于祖孙组件通信，无论层级多深，只要是嵌套的上下级或祖孙关系，都可以注入依赖
- 依赖注入后，孙子组件可接收，子组件同样也可以接收
- 注：如果注入依赖后,又给孙子或者儿子组件的标签 绑定了属性且假设绑定的属性吗与注入的属性同名,那么注入优先,会覆盖 标签上的 bind绑定属性

```js
组件A
export defaults {
   // 必须函数形式 return一个对象 this才能指向当前组件实例
    provide(){
        return {
            title: 'is Vue',
            comA: this.msg
        }
    },
   data() {
      return {
         msg: 'Hello Vue Hi'
       }
   }
}

组件C
export defaults {
    inject: ['title', 'comA']
}
// 使用
<template> 
    title:{{title}} : comA: {{comA}}
  </template>
```



## 多层级组件通信attres

- $attres;  包含了父作用域中不作为 prop 识别 (且获取) 的 attribute 绑定
- 说人话就是父组件传递的值在儿子组件没有被显式接收的(因为儿子组件不需要这个值)，都会流入到 attres，而attres 可以通过组件标签进行绑定继续传递到孙子组件，   如果父组件通过props 接收了则不会被流入到孙子组件
- 所以通过这种方式也可以将值传递到孙子组件





## 组件缓存keep-alive

注释：使用keep-alive 标签包裹组件 即可对组件进行缓存策略

- keep-alive有三个属性：
  - include  指定要缓存的组件
  - exclude  排除指定组件(使其不被缓存)
  - max    最大缓存数量

```js
<keep-alive include='home'>
   <router-view />
</keep-alive>
```



# 插槽

- 用于内容分发，而 slot 则承载内容分发的出口(展示)
- 插槽内容可以是字符串、动态值、其它组件

## 默认插槽

- 默认插槽 前提是只有一个插槽

```js
// 子组件 menu-item
// 组件标签上的url只能在组件内部的选项配置访问，无法在内容分发处访问
<menu-item url='/xxx'> 
   传给子组件默认插槽 <span>可以放标签 </span> 还可以放其它组件。 {{url}}这里无法访问标签上的url
</menu-item>

// 默认插槽会有一个隐藏的名字 default
<slot> 内容会默认分发到这个插槽 且覆盖插槽内的原始内容 </slot>
```



## 具名插槽

- 采用name属性标记

```js
// 子组件menu-item
<menu-item>
    <p slot='header'>具名插槽header </p>
    <p slot='footer'>具名插槽footer </p>
    <p>默认插槽 </p>
   </menu-item>

// 子组件内部template使用
<slot name='header'> </slot>
<slot name='footer'> </slot>
<slot> </slot>
```



## 作用域插槽

- 指父组件对子组件的内容进行加工处理，就需要用到作用域插槽
- slot-scope作用域插槽，已废弃
- 直接使用具名插槽进行bind绑定传递，父组件内通过子组件标签 v-slot:xxx='{ooo}'; 进行解构接收即可

```js
// 子组件menu-item
<menu-item :title='list'>
    // 在插槽模板内接收值 并进行加工区
 <template slot-scope='slotProps'>
   <strong v-if='slotProps.info.id === 2' style='color:red;'>{{slotProps.info.name}} </strong>
    <span v-else> {{slotProps.info.name}} </span>
    </template>
   </menu-item>

Vue.component('menu-item',{
    props:['title'],
    data()({return{}}),
    template:`
      <div>
        <li v-for='item in title' :key='item.id'> 
             <-- 将要处理的值绑定到插槽 -->
             <slot v-bind:info='item'>{{item.name}} </slot>
          </li>
        </div>
   `
})

// 父组件
new Vue({
    el:'#app',
    data(){return {
        list:[
            {
                id:1,
                name:'apple'
            },{
                id:2,
                name:'banned'
            }
        ]
    }}
})
```



# Vuex

- 问题：
  - 当多个组件共享统一状态时：传参方式对于多层嵌套的组件会非常繁琐，兄都组件则只能采用事件总线
  - 来自不同视图的交互行为需要变更同一状态时：采用父子组件数据引用或者事件拷贝，缺点:繁琐,难以维护
- 因此  Vuex应运而生：
- 实现组件全局状态(数据) 管理/共享的一种机制,可以方便的实现组件之间的传值和数据共享
- 易于维护和管理，且数据是响应式的，能够实时的保持数据与页面的同步
- vuex 实际就是采用了单例模式 架设中间层代理(函数) 使任何位置的组件都能共享状态和同步的值
- 适合存储到Vue的数据
  - 组件共享的数据
  - token
- 当项目足够大且数据复杂时可以使用vuex 去替代常用的组件传值方式



## Vuex的使用

- 那里需要那里导入，
- 或者注册到Vue.use(Vuex) -> 组件调用：this.$store.state.xxx或者采用辅助函数，可以更方便的调用 Vuex
- vuex 也遵循 vue的响应式状态规范，所以最好在state 一次性生命所需属性，如果没有提前声明，后续添加属性同样需要使用 Vue.set()



## vuex的函数

每个状态都有一个辅助函数 以map开头, 辅助函数的本质是将所处作用域映射为 store.dispath或 store.commit

- state:  驱动应用的数据源
  - mapState  辅助函数 可以输出多个状态。支持展开运算符, 注： 需要在计算属性 computed 内调用
- getters: vuex的计算属性，与computed 功能相同 ，
  - 辅助函数：mapGetters,   注： 需要在计算属性 computed 内调用
- mutation: 同步修改state 中的状态,  
  - 唯一更改state状态的途径，类似于事件+回调， 注：需要在组件的 methods 内调用
- action；  支持异步操作-(返回一个Promise可以进行then调用)> 提交给 mutation
- modules； 分割子模块  为每个子模块都开辟独立的 state，getters,mutatiion,action, 甚至可以继续分割子模块，最后将子模块集中导入到vuex
- mutation与action 都可以接收对象方式提交参数，详情参照官网

```js
import Vue from 'vue'
import Vuex from 'vuex'

注册到Vue 实例,实现全局调用
Vue.use(Vuex)

export defaults new Vuex.Store({
  state: {
    a: 5,
    b: 2
  },

 getters: {
   doneTodos: function(state) {
     return state.a + state.b
  }
 }

})

---------------组件调用-- 普通方式-----------------
store使用:     组件模板: $store.state.xxx    组件逻辑：this.$store.state.xxx
mutatioin使用: 组件逻辑： this.$store.commit('回调', 回调参数)
action使用：   组件逻辑：this.$store.dispath('要提交给mutation的对应函数名',参数)
    
---------------组件调用--辅助函数方式---------------
import {mapState, mapGetters, mapMutations, mapAction}
```



## vuex->modules

- 分割子模块, 默认是注册在全局命名空间的
- 且可以开启命名空间 namespaced: true，避免子模块同名污染，使其具有i更高的封装性和复用性

```js

```



## Vuex遵循规范

- 应用层级的状态应该集中到单个 store 对象中
- 提交 **mutation** 是更改状态的唯一方法，并且这个过程是同步的
- 异步逻辑都应该封装到 **action** 里面
- 对于大型应用，我们会希望把 Vuex 相关代码分割到模块中。下面是项目结构示例

```vue
├── index.html
├── main.js
├── api
│   └── ... # 抽取出API请求
├── components
│   ├── App.vue
│   └── ...
└── store
    ├── index.js          # 我们组装模块并导出 store 的地方
    ├── actions.js        # 根级别的 action
    ├── mutations.js      # 根级别的 mutation
    └── modules
        ├── cart.js       # 购物车模块
        └── products.js   # 产品模块
```



# 脚手架创建

注释：脚手架 Vue.cli

- 先安装vue与vue cli

- vue create 项目名 ：项目名尽量用英文

- 选择需要的插件和需求： 回车安装

- cd 项目名：  切换到项目文件

- npm i   : 安装所有依赖包

- npm run serve : 运行命令即可启动项目

  

## 脚手架文件目录说明

注释：我们写的代码都是放在src里面的

- vue.config.js  ： 项目webpack 环境配置
- public  :  vue入口html
- src     :  源码目录
  - App.vue: 根组件
  - components: 公共组件
  - assets:样式css’图片、静态资源目录，会被打包工具构建
    - imsges: 图片资源
    - logo  : 图片资源
  - router  : 前端路由
  - store   :   vuex
  - views   :    组件目录
- static   :  纯静态资源 如图片、不会被打包工具构建
- test     ： 测试文件目录
- build    ：webpack 打包输出文件
- utils    :  工具库
- main.js  :  入口js文件， 主要作用是初始化vue实例并使用需要的插件
- gitignore: git目录 可以将一些不需要git管理的文件添加到这里
- babel.config.js    :高版本js转换低版本js 照顾浏览器兼容性
- package-lock.json  : 下载的依赖包记录
- package.json       : 依赖管理和脚本调式
- README.md          ： 说明文件  
- eslintrc.js        :  es语法检测



## main.js文件说明

- maxin.js : 入口js

```js
import Vue from 'vue' // 导入vue
import App from './App.vue' // 导入根组件
import router from './router' // 引入router
import store from './store' //引入vuex

Vue.config.productionTip = false // 是否开启语法或其他提示 默认不开启

// 创建vue实例 传入对象
new Vue({ 
  router, // 挂载路由
  store, // 挂载vuex
    
  // render函数 渲染(传入App根组件)
  render: h => h(App) 
  // 将实例挂载到入口html文件的根节点 类似 el：‘#app
}).$mount('#app') 
```





## 图片src的变量更换问题

- 在data数据源内：当将src 链接或路径 进行变量绑定时， webpack将不能识别
- 解决办法：myImage2: require('./路径')； 给变量加个require路径即可正常解析

## 关于Vue中的css

- 子组件与父组件之间如果有相同的类名，互相引入后css样式就是全局作用相互影响
- scope :style加scope限制css只在当前组件生效，如果要突破限制去访问UI库的样式,需要加深度前缀: /deep/
- vue中src图片的引用, 路径前缀必须去掉src,用@代替,webpack才能正常解析

```js
<img src="@/src/assets/img/login_icon.png" alt=""/> // 报错因为路径前缀有src导致webpack无法编译
    
<img src="@/assets/img/login_icon.png" alt=""/> // 正常编译成功
```



## vue深拷贝

- JSON.方式一般应对普通对象足够了，如果对象有多层嵌套则需要封装一个深拷贝的函数

```js
//  使用JSON方法将数据解析成字符串，之后再转换成JSON对象数据
1 var vs=[1,2,453,12,432]
2 var gets=JSON.parse(JSON.stringify(vs))
3 gets.push(0)
4 console.log(vs)
//此时vs值不会变化，两个值是独立存在的
```



# 跨域问题

注释：跨域一般后端处理 比较方便 前端处理比较麻烦

- JSONP   ：将不同源的服务端请求写在script标签属性中，因为src属性不受同源策略的影响，
- webpack服务代理： 只能在开发环境有用  上线还是需要后端处理
- 参考 webpack 笔记 的代理跨域





## SPA单页应用

- 加载单个web应用页面，只在用户交互时动态更新页面程序和内容
- 通过 vue-router进行程序路径切换，而无需重新加载整个页面

# 路由

注释：路由就是一个指向，将路径指向相应的组件地址，根据不同的路径切换去显示对应的组件，

传统的监听页面变化是使用侦听器 并利用hashchange事件 监控页面hash值（#号后面的锚点部分）的改变进行切换，本质是前进后退，路由步骤如下

## 路由的配置

- 安装 npm install vue-router
- 导入->注册->路由实例化-> 挂载到 Vue实例
- 使用  ： 路由项配置好之后 需要在哪里展示路由出口, 就将路由出口放入哪里 <router-view />
- vue/cli脚手架也可以直接集成 vue-router
- 路由属于 Vue 的插件应用
- 更多的路由组件信息用到时参考官网

```js
// 路由文件夹 router 项目开发时可以将路由拆分模块化,每个开发人员独立用一个模块,最后集合到总路由
import Vue frm 'vue'
import VueRouter from 'vue-router'

// 注册路由插件
Vue.use(VueRouter)

// 路由列表
const routes = [
   // 路由作为对象添加到这里
    {
        path:'/xxx', // 访问路径
        component: xxx // 要展示的路由:必须与导入的组件名保持一致
    },

]

// 路由实例化
const router = new VueRouter({
    // 将路由列表作为对象传入
    routes
})

export default router

// 最后将路由实例挂载到 main.js的 new Vue实例上
import router from './router/.index.js'
new Vue({
    router,
    render: h => h(App)
}).$mount('#app')
```



## 编程式导航(跳转路由)

a标签或者router-link标签都可以跳转，但是实际开发我们使用下面这种更方便：

- 常见的编程式导航
  - this.$router.push();   通过路由跳转到...指定页面
  - this.$router.go();   通过路由回退到...指定的历史页面
  - 编程式导航可以携带简单参数

```js
// router-link 也可以携带参数 (不推荐)
<router-link :to=" '/homes/5' "

// 编程式导航 (常用)
<button @click='toHome'> to Home </button>

export default {
    methods: {
        toHome() {
           // 编程式跳转
           this.$router.push('/homes/5')
        }
    }
}
```





## vue中的Element UI库

1. 安装：  npm i element-ui -s
2. 引入：  在main.js 中引入， 如下：
3. 实例开发中推荐按需引入 可以减少打包体积

```js
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
Vue.use(ElementUI); //全局注册
```





## default active默认显示

注释：elementUI的属性， 默认显示当前路由页  不受页面刷新影响

```js
如果直接写死默认显示某一个元素,在刷新页面时会丢失当前选中项，此时使用$route.path；默认选中当前
 // 搭配 $route.psth 默认选中当前路由下的路径
<xxx :router='true' :default-active="$route.path"> </xxx>
```



## $router与$route (重要)

- this.$router： 属于路由实例化对象,整个路由的管理者，管理跳转，对象属性如下
  - 包括整个路由信息，其中就有 route 当前路由信息
  - 常用的属性有 this.$router.push()   用于路由跳转
- this.$route: 获取当前传入路由的基本信息，对象属性如下
  - fullPath:   当前路由信息，包括路径 url 上的参数等
  - meta :    路由元标签
  - name : 路由具名
  - path:  路由路径 包括 url 参数
  - params : 当前路由的参数对象,如果跳转前有携带参数对象的话,注：params只能配合 具名路由使用name,并且携带的参数必须是对应路由允许的范围，否则刷新就会丢失参数
  - query : 当前路由的参数对象,如果跳转前有携带参数对象的话,注：可以配合path 也可以配合name 且携带的参数会暴露在 url 地址栏
  - params 类似 post请求  query 类似 get 请求





## 路由重定向

注释：

```js
const router = new VueRouter({
  routes: [
     {
     //跟path是一样的，如果需要网址显示别名时可以用它，一般不用
      alias:'/findxxx',
      // 全路径
      path:'/',
      // 当用户访问没有指定url,而是全路径 /时,向到find 组件页面
      redirect:'/find'
    },
      {
      path:'*', //匹配所有如果没有该网址
      redirect:'/find' // 就重新重定向到find页面
    },
    // 以下为正常访问页面
    {
      path: '/find',
      component: find
    },
    {
      path: '/news',
      component: news
    }
  ]
})
```



## 嵌套路由children

- 指 某个路由下面还有嵌套的子组件 利用 children配置
- 配好以后需要将路由放入到父组件中

```js
const router = new VueRouter({
    routes:[
        {
            path:'/xxx',
            component:xxx,
            // 嵌套子路由
            children:[
                {
                   // 这里加斜杠可以单独访问，不加斜杠需要拼接父级再跟上子级如 /bar/xoxo
                    path:'/xoxo',
                    component:xoxo
                }
            ]
        }
    ]
})
// 父组件 bar
<router-view> </router-view>
```



## 命名路由

- 优点：防止路径访问错误，没有硬编码的url, params硬解码

```js
routes:[
    {
        path:'/user/:username',
        // 命名
        name:'user', 
        component:user
    }
]
```



## 动态路由

- 动态路由以冒号开头，其它页面跳转该路由组件时就可以携带参数
- 之后页面可以通过$route.params, 或$route.query 获取动态路由url携带过来的参数
- 由于$route方式获取动态路由的参数耦合度太高，vue又提供了路由组件传参模式props
  - props可以是对象，布尔true, 函数，如果是对象，那么接收也是对象的键值(值)

```js
const router = new VueRouter({
    routes:[
        {
            // 访问user 路由需要携参数 title，(也可以指定携带多个参数,以&分割即可)
            path:'/user/:id&title',
            component:user,
            // 开启路由组件传参 route.params会被设置为组件的props
            props:true
        },
    ]
})

A组件跳转去B组件时 传参
this.$router.push({
    name:'user',
    params:{
        id:5,
        title:'Hello Vue'
    }
})

// user组件
可以通过$route.params / props 接收参数
//路由组件模式获取参数，  路由组件则使用props接收
export default {
    // props接收
    props:['it','title'],
    data() {
        return{}
    }
}

```



## 路由配置模式传值

- query   和 params
- query + path 搭配时 动态路由的指定参数只能携带在path 上 且只能携带一个，额外参数可以放在 query对象内
- params + name 只能搭配具名路由，动态参数可以全部放在params 对象内，且可以携带多个参数

```vue
假设路由如下设置
{
  path:'/list/:id/,
  name:list,
  component:List
}

// query 类似get 请求，参数会显示在 url 地址栏, 用于简单参数
this.$router.push({
    path:'/xxx/5', // path 与 name 不能同时存在
    name: 'xxx' // 具名路由
    query:{
        title: this.title
    }
})

---------------------------------------------
假设路由设置如下
{
  path:'/list/:id&title',
  name: 'lsit',
  component: List
}

// params 类似 post 请求 用于多个数据时使用
this.$router.push({
   // params 传参只能配合具名路由name ，使用且携带的参数必须是对应路由允许范围,否则刷新丢失
    name:'xxx', 
    params:{
        id: 5,
        title: this.title
    }
})
```



## 路由元信息meta

- 用于给路由添加一个标记：可以用这个标记判断一些逻辑，比如权限

```js
const router = new VueRouter({
    routes:[
        {
            path:'/user',
            component:user,
            // 路由元信息，随意设置一个标记 ：表示访问该路由需要token验证
            // 那么在axios请求拦截器内就可以通过判断路由是否有这个标记，有就给它携带上token
            meta:{
                nodeTo:true
             }
        }
    ]
})
```



## 监听路由

- 如果需要根据路由参数的变化请求不同的后端数据，可以采用watch 监听路由，
- 或者获取参数后进行判断再请求,
- 或者直接再路由守卫里面进行操作

```js
watch: {
    // 监听路由对象 只要路由对象的属性发生变化就会触发
    $route() {
        if(this.$route.params.id === 5) {
            // 发起数据请求
        }
    }
    deep: true
}
```





## 路由导航守卫Hook

- 进入和离开路由时的处理,属于异步解析，类似于axios的请求和响应拦截器
- 主要用来做一些过渡动效 和权限设置，分为前置导航守卫和后置导航守卫
- 前置导航守卫
  - to   去那里 (相对于上一页面而言，去到的就是当前页面)
  - from  从那里来( 也就是上一个页面的信息) 
  - next  (可选属性 )是否放行(是否允许跳转到下一个要去的路径 即 to)，逻辑不重复前提下可以调用多次

```js
// 前置导航守卫
const router = createRouter({});
router.beforeEach(function(to,from){
    // 做些什么
})

// 后置导航守卫
router.afterEach(function(to,from) {
    // 做些什么
})
```



## 路由独享守卫

- 在路由列表直接定义一个守卫

```js
const router = new VueRouter({
    routes:[
        {
            path:'/user',
            component:user,
            // 进入路由时触发，进入后无效
            beforeEach:function(to,from){
                // 做些什么
            }
        }
    ]
})
```



## 路由滚动

- 传统方式通过window下的属性实现文档坐标滚动
- 而路由为我们提供了特定的方式

```js
const router = new VueRouter({
    routes,
    scrollBehavior(to,from, savedPosition) {
       // 这样就是每次路由进入组件页面坐标都是顶部 x: 0, y: 0
        if(savedPosition) {
            return savedPosition
        }
        // 也可以直接return 一个坐标对象,假设路由进入的页面有滚动条的话
        return {
            x: 0,
            y: 200
        }
    }
})
```





## 路由懒加载

- 异步的按需加载路由对应文件

```js
异步加载组件无需额外导入
const routes = [
    {
        path: '/home',
        name:'home',
        // 路由懒加载
        component: () => import('../components/home.vue')
    }
]
```





## 路由模式

- 默认hash模式，路径后面待#号，可以切换到 history 模式,
- history 模式不带#号
- 缺点：打包后需要后端配合设置，负责访问路径找不到对应文件 404

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)
const routes = []

const router = new VueRouter({
    mode: 'history',
    routes
})

export default router
```





## 时间戳转换插件moment

注释：用于时间转换，歌曲时长处理等等

官网：https://momentjs.bootcss.com/

- 下载-- 导入--使用

```js
npm install moment --save  //下载
import moment from 'moment' // 导入
```

- 后端一般给到的时间格式是：时间戳或者 Date.now()
- 使用过滤器全局注册的时候写在过滤器内部：Vue.filter('方法名',function(参数){return moment('mm:ss')})

```js
{{time}};时间戳： 556656897838
data:{time:''}
cerated(){ // 时间戳转换 根据需要转 只要分秒就只写分秒格式
  this.time = moument(556656897838).format('YYYY年MM月DD日 HH:mm:ss') // 时间格式
}
-------使用方法 -方法需要写在过滤器内部
// 使用方法跟过滤器一样  实参 | 方法
<td class="i4">{{ item.song.duration | formatTime }}</td>
filter:{
    formatTime(time){ // time 形参
       return moment(time).format('mm:ss')
  },
}
------获取时间戳
new Date().getTime()
```





## 后台数据转换

注释：有时候复用组件调用接口因为每个接口后端返回的数据结构可能不一样那么接口对应的参数就需要进行转换处理再使用

```js
search() {
  axios({
 url:"https://autumnfish.cn/search?keywords=" + this.inputValue + "&gfdd=" +
       Date.now(),
      }).then((res) => {
        console.log(res);
// 对接口数据进行转换处理
        this.list = res.data.result.songs.map((item) => {
       // 复用得图片渲染是item.picUrl 这个接口是item.cover,那就进行取存
        item.picUrl = item.cover;
// 这里接口代码是复用复制来的 复用组件有song包裹数据这个接口没有所以人为加一层包裹才不会报错
       item.song = {
             artists:item.artists,         
             album:item.album,
             duration:item.album}  
          return item;
      });
```

































































