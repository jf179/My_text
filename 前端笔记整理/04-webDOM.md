# DOM

- DOM ：即Document.Object Model 文档对象模型
- DOM -> 对象，宿主对象document, 也是由浏览器提供操作方法
- BOM  -> 浏览器对象，window,  包含DOM，
  - 区别 BOM由浏览器厂商执行标准，DOM由ECMA制定标准
- Host object:  宿主对象，执行JS脚本的环境提供的对象，浏览器对象,兼容性问题
- DOM只能操作HTML/XML， 
  - 发展：XML -> XHTML->HTML,
- 所有DOM获取到的多个元素节点都是类数组



## 本地对象Native object

- 本地对象由JavaScript提供，可以理解为JS的内置对象
- Object, Function , Array, String,  Number,  Boolean,  Error,  EvalError  , SyntaxError,  RangeError,  ReferenceError  ,  TypeError,  URLError,  Date,  RegExp
- Global 全局对象的统称，所有的内置对象都隶属其下



## HTML中的script

- 在html中使用 JS脚本的方式是过 script标签引入或直接包裹代码
- script元素有以下常用属性:
  - src：可选。表示包含要执行的代码的外部文件(js文件)
  - async : 可选。表示应该立即开始下载脚本，但不能阻止其他页面动作，比如下载资源或等待其
    他脚本加载。只对外部脚本文件有效。
  - defer: 可选。表示脚本可以延迟到文档完全被解析和显示之后再执行。只对外部脚本文件有效，现在浏览器已经忽略这一属性，因此一般来说JS文件应该放在页面body底部执行
  - type: 可选，已废弃,现代浏览器已经不需要再声明脚本类型了



## 常用api 

- document：DOM入口，包含整个html文档信息

- document.all ：返回页面的完整内容(所有DOM元素节点)

- document.links:  返回文档中所有的超链接合集

- document.URL:  返回来源文档的URL链接

- document.title:  返回文档标题

- document.domain  返回域名

- document.referrer  返回整个网址源

  

## 常用methods

- document.createElement('div');  创建一个新的元素
- document.toch  ：创建一个新的toch对象



## DOM原型

- 与JS原型一样DOM树也是基于原型关系
- Element.prototype; 所有doucment对象下的对象都继承自Element
- HTMLDocument.prototype;  查看原型方法也可自己添加
- Document.prototype;
- Element.prototype;

```js
HTMLDomcument.prototype.say = function(){
    console.log('添加原型方法say')
}
Document.prototype.eat = function(){
    console.log('Document添加方法eat')
}
// 此时Document可以继承HTMLDocument
document.say(); // 自身没有say方法，于是可以继承HTMLDocument的say方法
document.eat(); // 自身有eat方法，
```



## 文档模型

- document.compatMode
  - 'BackCompat'   文档为怪异模式
  - `"CSS1Compat"`：文档不是怪异模式，意味着文档处于标准模式或者准标准模式

```js
if(document.compatMode === 'BackCompat'){
    console.log('文档为怪异模式')
}
```



## DOM渲染树简介

- domTree --> DOM树(深度优先解析原则)
- 解析与加载是异步完成的
- domTree - > cssTree - randerTree渲染树(不包含隐藏节点)

```html
深度优先：从上至下遇见元素节点优先深度，待一个节点之下的所有子节点遍历完毕，再回头解析同级节点
                    html
            head                 body
       meta     title        div       ul
                            h1           li
                                     span i   em
```



## 回流/重绘

- JS对页面节点进行操作时，就会产生回流或者重绘
- 回流；重排 reflow --> 一定引起重绘
  - 节点尺寸、布局: display:none/block 发生时可能导致渲染树的部分或者全部 需要重新构建
  - 导致回流的因素
    - 页面渲染初始化时
    - DOM节点的增加、删除、位置变化，尺寸大小、边距、填充、宽高
    - DOM节点 display显示与否，浏览器窗口的尺寸 resize 变化
    - offset、scroll、client、width、height
- 重绘：repaint --> 不一定会引起回流
  - 渲染树重新构建或者部分节点重新构建时，浏览器会根据渲染树重新绘制受影响部分的节点
  - 颜色 字体导致的重绘不会引起回流
- 浏览器会开启队列批量处理回流&重绘 从而减少回流重绘次数
- 优化：样式:使用css类或者cssText 使其一次性回流重绘完毕，节点操作：使用文档碎片createDocumentFragment();

```js
<div></div>
let div = document.getElementsTagName('div')[0];
div.width = '200px';  // 回流 + 重绘
div.margin = '20px';  // 回流 + 重绘
div.backgroundColor = 'red'; // 重绘
div.border = '5px solid orange'; // 回流 重绘
div.fontSize = '20px'; // 回流 + 重绘

let span = document.createElement('span');
span.innerHTML = '我是一个牛逼的程序员';
documnet.body.appendChild(span);  // 回流 + 重绘 注意：尾部追加节点比头部追加节点损耗更小
```



## 时间线

- 浏览器加载页面开始-加载完毕，过程中发生的每一项执行的总流程-称之为 时间线
- document对象 -> JS开始参与 DOM-解析文档-构建DOM树-遇上link标签(开启线程)异步加载css外部文件-构建cssDOM
- 如果script脚本标签没有设置延迟加载 则会阻塞页面其他文件的进行，等待js文件加载完毕再继续，因此js脚本必须放在文档最后面
- 解析文档遇到img 先解析这个节点 src 创建加载线程 异步加载图片资源不阻塞文档加载
- 文档解析完成 js脚本开始按照顺序执行 可以进行页面交互了
- 文档解析完成时会触发 DOMContentLoaded 时间
- 待所有文档加载完毕包括 延迟执行的异步script脚本，才开始触发 window.onload

```js
// 若文档已加载完毕 write方法会清空文档内容 留下自己，若文档(DOM树还未渲染)时使用则会追加到文档尾部
document.write('<h1> hello world </h1>'); // 追加

// 待文档加载完毕后执行write,会先清理掉文档其他内容
window.onload = function(){
    document.write('<h1> hello world </h1>'); 
}
```



## 监听文档加载状态

- document.onreadystatechange 
  - loading  仍在加载
  - interactive   可交互  加载完毕 文档已被解析 但是图片样式表之类的资源仍在加载
  - complete   文档和所有子资源完成加载 即将触发load
- window.onload 等待文档包括异步脚本加载完毕触发，DOMContentLoaded文档加载解析完毕触发，不包括图片和样式表

```js
document.onreadystatechange = function(){
    // console.log(this.readyState);
    if(this.readyState === 'loading'){
        console.log('loading');
    }else if(this.readyState === 'interactive'){
        console.log('interactive');
    }else if(this.readyState === 'complete'){
        console.log('complete');
    }
}
```





## DOM选择器



注释：薪增的Selector/All选择器, 无法动态获取新增节点，

注：元素选择器返回的集合都时可迭代的，因为内部添加了Symbol.iterator属性

1. getElementById();  id选择器  不能在元素之上调用
2. getElementsByName();  name选择器，返回一个实时更新的 类数组的可迭代对象，不能在元素之上调用
3. getElementsByTagName(); 元素选择器，返回一个实时更新的 类数组的可迭代对象
4. getElementsByClassName(); 类名选择器，返回一个实时更新的 类数组的可迭代对象
5. querySelector();  通用选择器，选取单一元素，需要加前缀，缺点：无法实时更新DOM集
6. querySelectorAll(); 通用选择器，选取所有指定类型的元素集合类数组可迭代对象，缺点：静态集合-无法实时更新DOM集
   1. 可以在获取元素时添加类名，这样就实现了动态获取如 ('.row .seat.selected')

```js
document.querySelector('.box') // 获取元素 符合条件的第一个元素。用法与css相同
document.querySelector('.box>.xxx') // 获取类名box元素下面的类名xxx的子元素

document.querySelectorAll('li') // 获取符合条件的所有元素，无法获取动态添加的DOM节点
document.querySelector('.xxx').querySelectorAll('li') // 获取类名xxx元素内的所有li标签

document.getElementsByName('myicon'); // 返回指定name名的对象合集
document.getElementById('myform'); // 返回单个指定id名的元素
document.getElementsByTagName('div'); // 返回所有同类标签元素

 // 搜索DOM文档 返回所有指定类名的类数组对象, 如果使用在元素上则返回该元素节点下所有指定类名的子元素
let sum = document.getElementById('item');
let btns = sum.getElementsByClassName('span')
let sum = document.getElementById('item').getElementsByClassName('span'); // 以上两行的简写
```

```js
// 两个特殊元素的获取方式
document.documentElement // 获取整个html
document.body;   // 获取body
```

小记：HBuilder X 编辑器，元素颜色更改 Alt + 鼠标左键调出调色版



## 节点检查与标签名

- element.hasChildNodes();   检查一个元素是否有子节点
- nodeName与element.tagName读取元素节点名称的能力,注：tagName 仅接收元素节点

```js
<div> </div>

let div = document.getElementsByTagName('div')[0];
console.log(div.hasChildNodes()); // false   div内部没有子元素节点

console.log(div.tagName); // DIV
```



## DOM树的遍历

- 因为DOM选择器返回的是一个集合(只读)，一个类数组的可迭代对象，因此它虽然没有数组的方法，但内部有添加了Symbol.iterator,  依然是可遍历的

```js
<ul>
   <li>1</li>
   <li>2</li>
   <li><a href="#">aaa</a></li>
 </ul>

let ul = document.getElementsByTagName('ul')[0],
    li = ul.getElementsByTagName('li');

// 对li元素集合进行遍历
for(let node of li) {
    console.log(node)
    console.log(li.innerText)
}
```



## DOM元素的类

- DOM 节点是常规的 JavaScript 对象。它们使用基于原型的类进行继承
- 每个DOM元素都有对应的和同样的内建类，类似数组拥有的属性方法一样，为DOM提供能力
  - `Node` — 提供通用 DOM 节点属性
  - `Element` — 提供通用（generic）元素方法，
  - `EventTarget` — 为事件（包括事件本身）提供支持，
  - `HTMLInputElement` — 该类提供特定于输入的属性，
  - `HTMLElement` — 它提供了通用（common）的 HTML 元素方法（以及 getter 和 setter）



## 元素文本

- ele.innerHTML;   解析文本包括标签, 在ele内部直接插入

- ele.outerHTML; 解析文本包括标签，先删除ele,再添加元素到ele的位置


```js
<div class='cs'> hello </div>
let btn = document.querySelector('.cs);
btn.innerHTML = '<h1>xjj</h1>'; // 在div元素标签内插入h1元素
btn.outerHTML = '<h1>xjj</h1>'; // 先删除div元素标签，再添加h1元素
```





## 隐藏元素

```js
<div hidden>hello </div> // div元素将被隐藏-占位-但不可见
```



## 类数组添加原型方法

- 为类数组添加数组方法

```js
// 对象也是类数组
let obj = {
    0:1,
    1:2,
    2:3,
    length:3,
    push: Array.prototype.push,
    splice:Array.prototype.splice,
    [Symbol.iterator]:Array.prototype[Symbol.iterator] // 使对象可迭代
}
obj.push(4); // 不会报错，因为已经为其添加数组的原型方法push
let sum = obj.splice(0,2); // 1,2
```



## 对象属性添加

```js
let obj = {}
obj['name'] = 'xjj'; // 给对象obj添加属性name 值=xjj
```



# BOM事件

### 事件三要素

注释：事件三要素

1. 事件源：即触发事件的对象(DOM)，也就是由谁触发，即e.target,触发它的DOM对象是谁

2. 事件类型：如 **click** 点击事件、key键盘事件

3. 事件的处理程序handle：即对事件进行何种操作->由函数完成也称回调函数

   1. 浏览器会读取handle函数，创建事件信息作为参数->并将函数赋值给事件，之后载入到DOM元素

   ```js
    let btn = document.querySelector('.box')
    let Div = document.querySelector('div')
    // btn时事件对象，给它绑定一个事件监听函数，
    // 事件类型为click,e为事件类型，e.target则表示事件对象(也就是btn代表的div元素)
    btn.addEventListener('click',(e) =>{
   	  Div.innerText = 'Good';
        Math.ceil(Math.random() *9) // 随机数
    })
   ```



### 事件监听

1. el.addEventListener(); 注册事件监听
   1. 兼容：firefox、chrome、IE、safari、opera；不兼容IE7、IE8
   2. 可以为DOM元素注册多个事件监听函数
   3. el.removeEventListener(); 移除事件
2. el.attachEvent('onclick',fn); 一般不用IE已死
   1. 兼容：IE7、IE8；不兼容firefox、chrome、IE9、IE10、IE11、safari、opera
   2. 该函数的调用对象不再是事件对象，而是window,需要用handle.call(el);更改指向到事件对象本身
3. ES5监听方法
   1. el.onclick = function(){}

```js
// addEventListener 接收三个参数
1: 事件类型
2: 事件处理函数
3: 事件捕获/冒泡
el.addEventListener('click', handler, false)
```





### 事件回调

即事件监听注册后的回调函数，

- 可以直接将逻辑写在元素内，也可以将回调函数放在元素内或监听函数内，

```js
1：<input teye='text' onfocus='console.log('hello')'
2:<input type='text' onfocus='getDaate()';
function getDate(){
    console.log('hello')
}

3: let btn = document.querySelector('xxx');
btn.annEventListener('click',getDate,false);
```





### 事件源

- 指触发事件时被引用的DOM元素，如：div.addEventListener('click'); 这里div就是事件源
- 在事件源被绑定后，e.target与this都指向触发事件的对象，即DOM元素

```js
<div class="myicon"></div>
let div = document.querySelector('.myicon');
div.addEventListener('mouseove',function (e) {
    console.log(e) // MouseEvent 事件类型
    // 触发事件的对象btn (某个DOM元素),在事件冒泡/捕获阶段被调用
    console.log(e.target); 
    // e.target可以用this替代，这里this也指向事件源
    e.target.style.background = 'pink'; 
})
```



### 事件流

- 指接收事件的顺序，冒泡或捕获



### 元素对象

- tagName 返回一个元素的标签名，即事件对象名

```js
function handleClick(e){
    let tar = e.target; // 先获取事件对象如某个元素DIV, BUTTON
     // 获取对象元素的标签名，再转小写如 BUTTON -> button
    let tagName = tar.tagName.toLowerCase();
    if(tagName === 'button'){
        console.log('恭喜你是button按钮')
    }
}
```





### 鼠标事件

1. MouseEvent ： 
   1. click  鼠标左键点击时触发
   2. contextmenu 鼠标右键点击时触发
   3. mousemove 鼠标移动，DOM结构较为复杂是使用解决鼠标滑入滑出产生的问题
   4. mousedown鼠标按下，
   5. mouseup 鼠标弹起
   6. mouseover 鼠标移入， 对元素内部的子元素同样生效(不是冒泡)-适用于事件代理
   7. mouseout 鼠标移出,     对元素内部的子元素同样生效(不是冒泡)-适用于事件代理
   8. mouseenter 鼠标进入，不会对元素内部的子元素生效(推荐)
   9. mouseleave 鼠标离开，不会对元素内部的子元素生效(推荐)
2. FocusEvent   :  焦点事件 
   1. focus获得焦点, 
   2. blur失去焦点 ，
3. e.button； 可以判定鼠标姿势，左键点击0 ,中心滚轮点击1，右键点击2

### 键盘事件

1. keyup  : 键盘按键弹起触发   ==(常用)==
2. keydown :键盘按键按下时触发
3. keydown与keyup同时绑定时keydown优先执行

```js
// keyCode 监听键盘按键 不区分按键大小写 a与A都是 编码65
document.addEventListener('keyup', function(e) {
console.log(e.keyCode) // keyCode会根据事件对象获得用户按键的编码号
})
// 常用的按键编码
ent 回车键 编码：13
空格键 编码：32
Esc  退出键 编码：27
Ctrl + c 复制 编码：17 + 67
back 回退/删除键  编码：8
```



### 表单事件

1. submit   提交时触发
2. focus   聚焦时触发，没有冒泡功能
3. focusout  失去焦点时触发 等同blur
4. focusin   是focus的冒泡版

```js
<input type='button' value='Click one me' onclick='console.log('我被Click啦')';
<input type='text' onfocus='console.log('嘿! 我被聚焦啦')'；
```





### css事件

- transitionend   当一个动画完成时触发



### scroll事件

1. scroll允许对页面或元素滚动做出反应
2. 需要时查看文档

# DOM样式/类

- DOM无法修改css，但是可以操作style行内样式
- el.style;  可以查看所有能操作的css样式
- JS可以修改style,也可以修改样式类，所有JS操作DOM的样式属性都是驼峰形式如背景: backgroundColor
- 坐标位置都属于样式，

```js
// 一些例子
el.style.borderRadius = '20px'; // 边框圆角
el.style.fontSize = '16px'; // 字体大小
el.style.backgroundColor = 'red'; // 背景颜色
el.innerText = '我修改了el元素的文本'; // 修改文本
```





### 更换图片路径

- cs中背景图片是具有缓存效果的

```js
zxy.addEventListener('click', function() {
	Img.src = './img/zxy.jpg';
    Img.title = '张学友' // title属性
 })
```



## 表单属性&状态

注释：相应属性会触发相应状态

- type类型,  value值,   checked复选框, blur失去焦点  selected下拉选项,  disabled禁用,
- oninput事件； input事件，input值发生改变时实时生效
- onchange事件:   表单属性；当表单值发生改变后触发（指输入完毕后回车/鼠标离开表单元素后触发)；如果第二次输入的值未发生改变则不会触发该事件

```js
// oninput
let input = document.getElementsById('inputBtn');
input.oninput = function(){
    // 实时获取input值，在键入时就会持续触发input事件
    let val = this.value;
    console.log(val)
}
input.onchange = function(){
    // 鼠标离开表单元素/敲打回车键后才会触发change事件
    let val = this.value;
    console.log(val)
}
```



### input占位符

- placeholder关键字占位；缺点:无法边界的修改占位符的样式
- 采用JS事件控制,可以直接写在标签上

```js
<input type='text' oninput='if(this.value !== ''){this.className = '某个样式类'}' 
 onblur='if(this.value === ''){this.className = ''}'
/>
```





### style属性

- 行内：style
- 类  ： className

```js
style.backgroundColor  // 控制背景样式
style.color   // 控制宽高
style.display = 'none' // 控制显示/隐藏
style.backgroundPosition // 控制元素坐标

----操作样式类----

```



### className类

注释：语法：className = '类名'

- 属于操作css类.以字符形式替换/添加类，用于管理整个类(可以是多个)
- 优先选择类样式，尽量不要使用el.style.width = '200px';

```js
.box1{
    width:100px;
    height:200px;
    background-color:pink;
}
.box2{
    font-size:16px;
    border-radius:50px;
}
div.onclick = function(){
	this.className = 'box1 box2' // 同时保留2个类样式
}
```



### classList类

1. classList； 操作类.该属性也是可迭代的，遍历DOM时能利用其列出所有所有类，常用于管理单个类
   1. classList.add();  添加类
   2. classList.remove(); 移除类
   3. classList.toggle();  切换类，如果类存在就移除。不存在则添加
   4. classList.contains(); 检查元素是否由某个类，返回true/false

```js
let btn = document.querySelector('xxx')
btn.classList.add('w'); // 添加类 w
btn.style.display = 'none'; // 隐藏元素样式 block则显示样式
btn.style.margin = '20px 0px'; // 设置样式
```



### 样式之计算属性

1. getComputedStyle(); 返回属性的解析值, 该函数方法挂载在window下

```js
let btn = document.querySelector('div')
getComputedStyle(btn).width; // 100px ,获取btn元素的宽度值
getComputedStyle(btn).marginTop; // 20px 获取元素的上外边距值
window.getComputedStyle(btn); // 返回对象集合，包含btn元素的所有样式属性
```





### 获取元素样式属性

注释：获取元素的属性

```js
// 只能以window开头
window.getComputedStyle(box)['width'] // 获取元素宽度
window.getComputedStyle(box)[‘color]  // 获取颜色

// 优化；封装成函数调用 简化代码
function getStyle(el, attr){
    return window.getComputedStyle(el)[attr]
}

getStyle(div,'width') // 调用函数获得元素宽度
console.log(getStyle(div,'color')) // 调用函数获取元素字体颜色
```





### 排它思想

注释：利用双重循环，即外层一次(当前), 里层全部(其它)

```js
// 外层循环一次
for(let i = 0; i < element.length; i++){
    element[i].addEventListener('clcik', function(){
        // 里层循环全部
        for(let i = 0; i < element.length; i++){
           element[i].style.backgroundColor = '';
        }
        // 当前点击项样式生效
      this.style.backgroundColor = 'pink';
    })
}
```



### 控制图片路径

```js
element.style.backgroundImage = 'url(' + xxx.src + ')' // 路径采用拼接字符形式 可以是src/网络地址
```



### 商品全选控制

```js
let thead = document.querySelector('#j_cbAll');
let itemInp = document.querySelector('#j_tb').querySelectorAll('input')

thead.addEventListener('click', function() {
    // 将总选按钮的checked状态true赋值给变量vTo
	let vTo = this.checked;
	for (let i = 0; i < itemInp.length; i++) {
        // 如果总选按钮状态为true 单选按钮也全部为true
			itemInp[i].checked = vTo
	}
})

for (let i = 0; i < itemInp.length; i++) {
	itemInp[i].addEventListener('click', function() {
		let flgs = true; // 启用旗帜设定为true
		for (let i = 0; i < itemInp.length; i++) {
            // 如果有一个未被选中 则旗帜为false
			if (!itemInp[i].checked) {
				flgs = false
			}
		}
        // 如果旗帜不为false 则意味所有单选项被选中 旗帜状态默认true 将其赋值给总选按钮即可
		thead.checked = flgs
	})
}
```



### 元素检测/自定义属性

注释：两种获取和设置属性的方式

1. 获取：element.属性名，设置： element.属性名 = '值'
   1. eleemnt.id;   获取指定元素的id属性
   2. 自动属性包括比如 type name class id 等
2. 获取：element.getAttribute('属性')， 设置：element.setAttribute('属性名', 值)
3. 第一种用来设置/获取元素自带属性，第二种用来设置/获取自定义属性，自定义属性应该以 data开头
4. hasAttribute('xxx');   检测元素属性是否存在
5. 还可以通过事件源获取元素属性，如 e.target.attributes.class.value  获取元素的class名或name名

```js
// 获取属性
<div class='box' id="demo">hello</div>
let div = document.querySelector('div')
1:  div.id   // demo 获取元素节点的自带属性
2： div.getAttribute('class');  // 获取元素的属性名 box
3： div.hasAttribute('data-age'); // true

// 设置属性
<div id="demo" index="1" class="box">hello</div>
1： div.id = 'test' // 原生设置/更改元素属性名
2:  div.className = 'navs' // 原生设置/更改元素属性名
3： div.setAttribute('index',2) // 设置/更改自定义属性
4： div.setAttribute('class','foo') // 设置更改元素class类属性
```



```js
// 自定义属性设置
<div></div>  // index="1"
<div></div>  // index="2"
<div></div>  // index="3"
<script>
	let div = document.querySelectorAll('div');
	for (let i = 0; i < div.length; i++) {
        // 为 所有div 设置属性名
		div[i].setAttribute('index', i + 1)
}
</script>
```



### 新增自定义属性获取

注释：获取所有自定义属性，只能获取以data开头的自定义属性

```js
<div data-age="18"></div>

element.dataset  // 返回data开头的自定义属性的对象集合
element.dataset.age // 18
element.dataset['属性名'] // 以索引访问也可以 同上
element.hasAttribute('data-age'); // true
```



### 样式类检测

- 用于检测指定样式类是否在于classList样式列表中，返回true或false

```js
<div class='container'>
 <div class="seat"></div>
 <div class="seat"></div>
 <div class="seat"></div>
 <div class="seat occupied"></div>
 <div class="seat occupied"></div>    
</div>
container.addEventListener('click',function(e){
    // 使用事件代理，只代理类名seat类名的子元素，但不包含occupied的子元素
    if(e.target.classList.contains('seat') && !e.target.classList.contains('occupied')){
        console.log('通过')
    }else{console.log('未通过')}
})
```



# 封装元素获取函数

- 获取元素宽高，不包括边框，与offfsetWidth、offsetHeight类似

```js
function getStyles(elem,prop){
    if(window.getComputedStyle){
        if(prop){
            return window.getComputedStyle(elem,null)[prop];
        }else{
            return window.getComputedStyle(elem,null);
        }
    }else{
        if(prop){
            return elem.currentStyle[prop];
        }else{
            return elem.currentStyle;
        }
    }
}
```



### DOM属性

- DOM属性不总是字符类，比如 checked 属性就是布尔类型的





### 设置自定义属性

- 除了内置的class/id/name属性外。我们还可以自定义属性
- element.setAttribute(name, value); 第一个参数：自定义属性名，第二个参数：属性值
- element.getAttribute(name);  读取自定义属性或内置属性如class

```js
 <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ul>

let ul = document.querySelector('ul').querySelectorAll('li');
for(let i = 0; i < ul.length; i++) {
    // 为所有的li 元素添加自定义属性 index='索引值'
    ul[i].setAttribute('index', i + 1)
}
```





### 移除属性

注释：

```js
div.removeAttribute('属性名')
```



### Tab栏切换

注释：

```js
let list = document.querySelector('.tab_list').querySelectorAll('li');
let items = document.querySelector('.tab_con').querySelectorAll('.item');
console.log(items.length) // 获取到的是一个元素集合有length长度

for (let i = 0; i < list.length; i++) {
	list[i].setAttribute('index', i) // 给Tab栏设置自定义属性
	list[i].addEventListener('click', function() {
		for (let i = 0; i < list.length; i++) {
			list[i].className = ''; // 清掉Tab栏所有样式
		}
		this.className = 'current'; 当前选中项添加样式类
        
        // 获取自定义属性值 赋值给变量
		let index = this.getAttribute('index');
        // 循环所有内容区元素
		for (let i = 0; i < items.length; i++) {
			items[i].style.display = ''
		}
        // 将获取到的自定义属性值当作内容区的索引 达到联动效果
		items[index].style.display = 'block';
	})
}
```



总结：自定义属性是为了通过获取和设置属性，来操作数据，注：自定义设置的属性一般以data开头。可以在标签内显示的书写，==也可以通过JS设置setAttribute('属性名', 属性值)==



## DOM操作

注释：DOM的顶级对象 document，大多数为只读属性

### 父子节点

注释：父子节点的获取-->就近原则；如果没有返回null

1. 父节点/元素：parentNode(包括空白节点：不推荐) , parentElement 父元素(推荐)
2. 子节点：childNodes 所有子节点 -包括孙子节点和空白节点(不推荐)
3. 子元素：children 所有子元素-不包括孙子元素 (推荐)
   1. el.children.length;  子元素长度
4. el.parentElement.className;  获取父元素的属性名



  ```js
 <div class="box">
	<span class="wei">X</span>
 </div>
 let wei = document.querySelector('.wei');
let box = document.querySelector('.box');

console.log(wei.parentNode) // 获取父节点
console.log(wei.parentElement); // 也是获取父元素节点
console.log(box.children) // 获取子节点集合

// 首尾节点
console.log(box.children[0]) // 获取子节点集合的第一个元素
console.log(box.children[box.children.length -1]) // 获取子节点集合的最后一个元素 ：推荐
  ```



注释：childNodes获取的是所有节点的集合包括换行，可以通过[索引]来访问有效节点；不建议使用这种方式



### 首尾节点 (不推荐)

1. 第一个子节点：parentNode.firstChild / 推荐使用 parentNode.firstElementChild 
2. 最后一个子节点：parentNode.lastChild / 推荐使用 parentNode.parent.lastElementChild

   区别：firstChild与lastChild获取所有子节点返回的是一个集合，firstElementChild获取的是第一个有效元素



### 菜单下拉样式

```js
let nav = document.querySelector('.nav'); 
	let list = nav.children; // nav的子节点
	for (let i = 0; i < list.length; i++) {
        // 鼠标经过
	list[i].addEventListener('mousemove', function() {
        // list的第1个节点元素显示
		this.children[1].style.display = 'block';
	 })
        // 鼠标离开
	list[i].addEventListener('mouseout', function() {
		this.children[1].style.display = 'none';
	})
  }
```



### 兄弟节点(同级)

注释：

1. 下一个兄弟节点：nextSibling          获得包括文本 换行 (不推荐)
2. 上一个兄弟节点：previousSibling  获得包括文本 换行 (不推荐)
3. 下一个元素节点：nextElementSibling     (常用)
4. 上一个元素节点：previousElementSibling  (常用)



### 创建节点

语法：

- document.createElement('div');  创建元素节点  (常用)
- 创建完毕的元素节点，存储在内存中，需要用 append 打入DOM树，

```js
let li = document.createElement('li'); // 常用

// 该方法不要采用拼接字符方式 字符创建比较损耗性能
div.innerHTML = '<a href="www.baidu.com">百度</a>'  // 了解
document.write('<div>hello</div>')  // 了解 
```



### 追加节点

语法：appendChild(元素) 追加；  insertBefore(要插入的元素, 要插入到那个元素前面)，前插

1. el.before();  追加到指定元素前 (兄弟节点)
2. el.after();  插入到只读元素后 (兄弟节点)
3. el.append();   在el尾部插入元素。(父子节点)
4. el.appendChild();  在el尾部追加元素，(父子节点) (不推荐，已被append替代)
5. el.prepend() ;  在el 开头插入节点 (父子节点)
6. el.replaceWith();   将el 替换为给定的元素节点或字符
7. el.insertAdjacentHTML 接口可更方便实现元素插入; 缺点：无法动态更新DOM，详情参考官网
8. el.inesrtBefore()  (父子元素)  等同于 el.prepend()

```js
<div>
    <h1>我是h1</h1>
</div>    
let div = document.getElementsTagName('div')[0];
let h1 = document.getElementsByTagName('h1')[0];
let span = document.createElement('span');

span.innerText = 'hello';
div.insertBefore(span,h1); // 将span元素节点插入到h1元素节点之前
```





### 删除节点

语法：

- el.removeChild() (不推荐，已被remove替代)
- el.remove();

```js
ul.removeChild(ul.children[0]) // 删除ul的第0个子元素
div.remove(); // 移除自身

let btn = document.querySelector('div');
setTimeout(function(){
    btn.remove();
},2000) // 2秒后移除自身

-----------------------
   <div> </div>
let div = dcoument.getElementsByTagName('div')[0];
let p = document.createElement('p')
p.innerText = 'xjj hello';
// 上树
div.append(p)
// 移除div的第一个子元素 p
div.children[0].remove(); 
```



### 克隆节点

- element.cloneNode()

```js
//  复制ul的第0个子节点  true为深克隆 即复制文本，无参数则只复制空节点，默认fasle
let span = ul.children[0].cloneNode(true) ；// 深度克隆
ul.appendChild(span); // 克隆后作为子节点插入ul元素内部
```



### 替换节点

1. replaceChild()   用指定节点替换当前节点的一个子节点(不推荐，已被replaceWith替代)
2. replaceChildren();  将一个节点替换为一个节点集合，即替换成多个节点
3. el.replaceWith(新节点);  用新节点直接替换当前节点el

```js
let box = document.querySelector('#box');// 父元素
let target = document.querySelector('#target') // 要替换掉的子元素
let btn = document.querySelector('#btn'); // 按钮

btn.addEventListener('click' ,function(){
	let newBox = document.createElement('span'); // 创建元素
	newBox.innerText = '这是一个寂寞的天' 
	box.replaceChild(newBox,target) // 用newBox替换旧元素target
})
```



### 阻止连接跳转

```js
<a href="javascript:;">删除</a> // javascript:;   夹个冒号和分号即可
```

总结：parentNode : 父节点   children: 子节点，   

-  nextElementSibling 下一个兄弟节点，previousSiblilng上一个兄弟节点，



### DOM属性

- 我们可以为DOM添加自定义属性/方法，就像DOM元素内建的style，title一样


```js
<div class='cs'>hello </div>
let btn = document.querySelector('.cs');
btn.myDate = function(){
    console.log('xjj')
}
btn.myDate(); // xjj
```



### 获取元素名

- nodeName;  获取元素节点名

```js
<div class="box" id="box" style="background-color: pink">
  我是文本节点
  <!-- 我是注释节点 -->
  <h1>我是h1标题节点</h1>
  <a href="#">我是超链接节点</a>
  <p>我是段落节点</p>
</div>
let div = dcoumnet.getElementsByTagName('div')[0];
console.log(div.nodeName.toLowerCase()); // 获取元素节点名并转小写
```



### 节点类型

- nodeType;  1表示一个元素节点，具体参考官网

```js
let div = dcoument.getElementsByTagName('div')[0];
if(div.nodeType === 1){
    console.log('没错，这是一个元素节点')
}
```



### 文档碎片DocumentFragment

- 作为容器，用于优化散列的创建/插入元素，不会引起页面回流

```js
<div data-age="18"></div>
let div = document.getElementsByTagName('div')[0];

// 创建一个文档碎片 用于存储循环创建的元素
let num = document.createDocumentFragment();

console.time('s')
for(let i = 0;i < 10;i++) {
  let oLi = document.createElement('li')
  oLi.innerText = 'hello'
  oLi.className = 'list-item';
  // 存入的是内存中，循环不会引起页面重新渲染(回流)
  num.append(oLi)
}

// 一次性将创建在内存中的元素节点，打入DOM树节点 div
div.append(num)
console.timeEnd('s')
```





## 事件进阶

注释：传统函数表达式注册事件，相同的事件注册2次后面的程序会覆盖前面的程序，因此有了addEventListener

1. addEventListener('事件', 触发事件后的回调函数,useCapture)

```js
element.addEventListener('click',function(){},true) // true表示事件捕获阶段
```



### 移除事件

传统方式&

```js
事件元素.onclick = null; // 解绑点击事件

// 第二种方式
div.addEventListener('click', fn);
// 回调函数必须是具名函数才能移除
function fn(){
    console.log('触发一次');
    // 2个参数：要移除的事件名 以及事件函数
    div.removeEventListener('click', fn)
}

// 方式三 非严格模式下 使用arguments.callee移除匿名函数
this.removeEventListeneer('click',arguments.callee)
```



### DOM事件流

注释：事件流指接收事件的顺序，分三个阶段

1. 捕获阶段：默认false关闭，true为开启，
   1. 开启后子元素事件触发时会先执行父元素事件
   2. 顺序：父元素 --> (捕获)子元素
2. 冒泡阶段：默认false开启，true为关闭
   1. 只要子元素/父元素绑定了事件，那么点击子元素会依次向上传递至最外层的父元素
   2. 顺序：先触发子元素 - > 再依次向上触发父元素
   3. 表单事件focus没有冒泡机制，
3. **冒泡与捕获只能同时处于一种状态之中，即false默认开启冒泡，true默认开启捕获**

```js
// 事件只能处于捕获/冒泡，二者之中的一种
<div class="father">
	<div class="son">son</div>
</div>
<script>
    let father = document.querySelector('.father');
	let son = document.querySelector('.son');
   
    father.addEventListener('click', function() {
	     alert('father')
    }, true)；// 开启捕获

// 捕获阶段： 当父子元素都绑定了事件时，点击son 会先作用father-> son
// 冒泡阶段：将true去掉或该为false, 只点击son，father会自动触发事件处理程序，证明有冒泡效果

son.addEventListener('click', function() {
	alert('son')
}, true)
</script>
```



注：

- 有些事件没有冒泡效果-如：blur失去焦点, focus获得焦点, mouseenter 鼠标弹起，mouseleave 鼠标离开
- submit, rest ,select ,change

### 事件对象

注释：绑定的事件类型type即为事件的对象；

   ```js
// click为事件，回调函数拥有绑定事件的对象 它可以作为参数传递
div.addEventListener('click', function(enevt){
      console.log(event) // click属于鼠标事件，因此获得的是整个鼠标事件的对象集合
})
   ```



### 事件对象的属性

1. event.target:  事件对象；  返回的是触发事件的对象
2. this :   事件侦听中的this     返回的是绑定事件的元素对象

### 阻止事件的默认行为

1. e.preventDefault()； 用于阻止元素自带的默认行为如跳转/表单提交, IE9以下不支持

```
div.addEventListener('click', function(e){
	e.preventDefault(); // 阻止事件的默认行为 如a标签的跳转行为
})
```



### 阻止事件冒泡

注释：e.stopPropagation()，绑定到子元素身上，阻止事件冒泡

- 单独取消父元素事件冒泡无法打断事件代理，除非给每个子元素都写上e.stopPropagatiion()
- input focus button等元素没有冒泡的，但是也有对应的冒泡版本的写法 详情参考MDN文档

```js
div.addEventListener('click', function(e){
	e.stopPropagation(); // 阻止事件的冒泡效果
})
```



### 事件委托

注释：将事件监听设置给父节点，利用冒泡原理影响每个子节点

- 原理：只要父元素注册有事件，那么点击子节点就会触发父元素事件运行(默认false开启冒泡)
- 注意：事件委托下不要使用this，因为this指向当前事件对象，应该利用e.target
- 文档碎片 createDoucmentFragment();  一个文档容器 可以盛装元素

```js
<ul>
    <li>我是小丽</li>
    <li>我是小丽</li>
    <li>我是小丽</li>
 </ul>
let ul = document.querySelector('ul');
 ul.addEventListener('click', function(e) {
      // 利用冒泡原理 只需绑定父节点 之后点击子节点就会触发父节点事件
     e.target.style.backgroundColor = 'red'; 
 })
```



### 鼠标坐标page

注释：事件对象

1. e.pageX/Y :   返回鼠标相对于文档页面坐标 ，包含滚动，  (常用)
2. e.clientX/Y  : 返回鼠标相对于浏览器内窗可视区的坐标 -不包含滚动条卷入像素
3. e.offsetX/Y   返回鼠标位于元素的X/Y坐标，包含元素边框和滚动条卷入像素
4. screenX/Y :  返回鼠标相对于电脑屏幕的坐标 -不包含滚动条，多个屏幕的话会形参组合像素

 ```js
// 跟随鼠标  图片给绝对定位 absolute
let img = document.querySelector('img');
document.addEventListener('mousemove', function(e) {
	let x = e.pageX;
	let y = e.pageY;
	img.style.left = x - 40 + 'px'; // -40让鼠标位于图片中心
	img.style.top = y - 40 + 'px';
})
 ```



### 自定义事件

详细参考官网；需要时再学

- new CustomEvent();  自定义一个事件
- ele.dispatchEvent();  派发事件



# BOM对象

注释：BOM的核心是 window对象，它有两重身份，一种是作为JS全局对象global，一种是作为浏览器窗口的JS接口

### 组成

- window  ,直接定义的属性方法
- navigator  浏览器信息
- history  浏览器窗口访问记录
- Location  当前页面地址 、重定向等
- screen  浏览器屏幕相关信息
- frames  框架相关信息的信息和操作

### BOM常用方法

- window.print();   打印当前页面，
- window.closed() ;   判断当前页面是否已关闭 返回布尔值
- window.open(URL); 导航到指定url，如果点击a连接的 href 属性





### 页面加载事件

1. DOMContentLoaded    ； 等待页面DOM元素加载完毕再执行js代码，不包括图片css

   ```js
   document.addEventListener('DOMContentLoaded',function(){})
   ```



### 浏览器窗口事件

注释：resize ：window窗口变化事件 可以检测窗口大小的变化

1. window.innerWidth : 浏览器内部窗口高度-> 只读属性
2. window.innerHeight;  浏览器内部窗口高度-> 只读属性
3. 可以利用监听浏览器窗口变化进行相应样式处理

```js
window.addEventListener('resize', function(){
	 console.log('变化了')
     console.log(window.innerWidth) // 获取宽度
     console.log(window.innerHeight) // 获取高度
} )
```



### 窗口大小

- window.outerWidth 返回浏览器窗口的整体高度-只读属性
- window.outerHeight 返回浏览器窗口的整体高度-只读属性



### 移动端screen

- 浏览器屏幕相关信息  主要用于移动端
- screen.orientation; // 获取屏幕方向

```js
screen.orientation;
if(screen.orientation.type === landscape-primary){
    console.log('用户处于横屏状态')
}else if(screen.orientation.type === 'portrait-primary'){
    console.log('用户处于竖屏状态')
}
```





### 定时器

注释：

1. setTimetout : 定时器，接收2个参数：1 ：回调函数 2：延迟毫秒数
2. setInterval:  计时器，接收2个参数，1：回调函数， 2：间隔毫秒数
3. clearTimeout(要清除的定时器)     clearInterval(清除计时器)

   ```js
   setTimeout(function(){
   console.log('时间到 爆炸')
   },2000)
   
   let num = 0;
   setInterval(function() {
   	console.log('打开了' + num++ + '次')
   }, 2000)
   
   ----分割线---
    // 清除定时器
   let times = setTimeout(function(){
          console.log('111')
    },3000)
   clearTimeout(times) // 清除定时器
   clearInterval()   // 清除计时器
   ```



### location对象(重要)

注释：用于获取/设置当前窗口的文档信息如URL, 它即属于 window 也属于 document两者指向同一对象

1. location.href:   获取/设置整个url     ==(常用)==
2. location.host:  返回主机域名
3. location.port: 返回端口号
4. location.patchname: 返回路径(文件地址)
5. location.search:   返回参数，即？号后面的内容 ?name=xjj&age=18    ==(常用)==
6. location.hash : 返回# 后面的内容 常见于锚点连接

   ```js
   // 提交到index1.html页面
   <form action="index1.html">
   	用户名: <input type="text" name="uname">
   	<input type="submit" value="登录">
   </form>
   
   // index1.html页面接收---
   <div></div>
   <script>
        // 获取提交过来的参数值 进行截取并分割成字符串数组
   	let ais = location.search.slice(1).split('=');// ['uname', 'ddd']
   	let div = document.querySelector('div');
   	div.innerText = ais[1] // 将内容插入到div
   </script>
   
   // locatiion.hash演示
   <a id="myAnchor" href="/en-US/docs/Location.href#Examples">Examples</a>
   let anch = document.getElementById('myAnchor')
   let str = anch.hash.slice(1); // #Examples
   console.log(str); // Examples
   ```
### location对象方法

注释：

1. location.assign() :  重定向页面   记录历史
2. location.replace() : 替换当前页面，不记录历史  无法后退
3. location.reload() ： 重写加载页面 类似刷新


```js
//  页面重定向
<button>点我跳转</button>
<a href="www.baidu.com">地址</a>
<script>
 let btn = document.querySelector('button');
 let ais = document.querySelector('a');
 btn.addEventListener('click', function(){
	location.assign(ais) // 重定向页面
 } )
</script>

// 替换当前页面 不记录历史 无法后退
<button>点我跳转</button>
let btn = document.querySelector('button');
btn.addEventListener('click', function() {
	location.replace('www.gogo.com')  // 替换
})
```



### navigator对象

注释：获取浏览器对象信息集合，检测插件等

- 详细参照文档

```js
if(navigator.userAgent.match(/(phone|pad|ios|ipad/i)){ // 正则匹配识别用户终端
     window.location.href=""  // 如果是以上设备 使用移动端特性
}else{
   // 如果是PC端 使用PC端特性
}

navigator.appName // 获取浏览器名称
navigator.appVersion // 浏览器版本信息 
```



### history对象

注释：提供了对浏览器的会话历史的访问 。保存用户在浏览器访问的历史记录，从而实现前进后退

1. back()  :  后退
2. forward()  :  前进
3. go(参数) ： 1 前进一个页面， -1 后退一个页面

```js
history.back() // 后退
history.forward() // 前进
history.go(-1)  // 后退一页
// 用的不多 浏览器以及框架都有自己的路由模式  无需操心
```



history历史状态管理

- 详情参考文档



### localStorage本地存储

注释; 本地存储。键值对形式,  永久性，浏览器共享， 需手动移除

- 语法：localStorage.setItem('key', val);  键值对

```js
localStorage.setItem('username', Mak) // 存储token 用户名与名字 ,
localStorage.getItem('username')  // 获取token
localStorage.removeItem('username') // 移除token

----存储复杂数据类型--
let Good = {
    name: '王老板',
    age: 18,
    gender: '男'
}
// 存储复杂负数类型需要先转换成JSON格式
let jsonObj = JSON.stringify(Good);
localStorage.setItem('user', jsonObj)
// 获取复杂类型数据 需再将JSON格式转成JS
let getJson = localStorage.getItem('user');
let data = JSON.parse(getJson); // 转换
```



### sessionStorage会话存储

注释：存储&读取&移除与 localStorage相同， 属于临时会话存储，页面之间不共享，关闭页面自动销毁

1. 复杂数据类型的存储读取与localStorage相同





## 网页特效

### window窗口

- window.pageYOffset  窗口Y轴滚进去的像素数，等同scrollTop
- window.pageXOffset  窗口X轴滚进去的像素数, 等同scrollLeft

```js
// 可读属性

window.scrollBy(0,1); //  在窗口中按指定的偏移量滚动文档，0代表X轴,1代表Y轴

// 判断有无滚动条 原理：滚动像素的高度数是否大于页面可视区的高度 
element.scrollHeight > element.clientHeight ? '有滚动条' : '无滚动条'
// 判断滚动条是否到达底部 原理：
// scrollHeight滚动条件下整个文档页面的高度(包括不可见区域) === (scrollTop滚动后与顶部的距离 + clientHeight可视区高度); 即为到达底部
if((element.scrollHeight - (element.scrollTop + element.clientHeight))){
    console.log('到达底部了')
}
```



### offset 系列 (CSS)

注释; 偏移量，动态获取元素位置、宽高，返回值不带px单位，且是一个整数

- **只读属性**,该DOM接口为CSS提供，
- el.offsetParent   : 返回元素带有定位的父级元素(整个元素标签)，如果父级没有定位则返回body
- el.offsetTop    :    返回元素上方到带有定位的父元素的距离,如果父元素没有定位则参照body
- el.offsetLeft    :   返回元素与带有定位的父元素左边框的偏移量,,如果父元素没有定位则参照body
- el.offsetWidth :   返回自身padding 值、(与父元素无关)  包括边框 内容区的宽度, 
- el.offsetHeight :  返回自身padding值， (与父元素无关)  包括边框 内容区的高度
- el.offsetX/Y      返回鼠标在具体元素内部的坐标，包含边框和滚动条卷入像素
- el.pageX/Y:  返回鼠标在具体元素内部的坐标。
- e.X/e.Y轴坐标，一个双精度的浮点值，可视区
- offset系列获取元素宽高值有一个缺陷 就是所有的值都会包含进去包括padding，开启弹性盒子可解决

```js

```





### **client**系列

只读属性：用于测量元素边框 border, 元素也可以是整个可视窗口，与 window.inner系列类似

1. el.clientLeft;   返回元素自身左边框宽度 , 不带px
2. el.clientTop; 返回元素自身上边框宽度，不带px
3. el.clientHeight; 返回元素自身高度-不包括边框和滚动条 /浏览器窗口中页面可视区高度-包括X轴滚动条高度
4. el.clientWidth;  返回元素自身宽度 不包括边框和滚动条， /浏览器窗口中页面可视区宽度-包括Y轴滚动条宽度
5. el.clientX/Y  ；  返回元素在窗口可视区的X、Y轴坐标，不包括滚动条卷入的像素

```js
document.body.clientWidth // 怪异模式获取
document.documentElement.clientHeight; // 获取浏览器窗口内文档页面的可视区高度
window.innerHeight  // 等同效果 区别只在于它包含了滚动条

div.clientHeight; // 返回元素高度 不包括边框和滚动条

// 封装
// 浏览器内窗可视区计算
function getViewportSize () {
  if (window.innerHeight) {
    // 包括X/Y轴滚动条宽高
    return {
      width: window.innerWidth,
      height: window.innerHeight
    }
  } else {
    // 浏览器怪异模式的处理逻辑
    if (document.compatMode === 'BackCompat') {
      return {
        width: document.body.clientWidth,
        height: document.body.clientHeight
      }
    } else {
      // 不包括x/y轴滚动条宽高
      return {
        width: document.documentElement.clientWidth,
        height: document.documentElement.clientHeight
      }
    }
  }
}
```



总结：client系列用于获取 {元素/浏览器窗口} 的内部宽高(可视区)



### scroll系列 (重要)

注释：用于获取元素滚动的距离 可读写。坐标X/Y，元素也可以是window或者文档document

- scroll系列主要用于获取滚动条到文档/窗口的上/左侧滚动距离，
- 以及整个文档/宽高包括滚动条件下的不可见区域，不包括外边框
- 可以滚动属性
  - window.scrollBy(x,y);  在窗口中按指定的偏移量滚动文档
  - window.scrollTo(x,y);  滚动到文档中的某个坐标

```js
element.scrollHeight    // 返回元素自身高度 包括溢出部分，不包括边框
element.scrollWidth     // 返回元素自身宽度，包括溢出部分，不包括边框
element.scrollLeft      // 获取/设置元素的内容滚动时卷进去左侧的像素数,等同window.pageXOffset
element.scrollTop       // 获取/设置元素的内容滚动时卷进去顶部的像素数, 等同window.pageYOffset

element.scrollX/Y 等同于 element.pageXOffset/pageYOffset // 返回文档水平/垂直位置带px

// 滚动距离检测(滚动条滚动时卷进去顶部或左侧的像素数)
function getScrollOffset () {
  if (window.pageXOffset) {
    // return 一个引用对象
    return {
      left: window.pageXOffset,
      top: window.pageYOffset
    }
  } else {
    return {
      // body.scroll与 documentElement.scroll两者只会存在一个,不存在的就是0
      left: document.body.scrollLeft + document.documentElement.scrollLeft,
      top: document.body.scrollTop + document.documentElement.scrollTop
    }
  }
}

// 测量添加滚动条情况下整个内窗的宽高 (包括滚动条情况下的不可见区域)
function getScrollSize() {
    if(document.body.scrollWidth) {
        return {
            width: document.body.scrollWidth,
            height: document.body.scrollHeight
        }
    } else {
        return {
            width: document.documentElement.scrollWidth,
            height: document.documentElement.scrollHeight
        }
    }
}
```





### offset与style区别

1. offset 可以得到任意样式表中的样式值，获得的单位不带px单位，offsetWidth包含padding+ border+width,且属于只读属性，无法更改/赋值； 因此 获取元素大小位置 使用 offset
2. style   只能得到行内样式中的样式值，style.width获得的是带有px单位的值，且不包含padding和border,可读写，因此  对元素进行更改/赋值  使用style
3. 总结：所有系列都可以测量任何元素的宽高，但是每个都术业专攻
   1. offset 系列用于获取元素完整的宽高或与带有定位的父元素间距
   2. client 主要用于测量元素的边框border 或浏览器内窗(可视区)的宽高，
   3. scroll 用于测量滚动坐标/滚动距离，滚动条件下的整个元素的宽高包括溢出部分
      1. 如果要测量文档页面的完整高度包括滚动条溢出部分，使用 document.documentElement.scrollHeight
   4. 所有的系列都有 window对应的属性，相差不大，主要用于兼容性，不考虑兼容性可以不使用window对应属性

### 鼠标在元素内的坐标

```js
let div = document.querySelector('div');
div.addEventListener('click',function(e){
    // 利用事件对象获取鼠标与文档页面的距离，
    // 再减去元素距离文档页面的距离就是鼠标在元素内部的坐标
    let x = e.pageX - this.offsetLeft;
    let y = e.pageY = this.offsetTop;
    
    this.innerText = 'x坐标是:' + x + 'y坐标是:' + y;
})
```



### 获取伪元素的宽高

- 只读

```js
window.getComputedStyle(div1,'after').width
```





### 模态框拖拽

注释：要点title为点击元素区  login为整个模态框区

- 鼠标的移动和弹起必须写在鼠标按下事件函数的内部
- 原理: 利用属性在文档的坐标 减去 元素距离文档左侧/顶部的坐标 = 得出属性位于元素内部的坐标，
  - 然后再用鼠标位于文档的坐标，减去鼠标位于元素内部的坐标 = 得出元素实际距离文档左侧/顶部的间距
  - 将这个间距不停的赋值给元素，元素就能移动起来

```js
// 1: 鼠标按下时获取鼠标在元素中的坐标
// 3：鼠标移动时需要实时动态的获得在页面document中的坐标
// 4：鼠标弹起时解除事件

title.addEventListener('mousedown', function(e) {
// 1:鼠标到文档左侧距离减去模态框到文档左侧距离 = 鼠标在模态框的实际坐标
let x = e.pageX - e.target.offsetLeft, // page/client都可以获取文档距离client时文档可视区内
    y = e.pageY - e.target.offsetTop,
    _self = this; // 保存this
    
 // 监听文档
document.addEventListener('mousemove', move);
// 使用具名函数作为参数 方便移除事件
function move(e) {
    // 用鼠标在文档的坐标pageX 减去 鼠标在模态框内的实际坐标，就是模态框距离文档左侧的距离
    // 最后鼠标移动时进行实时赋值，模态框就会持续变化坐标实现拖拽
	_self.style.left = e.pageX - x + 'px';
	_self.style.top = e.pageY - y + 'px';
}
    
// 鼠标弹起 移除事件
document.addEventListener('mouseup', function() {
	document.removeEventListener('mousemove', move)
  })
})
```



### 拖拽元素

具体参考文档使用

### 拖拽事件

- **dragstart**   拖拽一个元素 或选择文本 时触发
- **dragend**    拖拽事件结束时触发
- **dragover**  当元素或文本被拖拽到一个有效位置目标上时触发
- **dragenter**  当拖动的元素或选择的文本进入有效的放置目标时触发
- **dragleave**  当拖动的元素或选择的文本离开有效的放置目标时触发
- **drop**    **当一个元素或是选中的文字被拖拽释放到一个有效的释放目标位置时，drop** 事件被抛出





# DOM监控

- 用于监控指定DOM的变化，一个异步执行的回调，详情参照MDN文档
- 可以观察的事件包括：属性变化，文本变化，子节点变化



### MutationObserver观察变化

- new MutatioinObserver(回调函数)
  - observe(要观察的节点,配置项要观察什么变动)
- 它是异步注册的，在同步元素执行后才会触发回调

```js
let div = document.getElementsByTagName('div')[0];

// 观察配置
const config = {attributes: true, childList: true, subtree: true}

// 回调函数
const callback = function(){
    console.log('DOM changed')
}
// 创建MutationObsercer
let observer = new MutationObserver(callback);

// 开始观察
observer.observe(div, config)

// 更改div文本 会触发观察接口的回调函数
div.innerText = 'xjj'; // DOM changed
```



# JSON数据

- JSON是一种纯数据，用于数据交互
- 所有编程语言的三大特征
  - scalar 标量：  --> 字符串  数字
  - sequence 序列： --> 数组  列表   list array
  - mapping 映射：键值对  键名: 键值
- 映射示例

```js
"name": "张三"   // 映射用冒号分开 键名一定要用双引号

// 并列数据用逗号隔开
"name": "张三",
"age": 18

// 映射的集合用 {} 包裹
{
    "name": "张三",
    "age" : 18
}

// 并列数据集合用 [] 包裹
[
    {
        "name": "张三",
        "age": 18
    },
    {
        "name": "李四",
        "age": 17
    }
]
```







### 获取设备物理像素比

```js
let dpr = window.devicePixelRation || 1  // 1或2
```



## 移动端touch

注释：Touch对象接口没有父类不继承任何属性

1. touchstart  :  移动端触摸事件
2. touchmove : 移动端触点移动事件
3. touchend : 移动端触摸事件结束触发，即:按住屏幕后松开触发
4. Touch.clientX/Y : 返回触点相对于可视区左/顶部边缘的坐标，不包括滚动距离，注：只读属性
5. Touch.pageX/Y  : 返回触点相对于文档左侧.顶部边缘的坐标，包含滚动，注：只读属性

```js
// 获取手指触摸屏幕的坐标
let fingX = 0;
let fingY = 0;
let 
box.addEventListener('click',functiion(e){
     e.targetTouches[0].pageX;  // targetTouches 手指列表
     e.targetTouches[0].pageY; // 0表示最先开始触摸的手指 
 })
```

注释：了解即可，实际移动端的时间需要单独学习



## 轮播图动画

注释：

1. 动画函数封装

```js
/**
 * 元素平移动画
 * @param {位移对象} el
 * @param {位移目标数值} target
 */
function moves(el, target) {
	clearInterval(el.timeID);
	// 获取盒子距离文档边框的距离
	let position = el.offsetLeft;
	// 如果盒子到文档左侧的距离小于目标值 就走正值 反之负值
	let step = position < target ? 60 : -60;

	el.timeID = setInterval(function() {
		// 正常移动
		position += step;
		// 如果目标值减去当前盒子与文档左侧距离 得出的剩余距离大于步长 继续移动
		if (Math.abs(target - position) > Math.abs(step)) {
			el.style.left = position + 'px';
		} else {
			// 反之就停止移动并停止到目标值终点位置
			el.style.left = target + 'px';
			clearInterval(el.timeID);
		}
	}, 20)
}
```



### 手写轮播图

注释：

1. 样式：设置1个div再包裹一个div 命名moverWidth
2. 放入ul 与div。同级元素 ，div放入span 设置绝对定位
3. 获取第二层div的宽度 即图片宽度
4. 循环span并设置i自定义属性用于联动
5. 绑定事件 设置span样式 采用排他思想
6. 获取自定义属性作为目标值，每次点击事件后乘以moverWidth得出每张图要移动的宽度，用变量target接收
7. 调用动画函数，将ul作为目标移动元素，target作为移动数值，传入动画函数

```js
// 获取轮播图元素的时候一并获取元素自身宽度
let moveWidth = document.querySelector('.inner').offsetWidth;

for (let i = 0; i < spanArr.length; i++) {
	// 设置自定义属性
	spanArr[i].setAttribute('index',i)
	spanArr[i].addEventListener('click', function() {
		for (let i = 0; i < spanArr.length; i++) {
			spanArr[i].className = '';
		}
        // 点击时按钮的样式类
		this.className = 'current';
		// 获取按钮自定义属性 乘以图片宽度 负 得出目标值
        let target = -this.getAttribute('index') * moveWidth;
		// 调用轮播图动画函数
		moveAnimation(ulBox, target)
	})
}
```

注：轮播图的箭头与数字按钮的联动：将自定义属性索引赋值给箭头按钮，即实现索引相等->双联动



### 表格创建原理

1. 层级-即： 先有tr 行， 再有td, --> 将td追加到tr， 

2. 如果td内部还要有层级嵌套:  如td内放2a链接按钮 删除编辑，
   1. 先创建一个td 再创建2个a
   2. 将a追加到td
   3. 再将td追加到tr

3. 完毕再一次性将tr追加到tbody显示到页面结构

   ```js
   // trBox 行，tbody表格
   function(){
       // 创建tr行
       let trBox = document.createElement('tr');
       // 创建td单元格
       let nameTd = document.createElement('td');
       // 追加到tr行
       trBox.appendChild(nameTd)
       
       // 创建一个td单元格 内部在嵌套放入2个a
       let btnTd = document.createElement('td');
        // 创建a
       let delA = document.createElement('a');
       let upA = document.createElement('a');
       // 将a追加到td
       btnTd.appendChild(delA);
       btnTd.appendChild(upA);
       // 再将td追加到tr
       trBox.appendChild(btnTd);
       
       //  最后一次性将所有tr追加到表格tbody;
       tbody.appendChild(trBox)
   }
   ```





### 优化表格创建

```js
// 点击按钮添加元素
addBtn.onclick = function() {
    // 获取用户输入值 姓名  邮箱
	let uname = userName.value.trim();
	let email = userEmail.value.trim();
	if (uname == '' || email == '') {
		alert('输入值不能为空');
		return;
	}
    // 调用创建元素函数
	createTabel(uname, email);
	userName.value = ''; // 每次添加完毕清空输入框
	userEmail.value = '';
}

// 封装函数 利用模板字符追加
function createTabel(name, email) {
	let str = `<tr>
		       <td>${name}</td>
		       <td>${email}</td>
		       <td>
                   // 事件的行内写法 行内写法this指向window 将其作为参数即可
	              <a href="javascript:;" onclick="removeTr(this)">删除</a>
				  <a href="javascript:;" onclick="upTr(this)">上移</a>
				  <a href="javascript:;" onclick="downTr(this)">下移</a>
				</td>
				</tr>`
		// 追加到tbody显示
		tBody.innerHTML += str;
  }

// 行内样式点击事件this指向window 以参数形式传入即可获取当前tr行
function removeTr(el) {
	// 获取当前a标签的父节点
	let target = el.parentNode.parentNode;
	// 删除当前行
	if (confirm('确定删除?')) {
		tBody.removeChild(target)
	}
}

// 上移
function upTr(el) {
	let target = el.parentNode.parentNode;
	if (target == tBody.children[1]) {
		alert('已经是最顶一行了');
		return;
	}
	// 追加到元素头部 与appendChild相反
	tBody.insertBefore(target, target.previousElementSibling)
}

// 下移
function downTr(el) {
	let target = el.parentNode.parentNode;
	if (target == tBody.lastElementChild) {
		alert('已经是最底部了');
		return;
	}
	// 下移相反  将target的下一个兄弟作为参数 移动到target的位置互换
	tBody.insertBefore(target.nextElementSibling,target)
}
```



# 模块化

- AMD； RequireJS
- CMD: SeaJS
- CommonJS: Node社区
- ES  Module: ECMA官方

- 模块化的核心：将脚本分成一个一个的小块，可以在任意脚本中引入使用

```js
export function test(a,b){return a + b}; // A 脚本暴露
export function test2(a,b){return a * b}; // A 脚本暴露
export {test,test2} // A脚本暴露方式二 {模块集合的容器}
// A脚本暴露方式三(如果是多个方法，不推荐这种写法，需要解构使用)如果是单个组件,推荐使用这种方法
export default { 
    test(a,b){return a + b},
    test2(a,b){return a * b}
}

// B脚本引入使用
import {test,test2} from './src/xxx';
// 单个组件导入使用
import myModules from './src/xxx'
```



## vite使用

- npm init -y   生成pack文件
- npm add vite -D   下载vite环境

```js
// 修改脚本调式命令 
"scripts": {
    "dev": "vite"
  },
```





































