
## Typescript前置概览
```markdown
【数据类型】
- 原始数据类型：number/string/boolean/undefined/null/symbol/bigint
- 复合类型(object)：object

【类】
- 类：定义了一切事物的抽象特点
- 对象：类的实例
- 面向对象oop三大特性：封装、继承、多态
- 三种修饰符public/private/protected

【泛型】generics


> type与interface区别：
- type：交叉或组合类型的时候使用
- interface：继承(extends)或者实现(implements)的时候使用
```


## 什么是ts,为什么学习及安装

### 1. 什么是ts
```markdown
- ts是js的超级；
- js是动态类型系统,不用编辑可直接执行;ts是静态类型风格及类型系统，需编译成js之后执行；
- ts提供从es6及以上版本的语法支持，js使用的话需要配置babel
- 兼容各种浏览器，服务器，各种系统，完全开源
```

### 2. 为什么学习ts

```markdown
- 程序更容易理解 (参数的提示)
- 效率更高 （代码补全，跳转代码块），增强编辑器和IDE的功能
- 更少的错误 （编译期间发现大部分错误，杜绝常见错误 form.a.b场景）
- 完全兼容js,es新语法
 *缺点*：
- 学习成本
- 短期开发成本大，适合需要长期维护的大型项目 
```

### 3. 安装
- `npm instal -g typescript`
- `tsc -v` //typescript complier //ts编译器(编辑器：vscode) 
- `tsc test.ts`  //此种格式表示现文件夹下存在的ts文件



## 原始数据类型 和 any类型
1. 原始数据类型
```javascript
let message:string = "mess";
let u:undefined = undefined;
let n:null = null;
let num:number = undefined
```

2. any类型
```javascript
let notsure:any =4;
notsure="maybe a string"
notsure.myname
``` 

## 数组 和 元组
```javascript
let arr:number =[1,2,3];  //数组的值只能是number
let arr1:[string,number]=["aa",11]  //数组的值可能是string或者number
``` 


## interface接口（object对象）
```markdown
- 描述对象的shape；描述函数的类型
- 对对象的形状（shape）进行描述，
- interface是不存在于js中的概念，所以ts编译之后，interface是不会被转换过去的，只能用作类型的静态检查
```

```javascript
// interface-basic.ts

//对对象的约束
interface person{
    readonly id:number,
    name:string,
    age?:number //?表示参数可传可不传
}
let myobj:person={
    id:123,
    name:"123"
}
// myobj.id=2;//id为readonly
```

## 函数
```markdown
- 函数是一等公民
- 输入输出类型定义
- 函数声明 与 函数表达式
* ts中，凡是在:后面都是在声明类型，与实际的代码逻辑无关系
```

```javascript
// function-types.ts

const add=(x:number,y:number,z?:number):number=>{
    if(z){
        return x+y+z;
    }else{
        return x+y;
    }

}
// function addd(x:number,y:number,z?:number):number{
//     return 1
// }
interface isSum{
    (x:number,y:number,z?:number):number
}

//两种写法
let add1: (x:number,y:number,z?:number)=>number=add;
let add2:isSum=add;

//传入对象解构
const addObj=({a,b}:{a:number,b:string}):string=>{
    return a+b
}
let add3=addObj({a:123,b:"22"});
console.log(add3);
```


## 类型推论 联合类型 类型断言
```markdown
1. 类型推论
- ts为我们提供的帮助 => 自动进行类型推论（当赋值一个变量string类型，会自动推论出string类型，赋值函数会自动推论没有明确声明的函数是函数类型。同理其他）
`let str = "string"`

2. 联合类型
- 允许一部分类型的情况下使用
- 此种情况只能访问联合类型里共有的属性或者方法，若需要访问某个类型里的属性或方法
`let aa:string | number = 123`

3. 类型断言
- 你比编辑器更清楚是什么类型(联合类型中的哪个)，用断言 **as**，缩小联合范围
- type guard(ts特性):遇到联合类型，缩小类型范围，与上相同功能
```

```javascript
// type-interface-more.ts

/**类型推论 联合类型 类型断言*/

//类型推论（type inference）
let str="str";

//联合类型
let stringAndNumber:string | number;

//类型断言
function getLength(input:string | number):number{
    let str= input as string  //类型断言
    if(str.length){
        return str.length
    }else{
        return str.toString().length
    }
}

//type guard
function getLength2(input:string | number):number{
    if(typeof input === "string"){
        return str.length
    }else{
        return str.toString().length
    }
}
```


## class类
```markdown
- 类(class):定义了一切事务的抽象特点
- 对象(Object):类的实例
- 面向对象(oop):封装、继承、多态

> typescript是怎样增强类的？使用访问修饰符（权限管理）
- public：修饰的属性或方法是*共有的*
- private：修饰的属性或方法是*私有的*
- protected：修饰的属性或方法是*受保护的*，即遗产继承
- readonly:只读
```

```javascript
// class.ts

let obj={
    aa:"1",
    bb:function(){
        console.log("函数😌 bbb");
    },
    cc(){
        console.log("函数🍚 ccc ");
    }
}

class ThePerson{
    protected name:string
    constructor(name){
        this.name=name
    }
    greet(){
        return `${this.name} 向你问好`
    }
}
class Student extends ThePerson{
    readonly major:string
    static country="中国"  //可直接访问的静态属性
    constructor(major,name){
        super(name)
        this.major=major;
    }
    studentGreet(){
        return `${this.major}专业的${this.name}向你打招呼！`
    }
}
let perone=new ThePerson("张三");
console.log(perone.greet());
let stuone=new Student("计算机","李四");
console.log(stuone.studentGreet());
console.log(stuone.greet());
// console.log(stuone.name);
console.log(Student.country);//静态属性，不需要实例化

```


## 类和接口 - 完美搭档
```markdown
- 继承的困境（一个类只能继承另外一个类，不同类之间有共同特性，该特性没有合适的父类，将特性提取成接口，）
- 类可以使用implements来实现接口
- 抽象类的属性和方法
```

```javascript
// class-interface.ts

/**interface实现逻辑功能的提取与验证(契约)
使用interface、implements抽象验证类的属性和方法*/
interface Radio{
    switchRadio(trigger:boolean):void
}
// interface CheckBettery{
//     bettery():void
// }
//接口也可实现继承
interface radioWithBettery extends Radio{
    bettery():void
}
class Car implements Radio{
    switchRadio(trigger:boolean){ //必须是实现radio中的方法
    }
}

// class Phone implements Radio,CheckBettery{
class Phone implements radioWithBettery{
    switchRadio(trigger:boolean){ //必须是实现radio中的方法

    }
    bettery(){

    }
}

```


## 枚举
```markdown
- 常量的情况，一个范围内的一系列常量（如：一周七天，三原色，四个方位）
- 数字枚举
- 数字枚举的反向映射
- 字符串枚举
- 常量枚举（不被编译成js代码） 
```

```javascript
// enums.ts

/**枚举 */
//枚举的值有两种类型：常量值  计算值
//常量枚举（常量值） 
const enum Direction{//常量枚举
    Up='UP',
    Down='DOWN',
    Left='LEFT',
    Right='RIGHT'
}

// console.log(Direction.Up);
// console.log(Direction[0]);
const value ='UP';
if( value ===Direction.Up){
    console.log("go up!");
}
```



## 泛型 



```markdown
### 初探 （函数）
- 定义时候不先指定类型，使用时在指定类型（如：一个函数输入什么就返回什么）
- 类似一个变量或者占位符
### 约束泛型 （函数）
- 使用 extends 关键词，对传入值进行约束 

### 泛型在类和接口中的使用 （类、接口）
- 作用于类中的方法（规定 参数与返回值的类型）；
- 作用于接口
```

```javascript
// genertics.ts
/**泛型 
 * 类似一个变量或者占位符
*/

//错误
// function echo1(a:any){
//     return a;
// }
// let aa=echo1("123");

//正确
function echo<T>(input:T):T{
    return input
}
let result = echo("123");


function swap<T,U>(one:T,two:U):[U,T]{
    return [two,one];
}
function swap1<T,U>(tuple:[T,U]):[U,T]{
    return [tuple[1],tuple[0]];
}
let result1=swap(123,"string");

//--------------约束泛型---------
function echoWithArray<T>(input:T[]):T[]{
    console.log(input.length);//拥有length属性
    return input;
}
let length1=echoWithArray([1,2,3,4]);
// let length2=echoWithArray("str");//有length，但参数类型不符合

//更优方法
interface hasLength{//要求有length属性
    length:number
}
function echoWithLength<T extends hasLength>(input:T):T{
    console.log(input.length);//约束传进来的参数要有length属性
    return input
}
let len1=echoWithLength([1,2,3]);
let len2=echoWithLength("string length");
let len3=echoWithLength({length:88,name:"123"});
// let len4=echoWithLength(123);//123没有length属性，故不符合要求

//----------泛型在类和接口中的使用-----------
//1、类
class Queue<T> {
    private data=[];
    push(val:T){
        return this.data.push(val);
    }
    pop():T{
        return this.data.shift();//array.shift删除数组第一项，改变原数组
    }
}

let queue1=new Queue<number>();
queue1.push(12.3);
// queue1.push("str");
console.log(queue1.pop().toFixed());
// queue1.pop();

//2、interface
interface KeyPairs<T,U>{
    key:T,
    value:U,
    id:T
}
let obj1:KeyPairs<string,number>={
    key:"key",
    value:123,
    id:"1"
}
let obj2:KeyPairs<number,string>={
    key:2,
    value:"123",
    id:2
}

let myArray:number[]=[1,2,4]
let myArray1:Array<number>=[1,2]

```

 
## 类型别名 字面量 交叉类型

1. 类型别名

```markdown
- 关键字 **type**
- 函数类型别名，联合类型别名
```

2. 字面量

```markdown
- 配合枚举使用 enums
```

3. 交叉类型

```markdown
>> type 与 interface的区别：interface描述《数据结构》，type描述《数据关系》
```
```javascript
interface Iname {
    name:string
}
type Iperson = Iname & {age:number}
lwt person:Iperson={
    name: "张三",
    age:18
}
``` 

```javascript
// type-alias.ts

/**类型别名 字面量 交叉类型
 * 
 */

//类型别名
let sum:(x:number,y:number)=>number
const resultSum=sum(1,2);

type aliasType= (x:number,y:number)=>number
let sum2:aliasType
const resultSum2=sum2(3,4);

type stringOrNumber=string | number;
let aa:stringOrNumber="123"
aa=123
// aa=true


//字面量(|)
let num:123=123;
type Directions='up' | 'down' | 'left' | 'right'
let towhere:Directions='down';//只能四种值

//交叉类型(&)
interface Name{
    name:string
}
type Person=Name & {age:number}
let personOne:Person={name:"123",age:123}

```


## 声明文件
```markdown
- .d.ts为声明文件
```
#### 使用第三方库时候：
1. 自行到以下网址下载声明文件
- 遇到第三方库的时候，需要下载对应的声明文件（如：npm上 @type/jquery），只有类型定义，没有具体实现
- types由definitelyTyped管理  [definitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped)
- 上述常见库搜索🔍  [definitelyTyped](https://www.typescriptlang.org/dt/search/?search=)       

2. 第三方源代码自带声明文件 
- 一次安装，双重绑定（定义文件及源代码）
- redux 


```javascript
// declaration-files.ts

/**
 * 创建声明文件，安装声明文件
 */

//redux(直接提供定义文件和源代码)
declare var jQuery:(selector:string)=>any
jQuery("#oop")
```


## 内置类型
- [内置类型 (https://www.typescriptlang.org/docs/handbook/utility-types.html)](https://www.typescriptlang.org/docs/handbook/utility-types.html)


```javascript
// build-in-types.ts

/**内置类型 */
//global Objects
let barr:Array<number>=[1,2];
let date=new Date();
date.getFullYear();
const reg=/abc/
reg.test("abc");

//build-in Object
Math.ceil(2.12);//Math是静态的,不是一个构造器


//DOM and BOM
let body=document.body;
let getlis=document.querySelectorAll("li");
getlis.keys();

document.addEventListener("click",e=>{
    e.preventDefault();
})

//utility types 附加:(https://www.typescriptlang.org/docs/handbook/utility-types.html)
interface Iperson{
    name:string,
    age:number
}

let personone:Iperson={name:"zs",age:12}
type IPartial=Partial<Iperson>  //）类型全部转化为可选项（此例子中为name与age都为可选）
let persontwo:IPartial={name:"ls"}
// type IOmit = Omit<Iperson , "name">
// type IOmit = Omit<Iperson , "age" | "name">;
let personthree:Omit<Iperson,"age">={name:"ww"}  //  【省略/剔除】

```

## 装饰器
1. 类的装饰器

```javascript
@testable
class MyTestableClass {
  // ...
}

function testable(target) {
  target.isTestable = true;
}

MyTestableClass.isTestable // true
```

2. 方法的装饰器(类的方法)

```javascript
class Math {
  @log
  add(a, b) {
    return a + b;
  }
}
function log(target, name, descriptor) {
  var oldValue = descriptor.value;

  descriptor.value = function() {
    console.log(`Calling ${name} with`, arguments);
    return oldValue.apply(this, arguments);
  };

  return descriptor;
}

const math = new Math();

// passed parameters should get logged now
math.add(2, 4);

```



