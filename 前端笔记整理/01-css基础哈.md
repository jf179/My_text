HTML

## 主要属性/标签

1.  a  超链接标签  属性: href 路径, target 跳转目标(_bank新窗口打开),  
2.  <del> 删除线，
3.  a标签的css属性：text-decoration:line-through; 也是删除线
4.  注意：p标签不能嵌套div，会解析出2个p标签
5.  <sup>  上标签，字体样式出现在左/右上角，<sub>则相反，都属于内联标签
6.  font-style: italic;  使字体变为斜体，em标签也可以斜体
7.  font-style:oblique;  当元素没有italic属性时使用oblique使其强制斜体
8.  font-family: "Times New Roman", Georgia;  引号内部是字体声明可以是多个浏览器会自动选择可以生效的一个

```css
<-- 有序列表 按照Number数字排序- reversed属性可以倒转排序 start属性则可以指定排序开始位置，默认为1>
<--- type属性则可以为其指定排序类型 A/a 按照字母排序，默认Number排序 --->
<ol reversed> 
 <li>第一个</li>
 <li>第二个</li> 
</ol>

< --- 描述列表——-->
<dl>
  <dt>hello</dt>
  <dd>world</dd>
</dl>

< ---- 无序列表 --> (常用)
<ul>
  <li>hello</li>
</ul>
```



## table表格

1. table 的样式问题
   1. 除 text-align 属性外 其他属性都属于HTML属性，只能写在行内
2. 主要属性
   1. align  表格位置-以周围元素为参照
   2. cellpadding 单元格间距(内)
   3. cellspacing 单元格边距(外)
   4. border  表格边框粗细
   5. colspan=’2‘； 合并单元格，X轴从左至右合并，目标单元格需要手动删除，可以整行单元格全部合并
   6. rowspan='2';  合并行，Y轴从上往下合并，目标行需要手动删除，顺序-往下合并，

```html
表格类的写法
.table{
 border-collapse:collapse;  // 边框合并
 border-spaing:0; // 外边距
}
.td{
   padding:10px; // 让表格内空间撑开些
}
<----- thead tfoot tbody 会按照顺序加载-先表头-再表底-再主体部分------>   
<table border="1" cellspacing="0" cellpadding="10">
      <thead>
        <tr>
          <th>ID</th>
          <th>年龄</th>
          <th>性别</th>
          <th>爱好</th>
          <th>体重</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>1</td>
          <td>18</td>
          <td>男</td>
          <td>羽毛球</td>
          <td>66</td>
        </tr>
        <tr>
          <td>2</td>
          <td>22</td>
          <td>女</td>
          <td rowspan="2">篮球</td>
          <td>66</td>
        </tr>
        <tr>
          <td>2</td>
          <td>22</td>
          <td>女</td>
          <!-- <td>篮球</td> -->
          <td>66</td>
        </tr>
        <tr>
          <td>2</td>
          <td>22</td>
          <td>女</td>
          <td colspan="2">篮球</td>
          <!-- <td>66</td> -->
        </tr>
      </tbody>
```



## form表单

1. 表单的组成(表单域  表单控件(即元素)  提示信息)

2. label标签：配合input使用，可以聚焦元素

3. target属性：下划线blank在新窗口打开，下划线self在当前窗口打开

4. enctype属性: 发送前对发送数据进行何种编码格式

   1. 默认：application/x-www-form-urlencoded 编码
   2. multipart/form-data: 涉及上传文件时必须指定该值

   

   ```html
   <form action="后台url地址" method="提交方式GET/POST" name="表单域名">
       用户名: <input type="text">   //定义文本输入
       密码:   <input type="password">   // 定义密码输入
       单选:   <input type="radio" name='btns'>女  // 给多个单选按钮设置同一个name名，实现多选一
       单选：  <inpt type='radio' name='btns'>男</inpt> 
       复选:   <input type="checkbox">   // 定义多选框
       vaule值: <input type="text" value="请输入">
       checked:<input type="text" checked="checked">
       提交;    <input type="submit" value="提交"> 
       重置:    <input type="reset" value="重新填写"> 
       botton: <input type="botton" value="获取短信"> 
       上传：   <input type="file" value="上传文件"> 
       label:  <label for="sex">男</label>
               <input type="radio" id="sex"> 
       下拉元素;<select>
       可选项： <option selected>湖南</option>
               <option>山东</option>
               <option>菏泽</option>
               </select>
       文本区: <textarea row="3" cols="20"></textarea>
       提交表单: <input type="submit" value="提交">
   </form>
   
   <---- 分割线--->
    <form acition='./active' method='get'>
        // 给label标签的for属性指定一个id名即可聚焦到对应元素,如单/复选框,input框
        <label for='username'>用户名</label>
        <input type='radio' id='username' />
       </form>
   ```

   1. url 用于指定接收处理表单数据的服务器地址
   2. method提交方式：  可选: get / post
   3. name 用于区分页面中的多个表单 (提交的属性值)

## input表单属性

1. 多个单选框实现多选一,需要在多个input内启用相同的name名如:name="sex"，只对单选框radio有效
2. vaule 值是input元素自带的属性，可定义内容值,
3. input事件：用户输入时实时触发获取元素值
4. change事件：元素失去焦点后触发，同样可以获取到元素值
5. checked   定义默认选中,  maxlength  定义输入字符长度
6. image 定义图像按钮，rest 定义重置按钮
7. button 定义可点击按钮,搭配js使用 ，submit 定义提交按钮
8. label 为input元素定义标注 增加用户体验,使用for在label标签内定义一个名字，对应 input内的 ==id== 名,即可作用于标签内，目的就是增加了作用范围
9. outline: none;  取消input和文本域 默认边框颜色
10. maxlength='5';    限定最大输入字符
11. minlength='2';    限定最小输入字符
12. disabled;   禁用输入框，并显示灰色
13. readonly;  与disabled类似，区别在于它是只读，无法在输入框输入字符
14. submit 提交事件 表单属性将会被发送到 action 属性指定的URL上

## select表单下拉元素

1. select内包含任意多个 option属性，用户多项选择如籍贯等
2. 定义默认选中项 ，简写 selected
3. value属性：选中时提交给表单的值，如果没有则默认获取文本值
4. disabled:  禁用选项，且不接受任何浏览器事件

```css
<select name=''>
   <option value='xxx'>  湖南 </option>
</select>
```



## 表单控件组

- 打包表单，对其进行分组

```css
<fieldset>
   <legend> 用户注册 </legend>
 <form for='' method='get' name='one'>
     <label> 姓名 </label>
     <input type='text' id='username' />
  </form>
</fieldset>
```





## textarea 表单文本元素

1. textearea 用于定义表单文本，大量书写文字的时候
2. rows定义文本区行数，cols 定义文本区可视宽度
3. resize:none;  禁止拖拽文本域
4. disabled   禁用文本域
5. maxlength/ minlength;  限定最大/最小输入字符
6. radonly； 禁止用户编辑文本域

```css
<textarea readonly>
   
</textarea>
```



## 描点链接

- a标签的作用：超链接，电话拨打，发邮件，锚点定位，协议限定符

```html
<a href='#footer'> 点我内部跳转位置</a>
<p id='footer'> 我在这里 </p>

<a href='http:// baidu.com' target='_blank'> // _blank在新页面打开连接
<a href="javascript:alert('打开一个框')">点我打开</a> // 协议限定符
<a href="javascript:;">点我打开</a>   // 协议限定 阻止跳转
```



## 文字间距

注释：letter-spacing：px;   可用于调整文字之间的间距



## object-fit

注释：主要用于图片或播放器 的裁剪，适应宽高



## iframe标签

- 内嵌框架标签，可以将一个HTML页面嵌入到当前页面中，属于行内块元素 inline-block;

```css
<iframe style="background-color: burlywood;width: 80%; height:500px;" src="https://bbs.csdn.net/topics/391896625" frameborder="0">
</iframe>
```



------



# 元素要点

## 元素的特点

1. 块级元素：默认独占一行,可以设置宽高
2. 内联块元素: 可以设置宽高,且一行可以显示多个,但多个元素之间会有默认间距,解决默认间距的办法：设置margin:right | left ; 为合适的负值即可
3. 内联元素: 一行可以显示多个,不能直接设置宽高,默认宽度是本身内容的宽度, 可以通过 display 进行转换

```css
// 块级元素
div, p, address, ul , ol , li ,dl ,dt, dd , table, form, fieldset, legend,

// 内联块元素
input , img, select, textarea,

// 内联元素
span, strong, del, ins , label, a , sup, sub
```



注意: 行内元素里面不能放块级元素, a标签除外(如果放块级元素最好吧a也转换下),    

1. display:block;转为块级元素，inline-block行内块元素，inline 行内元素
2. font-style:normal;    取消元素默认样式，以标准样式显示

## 元素的显示/隐藏

1. visibility: visible / hidden; 可见性(使元素不可见,元素原位置继续保留)
2. display: block / none;   显示方式 (none使元素隐藏,不保留元素原有位置)  
3. float:浮动方位；只能左右浮动.开启后会自动将元素转为块级
4. overflow: visibile/auto/hidden/scroll;  溢出内容管理auto自动适应 scroll为元素添加滚动条 无论内容是否溢出

## 元素/轮廓边框

1. outline: color | style | width;   轮廓的复合属性写法,画border外边 作用于图片 按钮 input 等
2. border: width | style | color;    边框的复合属性写法,  边框是向外扩散的， 详细边框的样式看看离线文档，
3. border:复合写法 四个值分别为：左上 右上  右下 左下。顺时针。



## 移动端特殊样式

```css
box-sizing: border-box;
-webkit-box-sizing: border-box; // 兼容性
-webkit-tap-highlight-color:taansparent; // 取消高亮效果
-webkit-appearance: none; // 移动端加上这句才能给ios设备上的按钮和输入框设置自定义样式
img,a {-webkit-touch-callout:none;} // 禁用长按页面时的弹出菜单
```



# CSS

## 选择器

​	1.并集选择器: 标签之间使用逗号分割, 如 ：div, p

​	2.后代选择器: 可以是任何标签 父标签 后面空格跟上子标签即可,如:   .pig li	

​	3.交集选择器：可以是任何标签按照各自规则书写即可，

​           如: div,p, .pig li

## 伪类选择器

1. anchor链接伪类   注意书写顺序 (==无herf属性则不作用==)

   - a:link  /设置a对象未被访问前的样式
   - a:visited /设置a对象已被访问后的样式
   - a:hover /设置a对象被鼠标悬停时的样式
   - a:active / 设置a对象被鼠标点击未弹起时的样式

2. first-child 结构伪类

   ```css
   .btn:first-child{};     // 父元素内第一个子元素
   .btn:last-child{};      // 父元素内最后一个子元素
   p:nth-child(1){color:pink}；// 父元素内第n个元素 支持奇数偶数
   
   div p:first-of-type{};   // 父元素内某一类元素得第一个
   div p:nth-of-type(n);    // 父元素内某一类元素得第n个
   ```

3. nth-child 与 nth-of-type区别：nth-child()不区分元素类型，按照父元素内得所有元素类得排名，nth-of-type可以指定父元素中某类元素-根据指定元素的排名进行选择；

4. focus 表单伪类

   - input:focus    / input获取焦点

5. 伪元素

   - :before      /在元素之前插入

   - :after         /在元素之后插入

     

     

## css三大特性

1. 层叠性

   - 用于解决样式冲突,同类标签遵循就近原则,未产生冲突的属性不层叠

2. 继承性

   - 子元素可以继承父元素的某些样式如 text-, font-,line-,以及color颜色,设置父元素的行高如1.5可以动态更改子元素的行高

3. 优先级

   - 选择器相同执行层叠性

   - 选择器不同, 则根据选择器权重执行

   - 权重可以叠加，不会产生进位问题,==注意==继承的权重为0

     ```html
     选择器             权重
     *通配符选择器        0
     元素选择器           1
     类、伪类选择器       10
     id选择器            100
     行内style样式       1000
     !important         无穷大
     max/min-width      会覆盖!important
     而max/min-width 之间相互覆盖取最大值优先
     ```

   - 

   - z-index:auto | 数值;  用于定位的元素层叠级别， auto 遵从父元素的层叠顺序。数值越小代表权重越高

## 背景属性

### 背景图片

复合写法:

1. background:url(图片路径) 是否平铺repeat  图片位置position(上右下左)
2. background:position | size | repeat | attachment | image | color | clip ;  。
3. position 的取值如果只写2个，  则 第一个代表X轴 第二个代表Y轴，如果只写一个值则默认X轴 Y轴默认50%即居中,如果写四个值则是:上/右/下/左
4. attahment:默认scoll随页面滚动而滚动,使用属性 fixed则可以固定位置。
5. 背景属性取值, 可以使用 ==复合写法==，如;background:red url() no-repeat fixed center top;即颜色 地址 是否平铺 固定方式 位置设定。



### 背景色半透明

可以设定盒子元素半透明

1. background:rgba(0, 0, 0, .3); r代表红色  g代表绿色  b代表蓝色  a  .3代表透明度。注：盒子内部的内容不受影响



### 倍图缩放

1. cover；优先横向适应宽度，
2. 宽、高；按照宽高顺序适应
3. contain; 优先适应高度，宽度不足则留白

```css
div{
    width:300px;
    height:200px;
    border:1px solid red;
    background:url('./');
    background-size: 300px 200px; // 或用cover
}
```



### 让图片适应

- object-fit: contain   让图片自适应 不变形



### 响应式图片

- 让图片横向不会超出父元素宽度，根据父元素响应式的进行变化

```js
.img{
    max-width:100%;
    height:auto;
}
```



### 图片分辨率适应

- 在高分辨率显示器上应该 适应两倍图，将其宽高设置为原始宽高的一半(或者使用rem转换)

```js
img{
    width: 100px;
    height: 100px;
}

// 假设原始图片宽高都为200px
<img src='./xxx'>
```





## 盒子模型

1. 标准盒模型 box-sizing: content-box;
   1. margin  border  padding   content  即 元素本身宽高值+内外边距+边框+内容
2. 怪异盒模型：box-sizing: border-box;
   1. 包括 maring border padding content 即 元素本身宽高+内外边距+内容；
   2. 与标准盒模型不同，它将在元素内部进行绘制，也就是padding和border都被包含在width内部了
3. 塌陷
   1. 垂直相邻盒子如果产生塌陷
      1. 可以给其中一个设置边距
      2. 启用定位
4. window.getComputedStyle(div).width; 可以获取元素的指定属性，返回原始值，不包括边框内外边距等
5. offsetWidth/Height;  返回元素自身宽高，包括边框内外边距



### 元素的水平/居中/对齐

1. text-align; 设置在父元素上  前提是转换成inline-balok，
2. margin：0 auto；
3. 开启弹性布局：justify-content:center;   align-items;center;
4. vertical-align: middle/top/bottom;  行/行内块元素如图片表单与文字中线对齐，只对行内及行内块元素有效
5. 图片底部缝隙解决：1 转成块级元素，2 使用vertical-align:bottom;(推荐)

## 伪元素妙用

```css
<---- 利用伪元素 在元素前.后添加小图标实现图标与文字对齐 方便--->
<---- 那里需要那里调用就行 ----->
span:before{
    content:url(./img/xxx);
    margin-right:5px;
    vertical-align:middle/top/bottom;
}
<span> </span>

----------分割线---- 用于后端传回的简单数据-------
p:before{
    content:attr(data-username); // 可以将后端数据与元素文本进行拼接
}
<p data-username='superatc'> 大老板 </p> // 可以将后端的数据存入变量放入即可使用
```





### 盒子居中

- 让一个具有宽高的盒子垂直水平居中
- 给父元素设置固定宽高，子元素宽高100%，随后设置父元素的padding四个方位全部一致距离即可

```css
.box{
    width:100px;
    height:100px;
    border:1px solid skyblue;
    padding:30px;
}
.son{
    width;100%;
    height:100%;
    background-color:pink;
}

<div class='box'> 
  <div class='son'> </div>
</div>
```





### 盒子阴影

1. box-shodw:常用5个属性
2. box-shadow:x轴 y轴  模糊度  v轴(远近距离/放大缩小)   rgba(0,0,0,0-1之间)

```css
.box{
    width:100px;
    height:100px;
    background:pink;
    box-shodw:10px 10px 7px 5px;
}
```





### 线性渐变

```css
background: linear-gradient(to 方位, 颜色1，颜色2)
```



### 浮动元素的布局准则

1. 浮动元素布局搭配标准流，即---- 用标准流父元素进行上下排列 内部再使用子元素浮动进行横向排列,
2. 注意:一浮全浮
3. 导航栏一般采用li标签包含a链接,防止关键字堆彻风险,且可以增加点击范围
4. 元素开启浮动后，自动转为块元素不能再与display属性共存
5. 开启浮动后，允许文本和内联元素环绕它
6. 脱离标准保留-不占位，会影响后面元素的正常布局-如果后面元素需要按照正常流布局则需要清除浮动

### 清除浮动带来的影响

由于浮动的元素不再占有原来的位置，布局时在父元素不方便给出固定高度的时候，此时如果子元素进行了浮动就会导致父元素的高度为0，从而影响后面的兄弟元素正常布局，此时就需要清除浮动

1. 为父元素添加 ：overflow:hidden;   <u>ps</u>:此方法无法显示溢出部分


```css
.clearfix:after{ // 在(元素)...后插入伪元素。
    content:"";  // 生成虚拟内容块
    display:block;  //必须是块级元素
    height:0;
    clear:both;  // 清除两端的浮动影响
    visibility:hidden;   //可见性=隐藏
}   // 然后在需要清除浮动的父元素类名内部直接调用 clearfix 如: <div class="btn clearfix">即可
```

2:添加双伪元素

```css
双伪元素清除浮动
.clearfix:before,
.clearfix:after{
    content:"";
    display:table; // 必须是块级元素
}
.clearfix:after{clear:both;} 
.clearfix{*zoom:1;} // 照顾IE7以下低版本浏览器
在父元素内部调用即可
```



### 清除浮动的前提

1. 父级元素未设置高度(不方便给出固定高度即:需要让内容自动撑开)

2. 子盒子浮动(会导致未设置高度的父元素塌陷)，且影响后面元素的布局时，

   给添加浮动的元素设置margin:bottom;时需要精确计算父盒子的高度值，

### 浮动小技巧

1. 一排浮动的盒子如果设置了边框,相邻的边框会产生1+1大于1的情况，即相邻边框变粗，
2. 利用margin:负值;  使相邻的边框叠压即可解决，
   1. 如果此时想让鼠标经过某个相邻盒子显示四个边框，那么使用z-index层叠权重即可，（前提是设置了相对定位，相对定位的盒子会压住其他盒子而如果相邻的盒子都加了相对定位就使用z-index）
3. 行内块元素也会有间距将元素空隙去掉即可，正确做法是margin负值

### 行内块小技巧

1. 父元素没有宽高的居中办法,使用text-align:center; 内部子元素即可居中
2. 将a标签转换成行内块,可以进行分页按钮设置



##  定位(position)

- 定位可以让盒子自由在某个盒子内部移动位置/固定，并且可以压住其他盒子(不会压住文字)

### 定位的组成

定位模式+边偏移  （即定位方式+移动位置(坐标)）定位模式如下

1. relative:  以自身位置为参照坐标,移动后原来位置继续占有/不脱离标准流.
2. absolute:以祖先元素为参照坐标, 移动后不再占有原来位置/ 脱离标准流.
3. fixed :  固定在浏览器可视区的某一位置,不随滚动条而滚动. 脱离标准流
   1. 子绝父相: 即子元素使用absolute绝对定位时,必须搭配父元素relative，来进行范围限制
   2. fixed小技巧: 固定定位的盒子走可视区(版心)left:50%; 再让固定定位的盒子margin-left走版心宽度的一半,即可实现右侧外固定，垂直居中同理
4. 如果两个盒子都设置了绝对定位，那么后面的盒子就会压住前面的盒子，可以通过z-index进行控制

```css
// 实践 配合z-index实现并排布局的样式统一
div p{
    position: relative;
    margin-left: -1px;
    float:left;
}
div p:hover{
    z-index:1;
}
```



###  定位的叠放次序

1.  X 水平轴   Y 垂直轴   Z 三维轴(即空间轴), 布局中如果出现盒子重叠的情况,可以使用 Z轴 z-index 来控制盒子(同一坐标位置)的前后排列次序，
2.  z-index:正整数/负整数/0;   数值越大越往上,默认是auto 即按照顺序层叠 后来者居上,只有开启定位的盒子才有z-index属性
3.  开启绝对定位的盒子不能再通过 margin: auto; 进行水平居中,解决办法left:50%; 再使用margin-left:负自身宽度的一半；
4.  开启固定/绝对定位/浮动的盒子会自动转换为块元素，可以直接进行宽高设置，

浮动元素只会压住其后面标准流的盒子 不会压住盒子内的文字内容，因为浮动本身就是设计用来进行图文环绕展示的，而绝对定位的盒子会将后面的标准流元素整个压住

字体图标的使用

1. 下载字体图标文件---打开style.css复制@font-放入css样式表--打开demo.html--再将fonts文件夹放在根目录下-
2. 然后在demo.html里面复制想要使用的图标代码放入需要使用的html文档标签内即可

### 三角形的做法

1. 设置一个没有宽高的盒子--给每个角设定边框粗细+实线+背景颜色--此时可以根据需要将其中三个边框的颜色同时设置为透明状即可得到一个三角形，
2. 调整边框的大小可以调整不同形状值的三角
3. 使用定位+负值--可以让三角形凸出在父盒子外部
4. 设定2个边框配合2D旋转可作出倒三角

```css
div{
    width:0;
    height:0;
    border:10px solid transparent; // 将其他三个角设置透明
    border-top:10px solid red; // 将其中一角设置显示 形成三角形
}
```



### 文本溢出处理

```css
white-space:nowrap; 强制一行显示
overflow:hidden; 溢出部分隐藏
text-overflow:ellipsis; 用省略号代替溢出部分
```

```css
// 多行溢出处理
overflow:hidden; 溢出部分隐藏
text-overflow:ellipsis; 用省略号代替溢出部分
display:-webkit-box; 开启弹性盒子模型
-webkit-line-clamp:2; 限制在一个块级元素显示的文本行数值
-webkit-box-orient:vertical; 设置检索伸缩盒子的子元素排列方式
```



## flex布局

- flex布局适用于任何元素

1. flex-direction:row/row-reverse/column/column-reverse; 控制方向
   1. row 水平X方向从左至右-默认方向
   2. row-reverse 水平方向，从右至左排列
   3. column  主轴为垂直方向Y，从上至下排列
   4. column-reverse  主轴为垂直方向，从下至上排列
2. flex-wrap: 控制换行 默认nowrap,可选wrap/wrap-reverse
3. justifdy-content: 控制主轴元素水平分布方式
   1. flex-start  左对齐
   2. flex-end 右对齐
   3. center  居中对齐
   4. space-between 两端对齐，等距分布
   5. space-around  等距平均分布
4. align-items 控制侧轴垂直排列方式，只针对单行
   1. flex-start  交叉轴起点对齐-默认
   2. flex-end 交叉轴底部对齐
   3. center  交叉轴中点对齐
5. align-content  控制侧轴垂直排列方式，针对多行
   1. flex-start  起点对齐-第二排与第一排不会有间距
   2. flex-end 终点对齐
   3. center 居中对齐排列-第二排有间距
6. 项目属性-针对指定元素
   1. order 属性值为number，数值越小排列越靠前
   2. flex:1;   数值越大分布到的空间越大
   3. flex-grow 默认0 如果布局空间存在剩余空间也不放大，保持指定宽高
   4. flex-shrink 魔法缩放比例1，如果空间不足自动缩放比例
   5. flex 以上3个属性的简写 默认order值为0 缩放比例为1 自动分布空间
7. align-self  允许自身与并排元素的排列方式不一致，与align-items属性类似

总结：父元素开启display：flex；盒子后 所有子元素将成为其子项目，再设置 float  和clear和vertical-align将无效



## HTML5 新增语义化标签

1. header:  头部标签   2. nav:  导航标签
2. article: 内容标签     3. section:   定义文档区域 - 类似div
3. aside :  侧边栏标签  4. footer:   尾部/底部标签

### 新增input表单元素

以下新增标签必须包含在form标签内部，提交时才能进行验证

1. search: 搜索          2. email: 限制输入类型必须为邮箱
2. url: 必须为url        3. date: 必须为日期类型
3. month:必须为月   4.week:必须为周
4. time:必须为时间     5. number:必须为数字类型
5. tel:手机号码            6. color:生成一个颜色选择表单

### 新增表单属性

1. required: 内容不能为空        2. placeholder:提示文本信息/==占位符==
2. autofocus: 文档加载完毕后自动聚焦到指定表单
3. multiple: 可以多选文件提交
4. autocomplete: 根据用户前期输入记录进行提示选项,默认打开:autocomplete="on",关闭 autocomplete="off"

### 新增选择器

新增的选择器极大的提高了效率,对相同元素的选择不再需要单独使用类名

### 新增属性选择器  (权重10)

1. [class];   选择所有具有class属性的元素

2. p[class] : 选择具有class属性的p元素

3. p[class="box"] : 选择具有btn属性且属性值等于val的p元素(**重点使用**)，对伪元素无效

4. p[btn^="val"] :    匹配具有btn属性且值以val开头的p元素

5. p[btn$="val"] :    匹配具有btn属性且值以val结尾的p元素

6. p[btn*="val"] :    匹配具有btn属性且值中包含有val的p元素

   ```css
   input[value]{}        1
   <input type="text" value="输入账号">
   <input type="text">  //end
   input[type=text]{}    2
   <input type="text">  |  <input type="password">
   div[class^=icon]{}    3 结尾同理
   <div class="icon1-data"> |<div class="icon2-data">
   ```

### 新增结构伪类选择器

1. p:first-child:                   匹配父元素中的第一个子元素p

2. p:last-child:                    匹配父元素中最后一个子元素p

3. p:nth-child(5n):            以5为参照往上递增如: ==5 10 15 20 25....==

4. p:nth-child(even/odd): 匹配偶数行/奇数行。写2n/2n+1也代表偶/奇数

5. p:nth-child(n)：          匹配父元素中第n个子元素p。n代表第几个,n+2代                                              

   ​                                         表从第二个开始算起(包含后面的所有元素)多个。     	                                  ==-n+2  代表前2个 包含第2个==

6. p:first-of-type:               指定类型p的第一个元素

7. p:last-of-type:                指定类型p的最后一个元素

8. p:nth-of-type(n)            指定类型p的第n个元素

9. seat.not(.xxx):hover          反选伪类，用于防止特定的元素被选中

   ```css
   ul li:nth-child(1){}
   .helo:not(.Good):hover{} // 选中helo类，但是除去Good类名不要选中它。意思就是反选
   <ul>
   <li class='helo'>第一个</li>
   <li class='helo'>>第二个</li>
   <li class='helo Good'>>第三个</li>
   </ul>   选择ul里面的li标签第一个
   
   ```

   **nth-child与nth-of-type的区别**：

   当父元素下面有多个子级元素(即相同的子级间=兄弟元素),此时nth-child就无法进行精确选择,就是需要用到nth-of-type,它会先区分指定的子元素再进行匹配, 当然也可以使用父元素 跟上子元素再跟上:nth-child()也可以实现与nth-of-type同样的效果

### 新增伪元素选择器(重点)

伪元素选择器可以利用css创建新的标签,不真实存在html文档内部 (**权重为1**)

1. ::before              在元素内部的前面插入内容

2. ::after                  在元素内部的后面插入内容

   ```css
   div::before{
       content:"字符/符号/数值"; // 生成内容
       可以使用定位/转换/图片等属性
   }
   <div>我</div>   
   before与after伪元素属于行内元素,无法设置宽高,需要转换才行
   <常用于输入框和一些小图标的场景>
   ```



### 新增滤镜filter

- 图片模糊/淡化处理


```css
img{
    filter:blur(4px); /数值越大越模糊
    filter:opacity(.7); // 数值越小越淡化
}
img：hover{
    filter:blur(0); /鼠标经过显示原图
}
```



### calc函数

- calc(); 可以对css样式进行一些加减乘除计算，

```css
新增属性处理函数
div{
    width:calc(100% - 30px) /子盒子永远比父盒子小30px
}
```



### 过渡动画(transition)

transition: 需要过渡的属性  花费时间 运动曲线 何时开始；书写在作用元素上

1. 属性: 宽高 背景颜色 内外边距，all 代表所有属性，
2. 花费时间:  单位秒  (必须有单位)   如：0.5s/1s
3. 运动曲线：默认ease (可以省略)
4. 何时开始：单位是秒  (必须有单位)  如:预设触发时间 3s 后开始,默认0

```css
transition:all 1s ease/linear/ease-in 2s; //运动曲线与开启时间可以省略 all表示全部属性需要过渡

transition:width 1s, height 1s; // 同时过渡高度与宽度，
div:hover{
    width:100px;
    height:100px;
}
过渡属性经常搭配 :hover  使用
```



### 新增视频标签

```css
<video src="url路径"> </video>
可选属性
autoplay="autoplay"  视频准备就绪后自动播放,谷歌浏览器需搭配静音
muted="mured"        静音播放
poster="url路径"      加载等待时的画面图片
loop="loop"          播放完毕继续循环
controls             向用户展示播放控件
width-height         设置播放器宽高
```



### 新增音频播放器

```css
<audio src="url路径"> </sudio>
可选属性
autoplay="autoplay"  音频准备就绪后自动播放,谷歌浏览器需搭配静音
muted="mured"        静音播放
controls             向用户展示播放控件
loop="loop"          播放完毕继续循环播放
```



### 网站favicon图标

1. 借助第三方网站将png图片转换成icon再放入文件夹根目录再导入使用即可



# css3 动画

## 2D变形(transform)

可选属性: 平移translate  旋转rotate   缩放scale  transition过渡时间 等

### 平移translate

```css
transform:translate(x,y) // x轴,y轴 如果单独移动一个轴另外一个轴写0即可, 
transform:translateX(100px) // 也可以这样单独写一个轴
transform位移的特性：不影响其他元素的位置。百分比单位仅相对自身元素,
利用百分比配合定位 可以动态的使盒子水平/垂直居中,对行内标签没有效果
position:absolute; top50%;left:50;
transform:translate(-50%,-50%)
```

### 旋转rotate

```css
transform:rotate(45deg) /旋转45度 
倒三角做法 /利用伪元素插入一个盒子 根据需要设置2个边框进行旋转
  div::after{
         content: '';
         position: absolute;
         top: 8px;
         right: 15px;
         width: 15px;
         height: 15px;
         border-right: 1px solid #000;
         border-bottom: 1px solid #000;
         transform: rotate(45deg);
         transition: all 0.5s; // 旋转向上的过渡时间
     }
   // 鼠标经过让旋转盒子再倒旋转向上
 div:hover::after{
         transform: rotate(225deg);
         margin-top: 7px;
     }
```

### 设置2D转换的中心点 transform-origin

```css
transform /默认以自身中心点为延展点,
transform-origin:x y; //修改中心点(谁作用 就给谁设置) 默认50%
/ 可选方位如:left right bottom top center或者百分比


```

### 缩放scale

```css
transform:scale(x,y)   1相当于本身宽高 2 放大2倍 0.1-0.9即缩小
scale 也可以设置转换中心点进行缩放 默认以中心点延展,且不影响其他元素
额外知识点: cursor：pointer;  光标样式   小手
```

### **2D转换注意点**

1. 复合写法

   ```css
   transfoem:translate() rotate() scale(); 属性间需要有空格,按照顺序 先位移 后旋转 再缩放
   ```

2. 位移 缩放不影响其他元素盒子,设置转换中心点的参数可以是百分比像素或者方位名词

3. animation 3D动画专用,其他属性基本都是通用的如过渡 变形和轴

## 动画(animation)

动画相比较过渡,可以实现更多的变化 控制 自动播放等

1. 基本使用：1 定义   2.使用

   ```css
   定义动画帧:
   @keyframes move{ // moveji即动画名 哪里使用哪里调
       0%{  //起始状态时 使用form也行
           transform:translateX(0px)   }
       100%{ //结束状态时 使用to也行
           transform:translateX(100px) }
   调用：
   复合写法
   animation:动画名 持续时间 运动曲线(linear/ease) 触发时间 播放次数 是否反方向   动画开始或者结束时状态；
     forwards 停止在动画结束时位置 
     alternate 动画结束后回到起始状态
     infinite 播放无限次
     steps(步长数值) 配合大小/宽度, 与运动曲线冲突只能2选一
    animation-play-state:running/pause; 鼠标悬停时是否   暂停或播放此属性不能进行复合写法 只能单独书写
   ```

2. 定义动画帧时使用的如果是精灵图背景就用background-position:X,Y;

3. 平移使用ease/linear,  动画帧(精灵图背景)使用steps 步长，

   ```css
   div {
    position: absolute;
    width: 200px;
    height: 100px;
    background: url(./Upind/bear.png) no-repeat;
    多个动画名用逗号隔开即可
    animation: w 1s steps(8) infinite, c 1s forwards;
     }
   @keyframes w{
       0%{background-position: 0 0;}
       100%{background-position: -1600px 0;}
   }
   @keyframes c{
       0%{left:0;}
       100%{left:50%}
       transform:translate(-50%，0); /走自身50%实现X轴     居中
   }
   ```

## 3D动画

### 3D变形 (transform)

特点:视距-----近大远小   物体后面遮挡不可见

三维坐标

1. x轴:  水平向右    左负 右正
2. y轴:  垂直向下    上负 下正
3. z轴:  垂直屏幕     外正 里负

简写: transform:translate3d(x,y,z)==不能省略,必须有值/0都可以==。

### 3D透视(perspective)

想要实现3D效果必须借助于 perspective 视距属性,单位必须是像素PX,且必须书写在其父元素身上。

```css
div{
    perspective:500px;
}
li{
  width:200px; height:200px;
  transform:translateX() rotateX/Y/Z/3d(45deg,45deg,)
}
3D的rotateX 即沿着X轴上下旋转  rotateY 即沿着Y轴左右旋转
rotateZ 即视距方向顺时针旋转
```

1. ==3D的旋转属性：rotateX： 即沿着X轴上下旋转  rotateY 即沿着Y轴左右旋转rotateZ 即视距方向顺时针旋转==
2. transform-style:preserve-3d;开启3D环境,默认关闭,==书写在父元素身上==用于多个子盒子间作协调效果
3. transfoem:translate() rotate() scale(); 属性必须  先位移 后旋转 再缩放,动画效果也可以先旋转

# 浏览器私有前缀

1. -moz- : 代表firefox 火狐浏览器

2. -ms- ：代表ie浏览器

3. -webkit- : 代表Safari、chrome 

4. -o-  :        代表Opera  浏览器

   ```css
   -webkit-border-radius:10px;
   border-radius:10px;
   这样书写代表兼容性
   ```





# 媒体查询

- 用于对应可视窗口的变化去展示对应的css样式
- 也可配合动画使用

```js
// 当设备高度小于或等于800px时 p标签的字体变为10px
@media(max-height: 800px) {
    p{
        font-size: 10px;
    }
}
<p>
    Hello World
    </p>
```





















 



