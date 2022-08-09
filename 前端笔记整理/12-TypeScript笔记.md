## 安装TS解析包

```js
npm install typescript -g // 安装到全局环境
```

- tsc init    初始化一个TS的JSON配置文件

- 创建 src目录-创建index.ts 后缀的文件

- 在node终端输入 tsc ./src/index.ts 运行该ts文件，会自动保存一份解析后的js同名文件

- 练习阶段：借助webpack

- 简化TS运行：下载ts-node 插件 自动更新运行内容，不需要再tsc day01.js转换成js，省略了1步

  ```js
  npm i -g ts-node // 安装自动更新插件包
  // 使用 内部自动隐式创建转换成js
  ts-node day01.ts ： 在终端运行TS
  // 只要书写过一次该命令 之后使用上下键 即可自动带出最近使用的命令
  // cls 可以清除终端内容，类似 clear
  ```



### TS文件转JS文件

注释：浏览器无法直接运行ts文件 解决：

1. 使用命令 tsc 将ts文件转化为js文件，语法： tsc index.ts  自动生成一个对应的js文件，与less预处理器类似
2. 然后在页面中引入转化后的js文件即可
3. 自动刷新转换：tsc --watch index.ts     监视index.ts文件的变化自动刷新并转换成js

### 错误处理

```js
Do you need to change your target library? Try changing the 'lib' compiler option to include 'dom'
// 

tsc -init // 生成tsconfig.json 编译配置文件
"target": "es5", // 改成es6
outDir: './' ts转js后的输出目录
script: true/false  开启或关闭严格模式
```





### TS之VSCode调试配置

1. 准备ts文件

2. 添加调试配置， 注：打开左侧的调式运行图标 -新建配置文件-替换文件内容如下

   ```js
   {
     // 使用 IntelliSense 了解相关属性。 
     // 悬停以查看现有属性的描述。
     // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
     "version": "0.2.0",
     "configurations": [
       {
         "type": "node",
         "request": "launch",
         "name": "调试TS代码",
         // ts-node 命令： “直接”运行ts代码。
         // 作用：调试时加载ts-node包（在调试时“直接”运行ts代码）
         "runtimeArgs": ["-r", "ts-node/register"],
         // 此处的 a.ts 表示要调试的 TS 文件（ 可修改为其他要调试的ts文件 ）
         "args": ["${workspaceFolder}/a.ts"]
       }
     ]
   }
   ```

   

3. 安装调式用到的包；注：要求在当前的ts文件运行终端安装 npm i ts-node typescript

4. 鼠标点击文件代码行红点，点击调式图标选择调式TS代码-->点击绿色三角按钮开始调式就可以了，跟浏览器控制台的调式一样



### 配置出口文件

1. 





## 注释

注释：与JS相同 

- 单行：//, 多行 /*  */,



## 变量与类型注解

注释：注解-> 规定变量的类型 该类型就是注解，正常情况下 注解类型声明后无法更改

```js
// 直接初始化并赋值
let age: number = 15; // number 就是注解 数字类型
let str: string = 'xjj'
let tro: boolean = true
let deo: undefined = nundefined
let null: null = null;
console.log(2 -  +'1') // 1， 字符前面+，隐式转换成number类型（前提是字符串内容为数值）
```



### 变量命名规则

注释：与JS相同 只能出现数字 字母 下划线_ 美元$，且不能以数字作为开头，区分大小写

### 数据类型

1. 基本数据类型：Number, String, Boolean, null, undefined
2. 复杂数据类型：Function, Object,Array
   1. 数值类型与JS相同如：‘ ’，单双引号表示空字符串，
   2. undefined 为未定义， null 为空值
3. any: 泛型，任意类型



### 联合类型

注释：类型注解的多选一，称为联合类型

```js
let a:number | string = 10 // 即a便利的类型可以是数值也可以是字符
    a:'xjj'
console.log(a.slice(0,2)) // xj 这里不会报错 因为a已经确定了类型

// 表示这个函数的参数可以是数字/字符， 同时函数的返回值也可以是数值/字符
function fn(name: number | string):number | string{
    return name.split(); // 报错：联合类型不能调用某个类型的专有属性,split属于字符类型专有
}
```



注意点：==联合类型的公有属性如toString是没有问题的，但是不能使用某个类型的专有属性方法==



### 运算符&语句

注释：算数 + - * /， 赋值 = ， 递增/递减 ++a/a++,  比较 == ,逻辑 && || ！取反， 与JS相同，略过

注释：语句、运算符与JS相同，略过， 三元表达式 有返回值



### 循环

注释：循环内的变量可以不声明类型，系统会自动识别，也就是类型推断

```js
for (let i: number = 0; i < 3; i++) {
  console.log(i); // 0 1 2
}
```



### 泛型

1. 注释：any : 泛型  ->即任意类型
2. 推荐使用 unknown 意为:未知类型，--一旦后续定义了某种类型则不可以再将其赋值给其他变量类型

```js
function index(name:any):any{ // 允许任意类型
  return name;
}
console.log(index(2)); // 可以传入任意类型

----分割线--
class Person {
    id?:number // id可有可无
    name:string 
} 
-----------分割线-------------
 function index(name:any):Array<any>{ // 允许数组的值为任意类型
    
}
```



### 接口注释：

1. 接口可以约束一致，也可以时联合/泛型，单一个接口只允许有一个任意属性
2. 如果有多个任意类型，可以使用联合类型

```js
interface Person {
    name:string;
    age?:number; // 报错 必须与任意属性保持一致 
    [index:string]:string // 任意属性为string
}
// 改进
interface Person{
    name:string;
    age?:number; // 不报错，因为任意属性使用了联合类型允许有number类型
    [index:string]:string | number
}
```



### 只读属性readonly

1. 第一次给对象赋值时有效，后续无法更改其属性

```js
interface Person{
    name:string;
    readonly age:number; // 设置age为只读属性
}
let Son:Person = {
    name:'xjj',
    age:18  // 第一次设置完毕后续无法更改器属性类型
}
Son.age = 20; // 报错，age为只读属性
console.log(Son.age)
```





## 数组

注释：数组操作方法如长度 length 存取等与JS相同

```js
let str:string[] = ['xjj','hello'] //  先声明数组类型 -- 字符串数组且类型后面跟上[]号再赋值
let str:number[] = [1,2,3,4,5]  // 数组类型 -- 数字型
str.push('hoo') // ['xjj','hello','hoo]

// 不推荐这种创建方式
let arr:string[] = new Array('xjj','hello')
```



### 多维数组的访问

```js
let winsArr = [
    [0,1,2],
    [3,4,5]
]
console.log(winsArr[0][1]); // 1 即winsArr 数组元素第0个[0,1,2]中的索引1 -> 即数值1
```



### 数组接口

注释：接口用于数组：需约束数组类型，也可使用联合类型

```js
interface Person {
  [index: number]: string; // 接口成员为字符串数组
}
let list:Person = ['1','2']
console.log(list); // ['1','2']

----分割线----
interface Person {
  [index: number]: number | string; // 接口成员为联合类型 数值/字符都可
}
let son:Person = [1,2,3,'4','5']
console.log(son); // [1,2,3,'4','5']
console.log(list[2] // 3
```



### 类数组

注释：==严格模式下 类数组的使用无效==

```js
interface Args{
    [index:number]: any;
    length: number;
    callee:Function;
}
function test(){
    let args: Args: arguments;
}
test()
```



### 元组

注释：元组类型：允许表示一个已知元素个数和类型的数组，可以包含任意元素，但声明和赋值必须保持一致

```js
let arr:[number,string] = [1,'2'] // 确定数组类型以及元素类型
console.log(arr[0].split) // 报错 number元素1 没有split方法
console.log(arr[1].split) // 成功 string有split方法
arr.push(3); // 允许
arr.push('xjj'); // 允许
```



### 数组定义

```js
let finded:string[] = ['xjj','hello']; // 数组必须时字符类型
finded.push(3); // 报错
```





## 函数

注释：实参与形参的类型注解 必须一致，==且实参与形参的数目和顺序必须--对应==

```js
// 形参类似声明变量指定类型注解 系统会为num进行赋值操作
function getSum(num: number[]): number { // number 指定返回值的类型 即只能return数值类型
  let sum: number = 0;
  for (let i: number = 0; i < num.length; i++) {
    sum += num[i];
  }
  return sum; // 如果函数指定了返回类型 这里就只能返回数值型number
}

let arr: number[] = [1, 2, 3, 4, 5];
getSum(arr) // 15
```

参数可以是多个且可以是不同的类型 ，也可以是数组形式

```js
function sing(songName: string,songBtn:number) {
  console.log(songName) // xjj
  console.log(songBtn) // 18
  console.log(typeof songName); // string
  
}
sing('xjj',18)

------分割线-----
function sing(songName: string[]) {
  console.log(songName); // [ '2002年的第一场雪', '18' ]
  console.log(songName.join('')); // 2002年的第一场雪18
}

sing(["2002年的第一场雪", "18"]); 
```

总结：2个点： 函数参数的注解和返回值的注解



### 全局函数

注释：引用第三方库的适合可以使用全局函数注解,即不关心什么数据 拿来调用即可

```js
// 关键字 declare
declare function create(name: object | null): void{
    
}
```



### object类型

注释; 定义一个函数 参数可以指定类型和返回类型

```js
function getObj(obj:object):object{ // 参数与返回值都为object
    console.log(obj);
    // 返回修改后的值
    return{
      name:'卡卡西',
      age:27
    }
}
console.log(getObj({name:'hello', age:22})); // 参数必须是对象形式
// { name: 'hello', age: 22 }
// { name: '卡卡西', age: 27 }
```



### 函数断言

注释：能用断言的都可以用，不止是元素和函数元素

```js
// as作为比较条件时需要加小括号 其它时候不需要加小括号
function getValue(str:number | string){
  if((str as string).length){ // 断言str 为字符类型 才能使用length方法
       return (str as string).length
  }else{
      return str.toString().length
  }
}
console.log(getValue('5')); // 1
```



### 函数接口

```js
interface ISear {
  // 指定函数表达式得参数类型和返回类型  
  (name:number, age: number): boolean
}

let searGood: ISear = function(name:number, age: number):boolean{
        return  name + age > 18
}
let sum = searGood(5,6)
console.log(sum); // false
```

其他函数封装如排序等与JS相同

### 函数的剩余参数

注释：类似JS的arguments

```js
 // 剩余参数需指定类型和用[]括号接收 且必须放在函数参数顺序的最后
function fn(str: string, ...args: string[]) {
  console.log(str);
  console.log(args);
}
fn("a", "b", "c", "d", "e");
```



### 函数的可选参数

注释：可以设置可选参数，也可以设置参数默认值

```js
function goTo(name:string,age?:number)string | number{
    // 表示age这个参数可有可无，必须按照顺序书写不能写再在前面，且后面不能再跟必须参数
}
// 允许参数有默认值
function goTo(name:string = 'xjj',sex:string){
    return name + sex;
}
```





### 函数重载

注释：指 函数名相同，函数的参数与返回值不同，TS会优先匹配写在前面的重载函数

```js
// 函数重载 
// 用于处理函数内部逻辑不足引起的问题 可以使用函数重载
function add(a: string, b: string): string;
function add(a: number, b: number): number;

// 必须二选一 要么全部为number要么全部为string
function add(a: string | number, b: string | number) {
  if (typeof a === "string" && typeof b === "string") {
    return a + b;
  } else if (typeof a === "number" && typeof b === "number") {
    return a + b;
  }
}

console.log(add("10", "20"));
console.log(add(10, 20));

console.log(add(1, "2")); // 报错 与函数重载的要求不符
```



### 函数表达式

```js
// 此处对匿名函数的参数进行了类型约束，但是因为被作为函数表达式，mySum未被约束
let mySum = function(x:number,e:number):number{
    return x + e;
}

// 对函数表达式进行约束,并限制返回值也只能是number类型
// 这里的箭头与ES6的箭头函数不是一回事，这里箭头表示左边是输入类型，右边是输出类型
let mySum:(x:number,e:number) => number = function(x:number,e:number):number {
    return x + e;
}
```



### 使用接口约束函数表达式

```js
interface Person{
    (source:string,subString:string):boolean
}
let mySend:Person = function(source:string,subString:string){
    return source.search(subString) !== -1;
}
```









## 对象

注释：与JS相同，区别是对象的使用需提前进行类型注解进行约束，简称：对象接口

### 对象接口

注释：为对象结构 进行类型注解,  接口也可继承接口实现复用

```js
// 为对象person 规定类型注解  
let person: {
  name: string; // name属性只能为字符
  age: number;  // age只能是数值
  // 给对象的方法进行类型注解 即:约束类型
  sayHi: () => void;             // sayHi方法没有参数 且没有返回值
  sing: (sex: string) => void;   // sing方法有一个参数为字符类型，同样没有返回值
  grile: (grend: string) => string;  // grile方法有一个参数， 且有返回值 类型为字符型
};

// 规定好对象接口后 即可为对象添加值了 属性与方法的类型必须对应接口的类型注解
person = {
  name: "xjj",
  age: 18,
  sayHi: function () {
    console.log(this); // this指向的就是当前person对象的接口映射
    console.log(this.age) // 18
    console.log("我没有参数也没有返回值");
  },
  sing: function (sex: string) {
    sex = sex;
    console.log("我有一个参数，但没有返回值" + sex);
  },
  grile: function (grend: string) {
    return (grend = grend);
  },
};

person.sayHi() // 我没有参数也没有返回值
person.sing('刘德华') // 我有一个参数，但没有返回值刘德华
let sum = person.grile("xjj")
console.log(sum); // xjj
```

对象方法的2个点：1 参数  2：返回值，  用鼠标悬停在变量数编辑器会提示类型



### 接口优化

注释：复用类型注解 简化代码，使用interface关键字声明，

```js
// 接口关键字interface 后面跟上接口名 使用该接口的对象必须与接口的类型约束保持一致
interface IUser {
    name: string; // 对象属性的类型注解
    age: number;   
    sayHi: () => void;  // 对象方法的类型注解
}
// 使用 ：将接口名放入对象名后面 即：为对象sing进行类型注解
let sing: IUser = {
    name: 'xjj',
    age: 18,
    sayHi:function(){
        console.log('没有参数也没有返回值')
    }
}
console.log(sing.name); // xjj
sing.sayHi() // 没有参数也没有返回值
```



### 接口拓展

注释; 允许应用接口的对象拓展接口中没有规定的属性 可以是任意类型

```js
interface Person{
    name:string
    age:18,
    [propName:string]: any // 允许应用对象拓展接口属性
} 
let Person = {
  name: "xjj",
  age: 27,
  sex:[1,2]
};
console.log(Person.sex) // [1,2]
```



总结; ==接口也可用于函数 数组==



### 对象存取

注释：可以通过点语法修改对象属性的值， 但是必须与之前的类型注解保持一致, 修改方法同理

```js
interface Person{
   name:'string
readonly age:18  // readonly 表示只读模式
}
let Son = {
    name:'xjj'
}
Son.age = 22 // 报错 无法将22分配给只读属性 已设置readonly 只读模式
Son.name = 'hello' 
console.log(Son.name) // hello
```



### 存取器

注释：控制对 对象成员的访问，get 取 和 set 存，

```js
class Person {
  firstName: string;
  lastName: string;
  constructor(firstName: string, lastName: string) {
    this.firstName = firstName;
    this.lastName = lastName;
  }
  // 获取属性成员- 返回数组形式
  get fullName() { // 读取成员属性
    return this.firstName + "--" + this.lastName;
  }
  set fullName(value) { // 设置成员属性
    let names = value.split(":");
    // 设置属性成员也是以数组形式访问
    this.firstName = names[0];
    this.lastName = names[1];
  }
}

const person: Person = new Person("东方", "不败");
console.log(person); // Person { firstName: '东方', lastName: '不败' }
console.log(person.fullName); // 东方--不败
person.fullName = "令狐冲"; // 如果没有set 将报错 为只读模式
person.lastName = "与师妹"; // 如果没有set 将报错 为只读模式
console.log(person.fullName); // 令狐冲--与师妹
```



### 静态成员

注释: 在类中通过 static 修饰的属性或者方法 就叫静态成员, 静态成员只能通过点语法来调用和设置

```js
class Person {
 static names: string;
 static constructor(){}// 报错  静态修饰符不能设置在构造函数上
 static sayHi() { // 不加静态修饰符 外部无法通过点语法调用
    console.log("hello");
  }
}
Person.names = 'xjj'
console.log(Person.names); // xjj
Person sayHi()  // hello 
```



### 抽象类

注释：当静态修饰符成员过多时可以使用抽象类。关键字  abstract

```js
// 父类利用abstract 关键字 定义抽象类 和抽象方法 留给子类去实现这个方法或属性的具体实现
// 如果在父类中取实现它或者外部通过父类去实例化抽象类 会报错 必须让子类去实现它
abstract class Animal {
  abstract name: string;
  // eat方法在抽象类里面不能实现只能定义 花括号也不能写
  abstract eat();
 // 非抽象方法
  sayHi() {
    console.log("hello");
  }
}

class Dog extends Animal {
  name: string = "小甜甜";
  // 实现父类 上的抽象方法eat
  eat() {
    console.log("红烧鸡翅");
  }
}

const dog: Dog = new Dog();
dog.eat(); // 红烧鸡翅
dog.sayHi(); // hello
console.log(dog.name); // 小甜甜
```





### class类的注解

注释：注解类当中的成员类型，如果类当中有函数，那么函数也需要有函数对应的注解

```js
class Gerrter {
    greeting: string;
constructor(message: string){
    this.greeting = message;
}
// 这是一个内置方法函数 没有参数也没有指定返回类型
greet(){
    return 'hello' + this.greeting;
}
}
let greeter = new Gerrter('world')
```



### class类的修饰符

注释：public 公用方法 class类中默认public， private : 私有属性。无法被子类继承

```js
class Animal{
    public name:string; // 公用属性
    publice constructor(isName:string){ // 公用方法
     this.name = isName;
  }
 private move(disName: number = 0){ // 私有方法
    console.log('hello')
 } }
 
 class Son extends Animal{
     constructor(name:string){
         super (name);
     }
     // 报错 move 为父类私有属性 无法访问
     move(disName: 5){
         super.move(disName)
     }
 }
```



### readonly修饰符

注释：用来修饰属性为只读模式  语法： readonly 属性: 类型注解，可用于对象，接口 枚举

```js
class Oto{
   readonly name: string // 只读模式
}
```



修饰符的总结：

1. public: 公共的成员属性；自身 子类  实例 都可调用访问
2. private: 私有 只有自身可调用
3. protected: 自身与子类都可调用
4. readonly：只读模式
5. static:  静态成员



## 类类型

注释：class类得类型 叫类类型，类也可以实现多个接口 以逗号分割即可

```js
interface Meid {
  name();
  age: number;
}
// Person类实现了 接口约束Meid
class Person implements Meid {
  name() {
    console.log("hello");
  }
  age: 15;
}
let sum = new Person();
sum.name();
```



### 类的继承

注释：继承叫子类页脚派生类 this指向的是父类，被继承的类叫父类也叫基类

```js
class Perth {
  // 在内部定义属性类型 也可以抽离出去用接口定义
  name: string;
  age: number;
  gender: string;
 // 定义构造函数
  constructor(name: string, age: number, gender: string) {
    this.name = name;
    this.age = age;
    this.gender = gender;
  }
  sayHi(str: string) {
    console.log(`我是${this.name},年龄: ${this.age} -名字叫: ${str}`);
  }
}

let geo = new Perth("父类", 55, "男");
console.log(geo.name); // 父类
geo.sayHi("父xjj");  // 我是父类,年龄: 55 -名字叫: 父xjj

----分割线---
 // 继承父类 TS中只要继承了父类就叫派生类 必须设定super
class Son extends Perth {
  constructor(name: string, age: number, gender: string,goo:number) {
     console.log(goo) // 105 这是私有属性 不能使用this
    // 继承父类属性成员
    super(name, age, gender);
  }
  // 继承父类方法
  sayHo(Gog: number) {
    super.sayHi("子xjj");
    console.log("我是子类方法中的私有参数" + Gog);
  }
}

let sum = new Son("子类", 18, "男", 105);
console.log(sum.name); // 子类
sum.sayHo(999); // 我是子类,年龄: 18 -名字叫: 子xjj
```



### 类的多态性

注释：父类型的引用指向了子类的对象，不同类型的对象针对相同的方法产生了不同的行为

```js
// 父类
class Perth {
    name:string
    constructor(name:string){
        this.name = name;
        say(){}
    }
}
class Son extends Perth{
    constructor(name:string){
        super(name)
    }
    goo(age:number){
        console.log(age)
    }
}

function show(ani: Perth){ // 该函数使用了父类的类型
    ani.say() // 只要父类里面有这个方法就可以
}

show(son1) // 向父类里面传入子类方法 这也叫多态

// 子类实例
let son1:Perth = new Perth('xjj')

// 父类实例引用子类方法 叫多态
let Perth2: Son = new Son('15')
```







## TS类型推断

注释：某些没有明确指定类型的地方，类型推断系统会帮助实现类型的推断

```js
let age = 39;
console.log(typeof age); // number
age = '35' // 报错：age已被推断为number类型  值为:38

---分割线--
// 如果声明变量不赋值，那就先给变量一个类型注解，否则就为any任意类型，不受类型约束
let name:string
    name = 'xjj'
```



### 类型别名

```js
// 用于给类型取别名 利用type关键字
type Name = string; // 赋值完成后Name等同string,
```



### 字面量类型

```js
// 同样利用type关键字
type EventName = 'click' | 'scroll' | 'mousemove'; // 类似三选一
```





## TS操作css&DOM

- 与JS相同 区别在于要进行上面的转换 ，然后在ts文件内写JS代码，与在html页面用script标签写js码一样

    ```js
  ---HTML页面---
  <h1 id="title"> 青花瓷</h1>
  
  --ts文件--
  let title = document.querySelector('#title') // 获取元素
  title.innerHTML = '我改了青花瓷哈哈'  // 更改元素
  ```





### 类型断言

获取元素时系统是通过选择器返回的元素统一推断为宽泛的Element 类型，原型只有一些公用属性，而没有具体元素的专有属性，如图片 img 被推断为Element  那么就没有它本身应该有的src 属性，导致无法设置他的src

- 解决：使用 as 关键字  进行类型断言
- 如果不清楚获取的DOM元素应该断言什么类型，可以使用 console.dir(元素)   查看DOM元素的原始形态以及Prototype属性
- Prototype: 原始形态
- as 后面也可以直接输入HTML跟上DOM名如div 系统会自动提示对应的原始形态名 
- 断言可以继承和赋值，前提是A的类型能够兼容B的类型，或者B的类型能够兼容A的类型
- 注意：断言不能转换元素/变量的类型, 只是方便获取元素的原型属性，若要转换需使用JS的转换方法

```js
// 使用 as 关键字 
// 表示 确定 image 这个元素 为图片类型 HTMLImageElement
let img =document.querySelector('#myImg') as HTMLImageElement;
let btn = document.querySelector("#btn"); as HTMLButtonElement

----分割线--
console.dir(img); // img#myImg 展开查看原型 --> [[Prototype]]: HTMLImageElement
console.dir(btn); // button#btn 展开查看原型 -> [[Prototype]]: HTMLButtonElement
```

总结：断言  ==所有元素的类型 前面统一为大写 HTML 中间是DOM名首字母大写 后面也是统一的 Element==

```js
// 获取所有p
let list = document.querySelectorAll('p') 
// 循环遍历 p 的 值和索引
list.forEach(function(item,index){
// 多个p元素需要使用类型断言 给p指定具体类型
   let p = item as HTMLParagraphElement
  // 采用字符拼接 更改p的innerText
   p.innerText = '[' + index + ']' + 'xjj'
})
```



### 多元素的类型断言

总结：如果是单个元素在获取时即可as断言其类型, 多个元素则需要使用循环再as断言， 如下例：

```js
let cells = document.querySelectorAll(".cell");

// 循环所有元素
cells.forEach(function (item) {
  let cell = item as HTMLDivElement; // 断言每一项元素类型
  cell.addEventListener("click", hand, { once: true }); // 只触发一次
});

// 抽离事件处理代码进行封装 哪里需要哪里调用即可
function hand(event: MouseEvent) { // 指定事件类型 即属性点击事件
  let target = event.target as HTMLDivElement;
  target.classList.add("o");
}
// 多元素不断言直接item绑定事件 也是可以的 只是断言会增加提示和约束性
```



### 类型声明

1. 与断言功能相似，只是可读性更高

```js
function getDo(key:string):any{
    return (window as any).cache[key]
}
interface Person{
    name:string;
    fn(): void;
}
// 直接类型声明 比在getDo('xjj') as Person可读性高
const tom:Person = getDo('xjj');
tom.run();
```



### style操作

语法：行内：dom.style.样式名， js操作：dom.style.样式名 = 样式值/class样式值

```js
// 获取元素并断言类型
let list = document.querySelector('p') as HTMLParagraphElement
// list.style.fontSize = '50px'
// list.style.color = 'red'
// 利用函数封装
function fn1(a){
  a.style.color = 'red'
  a.style.fontSize = '24px'
}
fn1(list)
```

### classList类操作

语法：dom.classList.add(类名1，类名2)

```js
// html/css页面 
<style>
.a{ font-size: 16px;
    color: red;}
.b{ border: 2px solid skyblue;
    width: 132px;
    height: 30px;}
 </style>

---ts文件---
let list = document.querySelector('p') as HTMLParagraphElement
// 同时添加了a 类名和 b 类名的样式
list.classList.add('a','b') 
// 移除类样式 a
list.classList.remove('a')
```

类名判断：语法： dom.classList.contains('类名')； 返回布尔值 包含true 否则false

```js
let list = document.querySelector('p') as HTMLParagraphElement
list.classList.add('a','b')
// 判断 list是否包含(存在) 类名a
let c = list.classList.contains('a')
console.log(c); // true
```



### 事件监听

语法：添加事件--> dom.addEventListener('要绑定的事件', 回调函数);    移除事件 --> dom.

```js
let list = document.querySelector('p') as HTMLParagraphElement
list.addEventListener('mouseenter',function(){
    alert('触发鼠标经过事件')
})
```



### 移除事件

语法： dom.removeEventListener('事件名',回调函数)，回调函数可以再绑定事件时设置也可以调用外部函数

```js
// html页面
<button class="a">点我添加事件绑定</button>
<button class="b">点我移除事件</button>

---ts--
let btn1 = document.querySelector(".a") as HTMLButtonElement;
let btn2 = document.querySelector(".b") as HTMLButtonElement;

// 1：抽离函数逻辑代码
function handleClick() {
  let sum: number = 10;
  let sun: number = 2;
  console.log("添加事件" + (sun + sum));
}
// 2：给btn1 添加点击事件 并调用函数
btn1.addEventListener("click", handleClick);

// 3：给解绑按钮btn2 绑定点击事件 利用回调函数 解绑btn1的点击事件
btn2.addEventListener("click", function () {
  btn1.removeEventListener("click", handleClick);
});
```

需要保证绑定和解绑事件使用的是同一个函数逻辑，

### 自动移除事件

注释：利用once关键字   语法：{once: true}  

```js
// 2：添加点击事件 并调用函数 同时传入第三个参数 设定该事件只触发一次，然后自动移除事件
btn1.addEventListener("click", handleClick, {once: true});
```



### 事件对象

注释：触发事件的相关属性信息，

```js
// 绑定的时click点击事件 event 就是该事件的相关信息
list.addEventListener('click',function(event){
    console.log(event.type) // click 绑定的时click点击事件
    console.log(event.target) // p 标签元素
    console.log(event)   // 打印event对象 的所有属性，要什么在这里查看即可
    // 这里可以组织事件的冒泡机制
})
```



### 事件类型注解

```js
let btn1 = document.querySelector(".a") as HTMLButtonElement;

// 指定函数的类型 为点击事件 点击事件属于鼠标对象 MouseEvent
function delo(event: MouseEvent){
       console.log(event.target);
    // console.log(11); 
}
// 给函数加了事件注解 使用时就会有相关提示 不加就没有提示
btn1.addEventListener("click", delo, { once: true });
```



## 枚举

注释：当变量的值，只能是几个固定值中的一个，应该用枚举来实现,  ==使用关键字 enum 声明枚举==

```js
enum user {成员1,成员2} // 与接口类似 ，区别在于：枚举成员以大写字母开头，不采用键值对形式，
// 1：创建了一个枚举 枚举名叫Gender 里面有2个成员
enum Gender {
    Female,
    Mele
}
// 2：创建变量 使用枚举作为类型注解 --同时将枚举成员Female 赋值给变量
let userGender: Gender = Gender.Female 
```

注意：枚举成员为固定的，不能使用枚举中不存在的成员，且为只读模式 无法更改其值

### 数字枚举

注释：==数字枚举成员具有默认自增行为==

```js
// 默认从0自增(索引)
enum Gender {
    Female,
    Mele
}
// 声明一个变量,将枚举Gender的成员Female赋值给它
let userGender: Gender = Gender.Female 
console.log(userGender) // 0  枚举成员的索引从0开始 可手动更改
console.log(Gender.Mele) // 1
console.log(Gender[0])  // Female 数组枚举可以通过索引来访问成员值

总结：数字枚举 访问索引用点语法，访问成员值使用索引
```



```js
// 手动更改了枚举成员的索引
// 索引必须与值对应,该枚举中没有索引2，只有1，100和101
enum Gender {
    Female = 1,
    Mele = 100,
    Toik
}
console.log(Gender.Mele) // 100
console.log(Gender[2])   // undefined 
console.log(Gender[100]) // Mele
console.log(Gender.Toik) // 101 成员索引默认为从0开始，参照上一个成员自增
console.log(Gender) // 打印结果如下分别为枚举数字和枚举字符
// 1:'Female' 100:'Mele'  101:'Toik'
// Female: 1   Mele: 100  Toik: 101
```





### 字符枚举

注释：==字符串枚举成员是没有自增行为的==，所以必须在初始化时就赋值

```js
enum Femle {
    Good = '您好',
    Sex = '男',
    A = 18, // 此处系统会认为18是成员 A时字符，正确方式应该将18 设为字符‘18’
}

console.log(typeof Femle.A); // number
console.log(Femle.Good); // 您好
console.log(Femle);  // 18: 'A'  Good:'您好'  Sex:'男'
console.log(Femle[0]); // 报错 undefined 字符枚举没有索引自增行为
```



### 常熟枚举

注释：与其他枚举相同，区别在于不能包含计算成员

```js
const enum Person{
    name,
    age,
    left = 'blue.lenght' // 报错 不能包含计算成员
}
let dire = [dire.name,dire.age]
```



总结：字符枚举与数字枚举不同，直接使用点语法就可以访问枚举成员值，另外：不管是数字还是字符枚举都不具备length等属性，

小案例：

```js
enum Player {
  X = "x", // 获取class样式类名为x 赋值给枚举成员X
  O = "o", // 获取class样式类名为o 赋值给枚举成员O
}
  // 获取元素
let cells = document.querySelectorAll(".cell");
let gameBord = document.querySelector("#bord");

let current:Player = Player.X; // 默认显示X样式
// 遍历所有元素 绑定事件处理函数
cells.forEach(function (item) {
  let cell = item as HTMLDivElement;
  cell.addEventListener("click", hand, { once: true });
});
 // 抽离封装一段事件处理函数作为回调函数
function hand(event) {
  let target = event.target as HTMLDivElement;
  target.classList.add(current); // 点击触发样式更改
  // 然后判断当前样式 决定下一次点击触发另一个样式
  current = current === Player.X ? Player.O : Player.X;
  gameBord.classList.remove(Player.X, Player.O); // 清除背景样式
  gameBord.classList.add(current); // 联动添加背景样式
}
```



## 声明文件&语句

### 声明语句

注释：ts无法识别引入的第三方库，需要声明

```js
// 声明语句利用关键字 declare 即全局声明，也可以用来声明全局函数/变量
declare var jQuery:(selector:string):any;
jQuery('#foo'); // 声明语句后才能使用jq的方法
```



### 声明文件

注释：一般将声明语句放入一个单独文件，该文件以 .d.ts为后缀，就是声明文件，才会被ts解析

1. 假如仍然无法解析，那么可以检查下 `tsconfig.json` 中的 `files`、`include` 和 `exclude` 配置，确保其包含了 `jQuery.d.ts` 文件。
2. 以JQ为例: 利用@types统一管理第三方库
   1. npm install @types/jquery --save-dev 下载
3. 具体参考文档
4. 

### 内置对象

注释：与js相同

1. DOM与BOM内置对象，
   1. Docment, HTMLElement, event, NodeList
2. node.js不是内置对象得到一部分，在node使用ts需要下载
   1. npm install @types/node --save-dev



## 总结

1. 变量：有类型注解：number, string,  boolean, 
2. 函数：有参数类型注解，返回值注解
3. 对象：有接口关键字 interface
4. 标签元素：有类型断言 关键字 as
5. any:  泛型，即任意类型
6. object: 表示非原始类型
7. [proName:string] : string/any  允许应用对象拓展属性



几个常用词

- void: 表示函数无返回值
- ()  : 表示函数无参数
- {once: true} : 表示事件监听只触发一次
- readonly: 只读

TS主要用于约束类型 方便后期维护，以及编码提示

如果不知道要处理的数据是什么类型可以用any，不过尽量不要用any 否则就失去TS的意义

所以在书写TS时尽量按照规定进行类型注解等，才会有相应的提示以及后期的维护便利































