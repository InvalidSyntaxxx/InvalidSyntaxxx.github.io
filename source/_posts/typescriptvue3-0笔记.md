---
title: TypeScript+Vue3.0笔记
tags:
  - Javascript
  - TypeScript
  - Vue
id: '1057'
categories:
  - - 学习笔记
date: 2022-05-07 16:46:40
---

## 一、什么是Typescript

[TypeScript英文文档](https://www.typescriptlang.org/docs/)

[TypeScript中文网，中文文档](https://www.tslang.cn/docs/home.html)

> *   JavaScript的超集，遵循最新的 ES6、Es5 规范。TypeScript 扩展了 JavaScript的语法，可以编译为JavaScript，添加了类型系统的JavaScript，可以适用与任何规模的项目。
>     
> *   TypeScript 是由微软开发的一款开源的编程语言。TypeScript 更像后端 java、C#这样的面向对象语言可以让 JS开发大型企业项目。
>     
> *   谷歌也在大力支持 Typescript 的推广，谷歌的 angular2.x+就是基于 Typescript 语法。
>     
> *   最新的 Vue 、React 也可以集成 TypeScript。
>     

![image-20220507164550922](https://www.wangwangyz.site/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220507164550922.png)

### TypeScript特性

#### 类型系统
<!-- more -->
从TypeScript的名字就可以看出来， **\[类型\]** 是其最核心的特性。

我们知道，JavaScript是一门非常灵活的编程语言：

> *   它没有约束类型，一个变量可能初始化时是字符串类型，过一会又被赋值为数字类型
> *   由于隐式类型转化的存在，有的变量很难在运行前就确定
> *   基于原型的的面向对象编程，使得原型上的属性或方法可以在运行时被修改。
> *   函数是JavaScript中的一等公民，可以赋值给变量，也可以当做参数或返回值

这种灵活性就像一把双刃剑，一方面使得JavaScript蓬勃发展，无所不能，从2013年开始就一直蝉联最普遍使用的编程语言排行榜冠军；另一方面也使得的代码质量参差不齐，维护成本高，运行错误多。

而TypeScript的 **类型** 系统，很大程度弥补JavaScript的缺点。

#### TypeScript是静态类型

类型系统按照 \[类型检查的时机\] 来分类，可以分为动态类型和静态类型。

动态类型是指在运行时才会进行类型检查，这种语言的类型错误往往会导致运行时错误。JavaScript是一门解释型语言，没有编译阶段，所以他是动态类型，以下这段代码在运行时才会报错：

```js
let foo = 1；
foo.split(' ');
//Uncaught TypeError: foo.split is not a function
//运行时报错：foo.split不是一个函数，造成线上bug
```

静态类型是指编译阶段就能确定每个变量的类型，这种语言的类型错误往往会导致语法错误。TypeScript在运行前需要先编译为JavaScript，而在编译阶段就会进行类型检查，所以 **TypeScript是静态类型** ，这段TypeScript代码在编译阶段就会报错：

```typescript
let foo = 1；
foo.split(' ');
//Property 'split' does not exist on type 'number'
//编译时报错：数字没有split方法，无法通过编译
```

你可能会奇怪，这段TypeScript代码看上去和JavaScript没有什么区别呀。

没错！大部分JavaScript代码都只需要经过少量的修改（或者完全不用修改）就变成了TypeScript的代码，这得益于TypeScript强大的 **类型推论**，即使不去手动声明变量 foo 的类型，也能在变量初始化的时候自动推论出他是一个 `number` 类型。

完整的TypeScript代码是这样的：

```typescript
let foo: number = 1;
foo.split(' ');
//Property 'split' does not exist on type 'number'
//编译时报错：数字没有split方法，无法通过编译
```

#### TypeScript是弱类型

类型系统按照 \[是否允许隐式类型转换\] 来分类，可分为强类型和弱类型。

以下这段代码不管是在JavaScript还是TypeScript中都是正常运行的，运行时数字1会被隐式类型转化为字符串 '1' ，加号 ‘+’ 被识别为字符串拼接，打印结果为 '11':

```js
console.log(1 + '1');
//打印字符串 '11'
```

TypeScript是完全兼容JavaScript的，他不会修改JavaScript运行时的特性，他们都是 **弱类型语言**。

## 二、安装并编译TypeScript

安装TypeScript需要NodeJS环境，如果电脑没有npm命令，可以去官网下载并安装NodeJS

官网地址：[Node.js (nodejs.org)](https://nodejs.org/en/)

![image-20220507113822749](https://www.wangwangyz.site/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220507113822749.png)

TypeScript安装命令

```shell
npm install -g typescript
# 通过tsc --version可以查看版本号以确保是否安装成功
```

![image-20220507121610924](https://www.wangwangyz.site/%E4%B8%AA%E4%BA%BA%E5%9B%BE%E5%BA%8A/image-20220507121610924.png)

安装以后编译ts文件很简单，我们在电脑上新建一个目录`code`，新建一个文件`index.ts`,然后在当前目录下输入：

```shell
tsc index.ts
```

编译完之后会在当前目录下输出一个`index.js`文件，则编译成功。如果想指定输出目录：

```
tsc --outFile ./js/index.js index.ts
```

## 三、基本的数据类型

#### 布尔值

布尔值是最基础的数据类型，在TypeScript中，使用`boolean`定义布尔值类型：

```typescript
let isDone:boolean = false;
```

#### 数值

使用`number`定义数值类型：

```typescript
let mynum:number = 1;
```

#### 字符串

使用`string`定义字符串类型：

```typescript
let mystring:string = 'TypeScript字符串'
//模板字符串,要用反引号括起来
let sentence:string = `Hello,This is ${mystring}.`;
//也可以使用js的加号 + 语法
let sentence2:string = 'Hello,This is'+mystring;
```

#### 空值

JavaScript没有空值(void)的概念，在TypeScript中，用`void`表示没有任何返回值的函数：

```typescript
function alertName():void{
    alert('my name is tom');
}
```

声明一个`void`类型的变量没有什么用，因为你只能将它赋值为`undefined`和`null`

```typescript
let unsable:void = undefined;
```

#### Null和Undefined

在TypeScript中，默认情况下`null`和`undefined`是所有类型的子类型。 就是说你可以把 `null`和`undefined`赋值给`number`类型的变量。可以使用`null`和`undefined`来定义这两个原始数据类型：

```typescript
let n:null = null;
let u:undefined = undefined
```

#### 数组

TypeScript像JavaScript一样可以操作数组元素。有两种方式定义数组。

第一种是元素类型后加 `[]`

```typescript
let list:number[] = [1,2,3];
```

第二种是使用数组泛型，`Array<元素类型>`

```typescript
let list:Array<number> = [1,2,3];
```

#### 元组

元组（Tuple）类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。但**定义变量时类型的顺序要一致**。

```typescript
let x : [string,number];
x = ['hello',10];
//正常运行
x = [20,'HELLO'];
//报错：不能将类型“number”分配给类型“string”。ts(2322)；不能将类型“string”分配给类型“number”。ts(2322)
```

当访问一个已知索引的元素，会得到正确的类型：

```typescript
console.log(x[0].substr(1)); // 正常运行
console.log(x[1].substr(1)); // 报错： 'number' does not have 'substr'
```

当访问越界元素时：

```typescript
x[3] = 'World';
//报错：不能将类型“"World"”分配给类型“undefined”。ts(2322)长度为 "2" 的元组类型 "[string, number]" 在索引 "2" 处没有元素。ts(2493)
```

#### Object

`object`表示非原始类型，也就是除`number`，`string`，`boolean`，`symbol`，`null`或`undefined`之外的类型。

使用`object`类型，就可以更好的表示像`Object.create`这样的API。例如：

```typescript
declare function create(o:objectnull):void;
create({pro:0});
create(null);
create(undefined);
create();//报错：应有 1 个参数，但获得 0 个。ts(2554)
create(42);//报错：类型“number”的参数不能赋给类型“object”的参数。ts(2345)
create("string"); //报错： 类型“string”的参数不能赋给类型“object”的参数。ts(2345)
create(false); //报错：类型“boolean”的参数不能赋给类型“object”的参数。ts(2345)
```

## 四、任意值(Any)

任意值(Any)用来表示允许赋值为任意类型。

如果是一个普通类型，在赋值过程中改变类型是不被允许的：

```typescript
let myFavoriteNum:string = 'seven';
myFavoriteNum = 7;
//报错：不能将类型“number”分配给类型“string”。ts(2322)
```

但如果是`any`类型，则允许被赋值为任意类型。

```typescript
let myFavoriteNum:any = 'seven';
myFavoriteNum = 7;
```

在任意值上访问任何属性都是允许的：

```typescript
let anything: any = 'hello';
console.log(anything.Myname);
console.log(anything.myname.length);
```

也允许调用任何方法:

```typescript
let anything:any = 'heloo'
anything.setName('Tom');
anything.setName('Allen').sayHello();
anything.Myname.setFirstName('Cat');
```

所以，**声明一个任意值(any)变量后，对它的任何操作，返回的内容的类型都是任意值**。

## 五、类型推论

如果没有明确的指定类型，那么TypeScript会依照类型推论（Type Inference）的规则推断出一个类型。

以下代码虽然没有指定类型，但在编译时会出错：

```typescript
let myFavoriteNum = 'seven';
myFavoriteNum = 7;
//报错：不能将类型“number”分配给类型“string”。ts(2322)
```

事实上，它等价于：

```typescript
let myFavoriteNum:string = 'seven';
myFavoriteNum = 7;
//报错：不能将类型“number”分配给类型“string”。ts(2322)
```

TypeScript在没有明确指定变量类型时，会对变量的类型进行推测，这就是类型推论。

**如果定义的时候没有赋值，不管之后有没有赋值，都会被推断成 `any`类型而完全不被类型检查：**

```typescript
let myFavoriteNum;
myFavoriteNum = 'seven';
myFavoriteNum = 7;
```

## 六、联合类型

联合类型(Union Types)表示取值可以为多种类型中的一种。

举个例子

```typescript
let myFavoriteNum:stringnumber;
myFavoriteNum = 'seven';
myFavoriteNum = 7;
```

从代码可以看出来联合类型使用 来分隔每一个类型。

这里的 `let myFavoriteNum:stringnumber;`的含义是允许`myFavoriteNum`为`string`和`number`类型，而不能是其他类型。

比如下面这个就会报错：

```typescript
let myFavoriteNum:stringnumber;
myFavoriteNum = true;
//报错：不能将类型“boolean”分配给类型“string  number”。ts(2322)
```

#### 联合类型的属性或方法

当TypeScript不确定一个联合类型的变量到底是哪个类型的时候，我们 **只能访问此联合类型的 共有属性或共有方法**。

```typescript
function getLength(something:numberstring):number{
    return something.length;
}
//报错:类型“string  number”上不存在属性“length”。类型“number”上不存在属性“length”。ts(2339)
```

上例中，因为`number`类型不存在`length`属性，所以会报错，正确的使用可以这样：

```typescript
function getLength(something:numberstring):string{
    return something.toString();
}
//`toString()`方法是number和string类型的共有方法。
```

联合类型变量在被赋值的时候，会根据类型推论的推断变量的类型：

```typescript
let myFavoriteNum:numberstring;
myFavoriteNum = 'seven';
console.log(myFavoriteNum.length);
//正常运行
myFavoriteNum = 7;
console.log(myFavoriteNum.length);
//报错：类型“number”上不存在属性“length”。ts(2339)
```

上例中，第二行的myFavoriteNum被推断成了 `string`型，访问 `length`属性就不会报错

而第四行的myFavoriteNum被推断为`number`型，访问`length`属性就会报错

## 七、接口

在TypeScript中，我们使用 `interface` 来定义一个接口类型的对象。

#### 什么是接口

在面向对象语言中，接口是一个重要的概念，它是对行为的抽象，而具体的行为则需要类去实现。

typesc的核心原则之一是对之所具有的结构进行类型检查。有时候被称作“鸭式辨型法”或者“结构性子类型化”。在TypeScript里，接口的作用就是为这些类型命名和为你的代码或第三方代码定义契约。

举个例子

```typescript
interface Person{
    name:string;
    age:number;
    sex:string;
};

let Allen:Person = {
    name:'Allen',
    age:28,
    sex:'male'
};
//正常运行
```

上面例子中，我们定义一个接口Person，接着定义一个变量 Allen，他的类型是Person。这样，我们就约束了Allen的形状必须是和接口Person一致，如果少了XX属性就会报错，同理，多了未定义的XX属性也会报错：

```typescript
interface Person{
    name:string;
    age:number;
    sex:string;
};

let Tom:Person = {
    name:'Tom',
    age:18,
};
//Tom报错：类型 "{ name: string; age: number; }" 中缺少属性 "sex"，但类型 "Person" 中需要该属性。ts(2741) index.ts(23, 2): 在此处声明了 "sex"。
//原因是Tom少了sex属性

let Ketty:Person = {
    name:'Ketty',
    age:20,
    sex:'female',
    grade:12
}
//报错：不能将类型“{ name: string; age: number; sex: string; grade: number; }”分配给类型“Person”。对象文字可以只指定已知属性，并且“grade”不在类型“Person”中。ts(2322)
//原因是Ketty多了grade属性
```

定义Tom变量却少了sex属性就会报错，同理，多了未定义的grade属性也会报错。

可见，**赋值的时候，变量的结构必须和接口的结构保持一致**。

#### 可选属性

有时候我们希望不要完全匹配一个接口的所有结构，那么我们可以用可选属性(在定义接口时，属性后加上 `?`关键字)：

```typescript
interface Person{
    name:string;
    age:number;
    sex?:string; //可选属性 sex
};
let Allen:Person = {
    name:'Allen',
    age:28,
    sex:'male'
};
//正常运行
let Tom:Person = {
    name:'Tom',
    age:18,
};
//正常运行
```

#### 任意属性

有时候我们希望一个接口允许有任意的属性，可以用 `[属性名:类型名]`定义任意属性

```typescript
interface Person{
    name:string;
    age?:number;
    [propName:string]:any;
};

let Allen: Person = {
    name:'Allen',
    gender:'female' //添加gender属性是允许的
};

let Tom:Person = {
    name:'Tom',
    gender:'male',
    gender2:'male',
    ID:123,
    123:'asdasd',
    isDone:false
}
//添加任意多个属性且类型不一致也是允许的，有点违背接口的初衷
```

使用 `[propName:string]:any;`定义了任意属性取`string`类型的值。

任意属性有点违背接口的初衷，既然已经定义了接口的结构，就不能任意去增加修改他的结构了

#### 只读属性

有时候我们希望对象中一些字段只能在创建时被赋值，后续只能可读不可写，那么可以用 `readonly`关键字定义只读属性：

```typescript
interface Person{
    readonly id:number;
    name:string;
    age:number;
};
let Tom:Person = {
    id:12345,
    name:'Tom',
    age:19    
}
Tom.age =  29 //不报错
Tom.id = 12345//报错：无法分配到 "id" ，因为它是只读属性。ts(2540)
```

上述例子中`id`属性被设置为只读属性，当再次赋值时就会报错。

## 八、数组

数组是存放多个元素的集合

最简单的方法是使用 \[ 类型 + 方括号\] 来表示数组：

```typescript
let fibonacci:number[] = [1,1,2,3,5,8,13];
```

数组中的项不允许出现其他的类型：

```typescript
let fibonacci:number[] = [1,'1',2,3,5,8,13];
//报错：不能将类型“string”分配给类型“number”。ts(2322)
```

数组的一些方法的参数也会根据数组在定义时约定的类型进行限制：

```typescript
let fibonacci:number[] = [1,1,2,3,5,8,13];
fibonacci.push(21);
//正常运行
fibonacci.push('34');
//报错：类型“string”的参数不能赋给类型“number”的参数。ts(2345)
//原因是push()方法只允许传入number类型的参数，而'34'是字符串字面量类型，后续章节会介绍
```

也可以指定一个 any 类型数组：

```typescript
let list:any[] = ['Tom',18,'male',true,{website:'https://wangwangyz.site'}];
```

## 九、函数01

#### 函数声明

在JavaScript中，有两种常见的定义函数的方式——函数声明(Function Declaration)和函数表达式(Function Expression)：

```javascript
//函数声明(Function Declaration)
function sum(x,y){
    return x+y;
}

//函数表达式(Function Expression)
let mySum = function (x,y){
    return x+y;
};
```

一个函数有输入和输出，要在TypeScript中对其进行约束，需要把输入和输出都考虑到，其中函数声明的类型定义比较简单：

```typescript
function sum(x:numebr,y:number):number{
    return x+y;
}
```

注意，**输入多余(或少于)、类型不匹配的参数，是不被允许的**：

```typescript
function sum(x:numebr,y:number):number{
    return x+y;
}
sum(1,2,3)//报错：应有 2 个参数，但获得 3 个。ts(2554)
sum(1)//报错：应有 2 个参数，但获得 1 个。ts(2554)
sum('srt',1)//报错：类型“string”的参数不能赋给类型“number”的参数。ts(2345)
```

#### 函数表达式

如果我们现在写一个对函数表达式(Function Expression)的定义，可能会写成这样：

```typescript
let mysum = function(x:number,y:number):number{
    return x + y;
};
```

这是可以通过编译的，不过事实上，上面的代码只对 **等号右侧的匿名函数**进行了类型定义，而等号左边的**mysum**，是通过赋值操作进行类型推论而推断出来的。如果需要我们手动给**mysum**添加类型，则是这样：

```typescript
//这里用的是类型推断
//let mysum = function(x:number,y:number):number{
//  return x + y;
//};

//如果是手动指定类型，应该是这样
let mysum:(x:number, y:number) => number = function (x:number,y:number):number{
    return x + y;
};
```

注意不要混淆了TS中的 `=>` 和ES6中的 `=>`.

在TypeScript的类型定义中，`=>`用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。

在ES6中，`=>`叫做箭头函数，应用十分广泛，可以参考ES6的箭头函数：[参考链接：阮一峰ES6箭头函数教程](https://www.bookstack.cn/read/es6-3rd/spilt.5.docs-function.md)

#### 用接口定义函数的形状

我们也可以使用接口的方式来定义一个符合某个形状的函数：

```typescript
interface SearchFunc{
    (source:string,subString:string):boolean;
}

let mySearchFunc:SearchFunc;
mySearchFunc = function(source:string,subString:string){
    return source.search(subString) !== -1;
}

let mySearchFunc1:SearchFunc;
mySearchFunc1 = function(source:number,subString:string){
    return 1==1;
}//报错：不能将类型“(args1: number, args2: string) => boolean”分配给类型“SearchFunc”。参数“args1”和“source” 的类型不兼容。不能将类型“string”分配给类型“number”。ts(2322)

let mySearchFunc2:SearchFunc;
mySearchFunc2 = function(source:string,subString:string){
    return 1;
}//报错：不能将类型“(args1: string, args2: string) => number”分配给类型“SearchFunc”。不能将类型“number”分配给类型“boolean”。ts(2322)
```

采用函数表达式接口定义函数的方式是，对等号左侧进行类型限制，可保证以后对函数名赋值时保证 **参数个数、参数类型、返回值类型** 不变。

## 十、函数02

#### 可选参数

前面提到，多余的(或者少于要求的)参数，是不允许的。那么如何定义可选的参数呢？

与接口中的可选属性类似，我们用关键字 `?`表示可选的参数：

```typescript
function buildName(firstName:string, lastName?:string){
    if(lastName){
        return firstName + " " + lastName;
    }else {
        return firstName;
    }
}

let tomcat = buildName("Tom", "cat");
let tom = buildName(undefined, "Tom");
```

需要注意的是，**可选参数必须接在必须参数的后面**，换句话说，**可选参数后面不允许再出现必须参数了**。(这一点和Python很像)

```typescript
function buildName(firstName?:string, lastName:string){//报错：必选参数不能位于可选参数后。ts(1016)
    if(lastName){
        return firstName + " " + lastName;
    }else {
        return firstName;
    }
}
let tomcat = buildName("Tom", "cat");
let tom = buildName(undefined, "Tom");
```

#### 参数默认值

在ES6中，我们允许给函数的参数添加默认值，**TypeScript会将添加了默认值的参数识别为可选参数**：

```typescript
function buildName(firstName:string, lastName:string = 'Cat'){
    return firstName + " " + lastName;
}
let tomcat = buildName("Tom", "ccccat");
let tom = buildName("Tom");
```

此时就不受 \[可选参数必须接在必需参数后面\] 的限制了：

```typescript
function buildName(firstName:string = 'Tom', lastName:string){
    return firstName + " " + lastName;
}
let tomcat = buildName("Tom", "cat");
let tom = buildName(undefined, "Tom");
```

## 十一、函数03

ES6中，可以使用 `...rest` 的方式获取函数中的剩余参数(rest参数):

```typescript
function push(array, ...items){//参数 "array" 隐式具有 "any" 类型
    items.forEach(function(item){
           array.push(item);
    });
}
let a :any[] = [];
push(a,1,2,3,4);
```

事实上，`items` 是一个数组，所以我们可以用数组的类型来定义它：

```typescript
function push(array:any[], ...items:any[]){
        items.forEach(function(item){
              array.push(item);
    });
}
let a = [];
push(a,1,2,3,4);
```

注意，rest参数只能是最后一个参数，关于rest参数，可以参考：[阮一峰ES6 rest参数详解](https://www.bookstack.cn/read/es6-3rd/spilt.2.docs-function.md)

#### 重载

重载允许一个函数接收不同数量或类型的参数，并做不同处理。

比如，我们需要实现一个函数`reverse`，输入数字123的时候，输出反转的数字321，输入字符串hello的时候，输出反转的字符串olleh。

利用联合类型，我们可以这么做：

```typescript
function reverse(x: numberstring):number  string  void{
    if(typeof x === 'number'){
        return Number(x.toString().split('').reverse().join(''));
    }else if (typeof x === 'string'){
            return x.split('').reverse().join('');
}
```

然而这样有个缺点，就是不能够精确地表达，输入为数字的时候，输出应该也为数字，输入为字符串的时候，输出也应该为字符串。

这时，我们可以使用 **重载** 定义多个 `reverse`的函数类型：

```typescript
function reverse(x:number):number;
function reverse(x:string):string;
function reverse(x:numberstring):numberstringvoid{
    if(typeof x === 'number'){
        return Number(x.toString().split('').reverse().join(''));
    }else if (typeof x === 'string'){
        return x.split('').reverse().join('');
} 
}
reverse(123) // function reverse(x: number): number (+1 overload)
reverse('12345') // function reverse(x: number): number (+1 overload)
```

上例中，我们重复定义了多次函数`reverse`，前两次都是函数定义，最后一次是函数实现。在编辑器的代码提示中，可以正确看到前两个提示。

注意，TypeScript中会优先从最前面的函数定义开始匹配，所以多个函数定义如果有包含关系，需要优先把精确的定义写在前面。

## 十二、类型断言01

基本语法、将一个联合类型断言为其中一个类型。

类型断言(Type Assertion)可以用来手动指定一个值的类型

> 通过_类型断言_这种方式可以告诉编译器，“相信我，我知道自己在干什么”。 类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用。 TypeScript会假设你，程序员，已经进行了必须的检查。
> 
> ——————引自[基础类型 · TypeScript中文网 · TypeScript——JavaScript的超集 (tslang.cn)](https://www.tslang.cn/docs/handbook/basic-types.html)

#### 基本语法

有两种表达：

> *   值 as 类型
>     
> *   <类型>值
>     

在tsx语法中（React的jsx语法的ts版）中必须使用 `值 as 类型`表示类型断言。

故建议大家在使用类型断言时，统一使用 `值 as 类型`这样的语法。

#### 将一个联合类型断言为其中一个类型

之前提过，当TypeScript中不确定一个联合类型的变量到底是哪个类型的时候，我们 **只能访问联合类型所有类型的共有属性或方法**：

```typescript
interface Cat{
    name:string;
    run():void;
}
interface Fish{
    name:string;
    swim():void;
}
function getName(animal:CatFish):string{
    return animal.name;
//正常运行
}

function getName(animal:CatFish):string{
    return animal.run();
//报错：类型“Cat  Fish”上不存在属性“run”。类型“Fish”上不存在属性“run”。ts(2339)
}
```

而有时候，我们确实需要在还不确定类型的时候就访问其中一个类型的属性或者方法，比如：

```typescript
interface Cat{
    name:string;
    run():void;
}
interface Fish{
    name:string;
    swim():void;
}
function isFish(animal:CatFish):boolean{
    if(typeof animal.swim === 'function'){
        return true;
    }
    return false;
}
//报错：类型“Cat  Fish”上不存在属性“swim”。类型“Cat”上不存在属性“swim”。ts(2339)
```

上述例子中，获取 `animal.swim`时会报错，

此时可以使用类型断言，将animal断言为Fish：

```typescript
function isFish(animal:CatFish):boolean{
    //将animal断言为Fish
    if(typeof (animal as Fish).swim === 'function'){
        return true;
    }
    return false;
}
```

这样就可以解决访问animal.swim时的报错问题了。

不过需要注意的是，类型断言只能够 ’欺骗‘TypeScript编译器，无法避免运行时的错误，滥用类型断言反而会导致运行时错误：

```typescript
function swim(animal:CatFish):void{
    (animal as Fish).swim();
}
const tom:Cat = {
    name:'Tom',
    run(){ console.log("I'm running...");}
};
swim(tom)
//编译时不报错，运行时报错：TypeError：animal.swim is not a function
```

上面例子中，编译时不会报错，但是运行时会报错，当我们执行 tsc命令编译时会看到输出的文件无内容。

原因是因为 `(animal as Fish).swim()` 这段代码隐藏了 animal可能为 Cat 的情况，将 animal 直接断言为 Fish了，而TypeScript编译器信任了我们的断言，故在调用 `swim()`时编译没有错误。

总之，**使用断言一定要格外小心**，尽量避免断言后调用方法或引用深层属性。以减少不必要的 **运行时错误**。

## 十三、类型断言02

将一个父类断言为具体的子类

当类之间有继承关系时，类型断言也是很常见的：

```typescript
class ApiError extends Error{
    code:number = 0;
}
class HttpError extends Error{
    statusCode:number = 200;
}

function isApiError(myError:Error){
    if(typeof (myError as ApiError).code === 'number'){
        return true;
    }
    return false;
}
```

上述例子中，我们声明了函数isApiError，它用来判断传入的参数是不是ApiError类型，为了实现这样一个函数，它的参数类型肯定得是比较抽象的父类Error，这样的话这个函数就能接受Error或他的子类作为参数了。

但是由于父类Error中没有code属性，故直接获取myError.会报错：类型“Error”上不存在属性“code”。这时候需要使用类型断言获取(myError as ApiError).code。

大家可能会注意到，在这个例子中会有一个更合适的方式来判断是不是ApiError，那就是使用instaceof：

```typescript
class ApiError extends Error{
    code:number = 0;
}
class HttpError extends Error{
    statusCode:number = 200;
}
function isApiError(myError:Error){
    if(myError instanceof ApiError){
        return true;
    }
    return false;
}
```

上面的例子中，用instanceof确实是一个很好的方式，因为ApiError是一个JavaScript的类，能够通过instanceof判断是否是它的实例。

但是有的情况下ApiError和HttpError不是一个真正的类，而只是一个TypeScript接口，接口是一个类型，不是一个真正的值，它在编译结果中会被删除，当然就无法用instanceof来判断：

```typescript
interface ApiError extends Error{
    code:number;
}
interface HttpError extends Error{
    statusCode:number;
}
function isApiError(myError:Error){
    if(myError instanceof ApiError){
        return true;
    }
    return false;
}
//报错：“ApiError”仅表示类型，但在此处却作为值使用
```

此时就只能用类型断言，通过判断是否存在 code 属性来判断传入的参数是否为 ApiError了：

```typescript
interface ApiError extends Error{
    code:number;
}
interface HttpError extends Error{
    statusCode:number;
}
function isApiError(myError:Error){
    if(typeof (myError as ApiError).code === 'number'){
        return true;
    }
    return false;
}
```

## 十四、类型断言03

将任何一个类型断言为 `any`

理想情况下，TypeScript的类型系统运转良好，每个值的类型都具体而精确。

当我们引用一个在此类型上不存在的属性或方法时，会报错：

```typescript
const foo: number = 1;
console.log(foo.length);
//报错：类型“number”上不存在属性“length”。ts(2339)
```

上面例子中，数字类型的变量foo上是没有length属性的，故TypeScript编译时给出了错误提示。

这种错误提示显然是很有用的。

但有的时候，我们非常确定这段代码不会出错，比如：

```typescript
window.foo = 1;
//报错：类型“Window & typeof globalThis”上不存在属性“foo”。ts(2339)
```

上面的例子中，我们需要将window上添加一个foo属性，但是TS会给我们报错，提示window不存在属性foo。

此时我们可以使用`as any`临时将window断言为`any`类型：

```typescript
(window as any).foo = 1;
```

在 `any` 类型中，**访问任何属性都是可以的**。

需要注意的是，将一个变量断言为any可以说是解决TypeScript中类型问题的最后一个手段。

但是它极有可能真正掩盖了类型错误，如果不是十分确定，就不要使用 `as any`。

总之，一方面不要滥用 any类型，另一方面也不要忽略他带来的作用。我们需要在类型的严格性和开发的方便性之间平衡利弊，才能发挥TypeScript最大的价值。

## 十五、类型断言04

将`any`类型断言为一个具体的类型

在日常的开发中，我们不可避免的需要处理any类型的变量，它们可能是由于第三方库未能定义好自己的类型，也可能是历史遗留的或其它人编写的烂代码，还可能是受到TypeScript类型系统的限制而无法精确定义类型的场景。

遇到`any`类型的变量时，我们可以选择无视他，任由他滋生更多的`any`类型。

我们可以选择改进它，通过类型断言及时地吧`any`断言为精确的类型，亡羊补牢，使我们的代码向着可维护性高的目标发展。

举例来说，历史遗留的代码中有个getCacheData函数，它的返回值是any:

```typescript
function getCacheData(key:string):any{
    return (window as any).cache[key];
}
```

那么我们在使用这个getCacheData函数的时候，最好能够将调用了getCacheData之后的返回值断言为一个精确的类型，这样就方便了后续的操作：

```typescript
function getCacheData(key:string):any{
    return (window as any).cache[key];
}

interface Cat{
    name:string;
    run():void;
}

const tom = getCacheData('tom') as Cat;
tom.run();
```

上面例子中，我们调用完getCacheData之后，立即将她断言为Cat类型，这样的话明确了tom的类型，后续对tom的访问时就有了代码补全，提高了代码的可维护性。

> ##### 题外话
> 
> 个人认为在遇到较多any类型变量时，尽量在接下来少用或者不用`any`类型，根据IDE给出的提示将类型规范化，能减少项目80%的潜在bug。
> 
> #### 解决any出现次数过次的问题
> 
> 复杂类型的类型定义
> 
> 1.  细拆出重复定义的公共项，使用extents 关键字或者 & 交叉运算符来进行整合，提高利用率； 例1：
>     
>     ```typescript
>     1. // bad
>       interface Person {
>           firstName: string;
>         lastName: string;
>        }
>     
>     interface PersonWithBirthDate {
>       firstName: string;
>      lastName: string;
>      birth: Date;
>     }
>     // good
>     interface Person {
>       firstName: string;
>      lastName: string;
>     }
>     interface PersonWithBirthDate extends Person {
>       birth: Date;
>     }
>     ```
>     
>     例2：
>     
>     ```typescript
>     export type List = {
>         creatTime: number  string;
>         creator: string;
>         desc: string;
>         id: string;
>         modifier: string;
>         token: string;
>         updateTime: number  string;
>       };
>     
>     export type ProjectList = {
>      id: string;
>      projectName: string;
>     } & List;
>     
>     export type TaskList = {
>      name: string;
>     } & List;
>     ```
>     
>     新的问题：如果很多字段要打问号怎么办？下面会讲到
>     
> 2.  使用typeof定义一个类型匹配初始值（常见的使用场景之一：固定的Schema配置适用）
>     
>     ```typescript
>     const INIT_OPTIONS= {
>       width: 640,
>      height: 480,
>      color: "#00FF00",
>      label: "VGA",
>     };
>     
>     interface Options {
>       width: number;
>      height: number;
>      color: string;
>      label: string;
>     }
>     
>     // 快速获取配置对象的形状
>     type Options = typeof INIT_OPTIONS;
>     ```
>     
> 3.  使用Ts 内置类型来解决？号太多的问题
>     
>     *   Utility Types
>     *   充分利用lib.es5.d.ts中的Partial, Pick , Extract, Omit等方法，扩展第三方、或已存在的类型，不要重复定义完全一样的字段。
> 
> 参考链接：[TypeScript：为什么不要用any声明类型 - 掘金 (juejin.cn)](https://juejin.cn/post/7074832632541872136)
> 
> [规范TS项目Any类型的使用\_Jasmine\_jiamei的博客-CSDN博客\_ts 对象属性any](https://blog.csdn.net/weixin_43827779/article/details/120343486)

## 十六、类型断言05

类型断言的限制

从上面的例子可以总结出：

> *   联合类型可以被断言为其中一个类型
>     
> *   父类可以被断言为子类
>     
> *   任何类型都可以被断言为 `any`
>     
> *   `any`可以被断言为任意类型
>     

那么类型断言有没有什么限制呢？是不是任何一个类型都可以被断言为任何另一个类型呢？

答案是否定的——并不是任何一个类型都可以被断言为任何另一个类型。

具体来说，若A、B两者具有共同的属性或者方法，那么A能够被断言为B，B也能够断言为A。

下面我们通过一个简化的例子，来理解类型断言的限制：

```typescript
//两者是有共同的属性或者方法，比如Animal和Cat都有name
interface Animal{
    name:string;
}
interface Cat{
    name:string;
    run():void;
}
function testAnimal(animal: Animal){
    return (animal as Cat);
}
function testCat(cat : Cat){
    return (cat as Animal);
}
```

上述例子中是可以断言的，我们再看看下面的例子：

```typescript
//两者没有有共同的属性或者方法，
interface Animal{
    name:string;
}
interface Cat{
//  name:string;
    run():void;
}
function testAnimal(animal: Animal){
    return (animal as Cat); //报错：类型 "Animal" 中缺少属性 "run"，但类型 "Cat" 中需要该属性。ts(2352)
}
function testCat(cat : Cat){
    return (cat as Animal);//报错：类型 "Cat" 中缺少属性 "name"，但类型 "Animal" 中需要该属性。ts(2352)
}
```

这时候会报错，两者不能充分重叠，这意味着要想断言成功，需要具备一个条件：

*   要使得A和B能够被 **互相断言**，就要A兼容B或者B兼容A

## 十七、类型断言06

双重断言

既然：

*   任何类型都可以被断言为any
*   an可以被断言为任何类型

那么我们是不是可以使用双重断言 `as any as foo` 来将任何一个类型断言为另一个类型呢？

```typescript
interface Cat{
    run():void;
}
interface Fish{
    swim():void;
}

function testCat(cat : Cat){
    return (cat as any as Fish);
}
```

上述例子中，若直接使用 `cat as Fish` 会报错：类型 "Cat" 中缺少属性 "swim"，但类型 "Fish" 中需要该属性。因为Cat和Fish都互不兼容。

但是若使用双重断言，则可以打破 \[要使得A能都断言B ,就要A兼容B或者B兼容A\] 的限制，将任何一个类型断言为任何另一个类型。

若你使用了这种双重断言，那么十有八九都是非常错误。他很可能会导致运行时错误。

**除非迫不得已，否则千万别用双重断言**。（咋迫不得已？基本不用吧？）

## 十八、类型断言07

类型断言VS类型转换

类型断言只会影响TypeScript编译时的类型，类型断言语句会在编译结果中被删除：

```typescript
function toBoolean(something:any):boolean{
    return something as boolean;
}
toBoolean(1);
//返回值为1
```

在上面的例子中，将`something`断言为`boolean`可以通过编译，但是并没有什么用，代码在编译后会变成：

```javascript
function toBoolean(something) {
    return something;
}
toBoolean(1);
//返回值为1
```

所以类型断言不是类型转换，他不会真的影响到变量的类型。

若要进行类型转化，需要直接调用类型转换的方法：

```typescript
function toBoolean(something:any):boolean{
    return Boolean(something);
}
toBoolean(1);
//返回值为true
```

## 十九、类型断言08

类型断言VS类型声明

在这个例子中：

```typescript
function getCacheData(key:string):any{
    return (window as any).cache[key];
}

interface Cat{
    name:string;
    run():void;
}

const tom = getCacheData('tom') as Cat;
tom.run();
```

我们使用 `as Cat`将`any`类型断言为了`Cat`类型。

但实际上还有其他方式可以解决这个问题：

```typescript
function getCacheData(key:string):any{
    return (window as any).cache[key];
}

interface Cat{
    name:string;
    run():void;
}
const tom:Cat = getCacheData('tom');
tom.run();
```

上面的例子中，我们通过类型声明的方式，将`tom`声明为`Cat`，然后再将`any`类型的getCacheData('tom')赋值给`Cat`类型的`tom`。

这和类型断言是非常相似的，而且产生的结果也几乎是一样的——`tom`在接下来的代码中都变成了`Cat`类型。

他们的区别，可以通过这个例子来理解：

```typescript
interface Animal{
    name:string;
}
interface Cat{
    name:string;
    run():void;
}
const animal: Animal = {
    name:'tom'
}
let tom = animal as Cat;
```

上述例子中，由于`Animal`兼容 `Cat`，故可以将 `animal` 断言为 `Cat`赋值给`tom`。

但是若直接声明tom为`Cat`类型：

```typescript
interface Animal{
    name:string;
}
interface Cat{
    name:string;
    run():void;
}
const animal: Animal = {
    name:'tom'
}
let tom: Cat = animal;
//报错：类型 "Animal" 中缺少属性 "run"，但类型 "Cat" 中需要该属性。ts(2741)
```

则会报错，不允许将Cat类型的tom赋值为animal。

我们可以得出结论：

*   A断言为B时，A和B有一个及以上相同的属性或方法即可
*   A声明为B时，A必须具备B的所有属性和方法

知道了他们的核心区别，就知道了类型声明是比类型断言更加严格的。

所以为了增加代码的质量我们最好优先使用类型声明，这也比类型断言的 `as`语法更有优势。

## 二十、类型断言09

类型断言VS泛型

这是一个例子：

```typescript
function getCacheData(key:string):any{
    return (window as any).cache[key];
}
interface Cat{
    name:string;
    run():void;
}
const tom = getCacheData('tom') as Cat;
tom.run();
```

我们还有第三种方式可以解决这个问题，那就是泛型：

```typescript
function getCacheData<T>(key:string):T{
    return (window as any).cache[key];
}
interface Cat{
    name:string;
    run():void;
}
const tom = getCacheData<Cat>('tom');
tom.run();
```

通过给`getCacheData`函数添加一个泛型 ,我们可以更加规范的实现对`getCacheData`返回值的约束，这也同时去除掉了代码中的`any`，是最优的一个解决方式。

> 关于**泛型**
> 
> 是一种把明确类型的工作推迟到创建对象或者调用方法的时候才去明确的特殊的类型。
> 
> **泛型的定义**
> 
> 主要有以下两种：
> 
> 1.  在程序编码中一些包含**类型参数**的类型，也就是说泛型的参数只可以代表类，不能代表个别对象。（这是当今较常见的定义）
> 2.  在程序编码中一些包含参数的[类](https://baike.baidu.com/item/类)。其参数可以代表类或对象等等。（现在人们大多把这称作[模板](https://baike.baidu.com/item/模板)）
> 
> 不论使用哪个定义，泛型的参数在真正使用泛型时都必须作出指明。
> 
> 参考：[泛型\_百度百科 (baidu.com)](https://baike.baidu.com/item/泛型/4475207#2)

## 二一、type关键字

使用type关键字定义类型别名和字符串字面量类型

我们来看一个方法：

```typescript
function getName(n:string(() => string)):string{
    if(typeof n === 'string'){
        return n;
    }else{
        return n();
    }
}
```

`type`关键字作为类型别名用来给一个类型起个新名字

```typescript
type Name = string;
type NameResolver = ()=> string;
type NameOrResolver = Name  NameResolver;
function getName(n:NameResolver):Name{
    if (typeof n === 'string'){
        return n;
    }else{
        return n();
    }
}
```

上述例子中，我们使用`type`关键字创建类型别名。

**类型别名常用语联合类型**。

字符串字面量类型用来约束取值只能是某几个字符串中的一个

```typescript
type EventsName = 'click'  'scroll'  'mousemove';
function handleEvent(ele:Element  null,event:EventsName){
    //do something
}
handleEvent(document.getElementById('hello'),'click');
//正常运行
handleEvent(document.getElemnetById('world'),'dbclick');
//报错：类型“"dbclick"”的参数不能赋给类型“EventsName”的参数。ts(2345)
```

## 二二、元组

数组合并了相同类型的对象，而元组(Tuple)合并了不同类型的对象。

元组起源与函数编程语言(如F#，Python也有元组的概念)，这些语言中会频繁使用元组。

**举个例子**，定义一对值分别为 `string`和 `number`的元组：

```typescript
let tom:[string,number] = ['Tom',15];
```

当赋值或访问一个已知索引的元素时，会得到正确的类型：

```typescript
let tom:[string,number];
tom[0] = 'Tom';
tom[1] = 15;
```

也可以赋值其中一项：

```typescript
let tom:[string,number];
tom[0] = 'Tom';
```

但是当直接对元组进行初始化赋值操作时，需要提供所有元组类型指定的项

```typescript
let tom:[string,number];
tom = ['Tom',18];
```

下面这样就不行了：

```typescript
let tom:[string,number];
tom = ['Tom'];
//报错：不能将类型“[string]”分配给类型“[string, number]”。源具有 1 个元素，但目标需要 2 个。ts(2322)
```

**越界元素**

当添加越界元素时，他的类型会被限制为元组中每个类型的联合类型：

```typescript
let tom:[string,number];
tom = ['Tom',18];
tom.push('male');//可以添加stirng，此时tom为['Tom',18,'male']
tom.push(true);//不可以添加boolean
//报错：类型“boolean”的参数不能赋给类型“string  number”的参数。ts(2345)
```

## 二三、枚举

枚举(Enum)类型用于取值被限定在一定范围的场景，比如一周只能有七天，颜色限定为红绿蓝等。

枚举使用`enum`关键字来定义：

```typescript
enum Days{Sun, Mon, Tue, Wed, Thu, Fri, Sat};
```

枚举成员会被赋值为从 0 开始递增的数字，同时也会对枚举值到枚举名进行反向映射：

```typescript
enum Days{Sun, Mon, Tue, Wed, Thu, Fri, Sat};

console.log(Days['Sun'] === 0);//true
console.log(Days['Mon'] === 1);//true
console.log(Days['Wed'] === 3);//true
console.log(Days['Thu'] === 4);
//console.log(Days['Sun'] === 4);//false,且不能通过编译，IDE提示：此条件将始终返回 "false"，因为类型 "Days.Sun" 和 "4" 没有重叠。

console.log(Days[0] === 'Sun');//true
console.log(Days[1] === 'Mon');//true
console.log(Days[3] === 'Wed');//true
console.log(Days[4] === 'Sun');//false，IDE不进行提示
```

上面的例子会被编译为JS代码：

```javascript
var Days;
(function (Days) {
    Days[Days["Sun"] = 0] = "Sun";
    Days[Days["Mon"] = 1] = "Mon";
    Days[Days["Tue"] = 2] = "Tue";
    Days[Days["Wed"] = 3] = "Wed";
    Days[Days["Thu"] = 4] = "Thu";
    Days[Days["Fri"] = 5] = "Fri";
    Days[Days["Sat"] = 6] = "Sat";
})(Days  (Days = {}));
;
console.log(Days['Sun'] === 0); //true
console.log(Days['Mon'] === 1); //true
console.log(Days['Wed'] === 3); //true
console.log(Days['Thu'] === 4);
// console.log(Days['Sun'] === 4);//false,IDE提示：此条件将始终返回 "false"，因为类型 "Days.Sun" 和 "4" 没有重叠。
console.log(Days[0] === 'Sun'); //true
console.log(Days[1] === 'Mon'); //true
console.log(Days[3] === 'Wed'); //true
console.log(Days[4] === 'Sun'); //false，IDE不进行提示
```

## 二四、类01

#### 类的概念

虽然JavaScript中有类的概念，但是可能大多数JavaScript程序员并不是非常熟悉类，这里对类相关的概念做一个简单的介绍。

*   类(Class):定义一件事物的抽象特点，包含它的属性和方法
    
*   对象(Object):类的实例，通过 `new`生成
    
*   面向对象编程(Object Oriented Programming，简称 OOP)三大特性：**继承、封装、多态**
    
*   继承(Inheritance):子类继承父类，子类除了拥有父类的所有特性外，还有一些更具体的特征。
    
*   封装(Encapsulation):将对数据的操作细节隐藏起来，只暴露对外的接口。外界调用端不需要（也不可能）知道细节，就能通过对外提供的接口来访问该对象，同时也保证了外界无法任意更改对象内部的数据。
    
*   多态(Polymorphism):由继承而产生相关的不同的类，对同一个方法可以有不同的响应。比如Cat和Fish都继承自Animal，但是分别实现了自己的eat方法。此时针对某一个实例，我们无须了解他是Cat还是Dog，就可以直接调用eat方法，程序会自动判断出来应该如何执行eat方法。
    
*   存取器(Getter & Setter)：用于改变属性的读取和赋值行为
    
*   修饰器(Modifiers):修饰符是一些关键字，用于限定成员或类型的性质。比如public 表示共有的属性或方法。
    
*   抽象类(Abstract Class):抽象类是提供给其他类继承的基类，抽象类不允许被实例化。抽象类中的抽象方法必须在子类中被实现。
    
*   接口(Interface):不同类之间共有的属性或方法，可以抽象成一个接口，接口可以被类实现(implements)。一个类只能继承自另一个类，但是可以实现多个接口。
    
*   构造函数(Constructor):构造函数 ，是一种特殊的方法。主要用来在创建对象时初始化对象， 即为对象成员变量赋初始值，总与new运算符一起使用在创建对象的语句中
    

使用`class`关键字定义类，使用 `constructor`关键字定义构造函数。

通过`new`生成新的实例的时候，会自动调用构造函数：

```typescript
class Animal{
    public _name;
    constructor(name:string){
        this._name = name;
    }
    sayHello(){
        return `My name is ${this._name}`;
    }
}

let tom = new Animal('Tom');
console.log(tom.sayHello); //My name is tom
```

## 二五、类02

类的存取器 ：`get` 、`set`

使用getter 和 setter 可以获取和改变类的属性：

```typescript
class Animal{
    // private name:string;
    constructor(name:string){
        this.name = name;
    }
    get name(){
        return 'Jack';
    }
    set name(value){
        console.log('This name:'+value);
    }
}

let a = new Animal('Kitty');//setter Kitty
a.name = 'Tom';//setter Tom
console.log(a.name);//getter Jack
```

## 二六、类03

类的静态方法

使用`static`修饰符修饰的方法成为静态方法，他们不需要实例化，而直接通过类来调用：

```typescript
class Animal{
    public _name;
    constructor(name:string){
        this._name = name;
    }
    sayHi(){//这是实例方法
        return `My name is ${ this._name }`;
    }
    static sayHello(){//这是类方法
        return "I'm Animal class";
    }
}

let a = new Animal('Jack');
console.log(a.sayHi());//My name is Jack
console.log(Animal.sayHello());//I'm Animal class
```

## 二七、类04

类的三种访问修饰符：public、private、protected

访问权限大小由大到小：

*   public
    
    全局的、公共的，当前所涉及到的地方都可以使用
    
    ```typescript
    class Animal{
    public _name;
      public constructor(name:string){
          this._name = name;
      }
    }
    
    let a  = new Animal('Jack');
    console.log(a._name);// Jack
    a._name = 'Tom';
    console.log(a._name);// Tom
    ```
    
*   protected
    
    受保护的，允许子类访问，不允许公共访问：
    
    ```typescript
    class Animal{
    protected name;
      public constructor(name:string){
          this.name = name;
      }
    }
    
    class Cat extends Animal{
      public constructor(name:string){
          super(name);
          console.log(this.name);
      }
    } 
    ```
    
*   private
    
    私有的，只能在类的内部使用，子类也无法访问，无法在实例后通过类的实例属性访问：
    
    ```typescript
    class Animal{
    private _name;
      public constructor(name:string){
          this._name = name;
      }
    }
    
    let a  = new Animal('Jack');
    console.log(a._name);// 报错:属性“_name”为私有属性，只能在类“Animal”中访问。ts(2341)
    a._name = 'Tom'; //报错:属性“_name”为私有属性，只能在类“Animal”中访问。ts(2341)
    console.log(a._name);// 报错:属性“_name”为私有属性，只能在类“Animal”中访问。ts(2341)
    ```
    

默认是**public**，但是 TSLint 可能会要求必须用限定符来表明这个属性或方法是什么类型。

## 二八、类05

参数属性和只读属性关键字

修饰符和`readonly`还可以使用在构造函数参数中，等同于类中定义该属性同时给该属性赋值，使代码更加简洁：

```typescript
class Animal{
    public name:string;
    public constructor(public myname:string){
        this.name = myname;
    }
}
```

只读属性

```typescript
class Animal{
    readonly name:string;
    public constructor(myname:string){
        this.name = myname;
    }
}

let a  = new Animal('Tom');
console.log(a.name);//Tom
a.name = 'Jack';//报错：无法分配到 "name" ，因为它是只读属性。ts(2540)
```

## 二九、类06

抽象类

`abstract`关键字用来定义抽象类和其中的抽象方法。

什么是抽象类？

首先，抽象类是不允许被实例化的：

```typescript
abstract class Animal{
    //public name:string;//这报错：属性“name”没有初始化表达式，且未在构造函数中明确赋值。
    public name:any;
    public constrcutor(name:string){
        this.name = name;
    }
    public abstract sayHi():void;
}

let a = new Animal('Tom');
//报错：无法创建抽象类的实例。ts(2511)
```

上面例子中，我们定义了一个抽象类类`Animal`，并且定义了一个抽象方法 `sayHi`，在实例化抽象类是报错了。

其次，抽象类中的抽象方法必须被子类实现：

```typescript
class Cat extends Animal{
    /**
     * eat
     */
    public eat() {
        console.log('Im eating');
    }
}
//报错：非抽象类“Cat”不会实现继承自“Animal”类的抽象成员“sayHi”。ts(2515)
```

正确的抽象类例子：

```typescript
abstract class Animal{
    public name: any;
    public constrcutor(name:string): void{
        this.name = name;
    }
    public abstract sayHi():void;
}

class Cat extends Animal{
    public sayHi(): void {
        console.log(`This is Cat ${ this.name}`);
    }
}
let a = new Cat('Tom');
```

> 上面例子为原视频的例子，却报错：应有 0 个参数，但获得 1 个。ts(2554)
> 
> 找了一些资料还没找到原因，后续再来更

## 三十、类与接口

#### 类继承接口

实现(implements)是面向对象的一个重要概念。一般来说，一个类只能继承自另一个类，有时候不用类之间可以有一些共有的特性，这时候就可以把特性提取成接口(interfaces)，用`implements`关键字来实现，这个特性大大提高了面向对象的灵活性。

举例来说，门是一个类，防盗门是门的子类。如果防盗门有一个报警器的功能，我们可以简单的给防盗门添加一个报警方法，这时候如果有另一个类：车，也有报警器的功能，就可以考虑把报警器提取出来，作为一个接口，防盗门，和车都去实现它：

```typescript
interface Alert{
    alert():void;
}
class Door{
}

class SecurityDoor extends Door implements Alert{
    alert(){
        console.log('SecurityDoor alert');
    }
}
class car implements Alert{
    alert(){
        console.log('Car alert');
    }
}    
```

一个类可以实现多个接口：

```typescript
interface Alert{
    alert():void;
}
interface Light{
    lightOn():void;
    lightOff():void;
}

class Car implements Alert, Light{
    alert(){
        console.log('Car alert');
    }
    lightOff(){
         console.log('Light Off');
    }
    lightOn(){
         console.log('Light On');
    }
}
```

上述例子中，`Car` 实现了 `Alert` 和 `Light`接口，既能报警，也能开关灯。

#### 接口继承接口

接口和接口之间可以是继承关系：

```typescript
interface Alert{
    alert():void;
}
interface LightableAlert extends Alert{
    lightOn():void;
    lightOff():void;
}
```

这很好理解，`LightableAlert` 继承了 `Alert` ，除了拥有`alert`方法之外，还可以拥有自己定义的两个新方法 `lighton`和`lightoff`。

#### 接口继承类

常见的面向对象语言中，**接口是不能继承类的，但是在TypeScript中是可以的**：

```typescript
class Point{
    x:number;
    y:number;
    constructor(x:number,y:number){
        this.x = x;
        this.y = y;
    }
}

interface Point3d extends Point{
    z:number;
}

let point3d:Point3d = {x:1,y:2,z:3};
```

**但在这里不推荐这样使用，我们在定义接口的时候只做定义，具体实现交给实现接口的类去完成**。

## 三一、泛型01

泛型(Generics)是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候在指定类型的一种特性。

首先，我们来实现一个函数 `createArray`，它可以创建一个指定长度的数组，同时将每一项都填充一个默认值。：

```typescript
function creatArray(length:number,value:any): Array<any>{
    let result = [];
    for(let i = 0; i < length; i++){
        result[i] = value;
    }
    return result;
}

creatArray(3,'x');//['x','x','x']
```

上例中，我们使用了之前提到过的数组泛型来定义返回值的类型。

这段代码编译不会报错，但是一个显而易见的缺陷是，他并没有准确的定义返回值的类型；

`Array<any>`允许数组的每一项都为任意类型。但是我们预期的是，数组中每一项都应该是输入的`value`的类型。

这时候，泛型就派上用场：

```typescript
function createArray<T>(length: number, value: T):Array<T>{
    let result: T[] = [];
    for(let i = 0; i<length; i++){
        result[i] = value;
    }
    return result
}
creatArray<string>(3,'x');//['x','x','x']
```

上例中，我们在函数名后添加了`<T>`，其中`T` 用来指代任意输入的类型，在后面的输入`value:T` 和输出 `Array<T>` 中即可使用了。接着在调用的时候，可以指定他的具体的类型为`string` 型。当然，也可以不手动指定，而让类型推论自动推算出来：

```typescript
function createArray<T>(length: number, value: T):Array<T>{
    let result: T[] = [];
    for(let i = 0; i<length; i++){
        result[i] = value;
    }
    return result
}
//不指定类型，通过类型推断来自动推断出类型 
creatArray(3,'x');//['x','x','x']
```

## 三二、泛型02

多个类型参数

定义泛型的时候，可以一次定义多个类型参数：

```typescript
function swap<T, U>(tuple:[T, U]): [U, T]{
    return [tuple[1],tuple[0]];
}
swap([7,'seven']);//['seven', 7]
```

上例中，我们定义了一个 `swap` 函数，用来交换输入的元组

## 三三、泛型03

泛型约束

在函数内部使用泛型变量的时候，由于事先不知道他是哪种类型，所以不能随意的操作它的属性或方法：

```typescript
function loggingIdentity<T>(arg: T): T{
    console.log(arg.length);
    return arg;
}
//报错：类型“T”上不存在属性“length”。ts(2339)
```

上例中，泛型 `T` 不一定包含属性 `length` ，所以编译的时候报错了。

这时，我们可以对泛型进行约束，致允熙这个函数传入那些包含 `length` 属性的变量，这就是泛型约束：

```typescript
interface LengthWise{
    length: number;
}
function loggingIdentity<T extends LengthWise>(arg: T): T{
    console.log(arg.length);
    return arg;
}
```

上例中，我们使用了 `extends` 约束了泛型 `T` 必须符合接口 `LengthWise` 的形状，也就是必须包含 `length` 属性。

此时如果调用 `loggingIdentity` 的时候，传入的 `arg` 不包含 `length` ，则会在编译时报错：

```typescript
interface LengthWise{
    length: number;
}
function loggingIdentity<T extends LengthWise>(arg: T): T{
    console.log(arg.length);
    return arg;
}
loggingIdentity('1111');
// 4
loggingIdentity(8);
//报错：类型“number”的参数不能赋给类型“LengthWise”的参数。ts(2345)
```

## 三四、泛型04

泛型接口

之前学习过，可以使用接口的方式来定义一个函数需要符合的形状：

```typescript
interface SearchFunc{
    (source: string, subString: string): boolean;   
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string){
    return source.search(subString) != -1;
}
```

当然也可以使用含有泛型的接口来定义函数的形状：

```typescript
interface CreateArrayFunc{
    <T>(length: number, value: T): Array<T>;
}
let createArray: CreateArrayFunc;
createArray = function<T>(length: number, value: T): Array<T>{
    let result:T[] = [];
    for(let i = 0; i < length; i++){
        result[i] = value;
    }
    return result;
}

createArray(3,'x');
//['x','x','x']
```

进一步，我们可以把泛型参数提前到接口名上：

```typescript
interface CreateArrayFunc<T>{
    (length: number, value: T): Array<T>;
}

let createArray: CreateArrayFunc<any>;
createArray = function<T>(length: number, value: T): Array<T>{
    let result:T[] = [];
    for(let i = 0; i < length; i++){
        result[i] = value;
    }
    return result;
}

createArray(3,'x');
//['x','x','x']
createArray(3,true);
//[true,true,true]
createArray(3,{isExist:true});
//[{isExist:true},{isExist:true},{isExist:true}]
```

## 三五、泛型05

泛型类

与泛型接口类似，泛型也可以用于类的类型定义中：

```typescript
class GenericNumber<T>{
    zeroValue!: T; //!为非空断言, 否则报错：属性“XXX”没有初始化表达式，且未在构造函数中明确赋值。
    add!: (x: T, y: T) => T;
}
let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x,y){ return x+y; };
```

泛型参数的默认类型

在TypeScript2.3以后，我们可以为泛型中的类型参数指定默认类型。当使用泛型时没有在代码中直接指定类型参数，从实际值参数中也无法推测出时，这个默认类型就会起作用。

```typescript
function createArray<T = string>(length: number, value: T):Array<T>{
    let result:T[] = [];
    for(let i = 0; i < length; i++){
        result[i] = value;
    }
    return result;
}
```

## 三六、声明合并

同名函数、接口、类的合并

如果定义了两个相同的名字的函数、接口或类，那么他们会合并成一个类型：

#### 函数的合并

之前我们学习过，我们可以使用重载定义多个函数类型：

```typescript
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number  string): number  string{
    if (typeof x === 'number'){
        return Number(x.toString().split('').reverse.join(''));
    } else if( typeof x === 'string'){
        return x.split('').reverse.join('');
    }
}
```

#### 接口的合并

接口中的属性在合并时会简单地合并到一个接口中：

```typescript
interface Alarm{
    price: number;
}
interface Alarm{
    weight: number;
}
```

相当于：

```typescript
interface Alarm{
    price: number;
    weight: number;
}
```

注意，**合并的属性的类型必须是唯一的**：

```typescript
interface Alarm{
    price: number;
}
interface Alarm{
    price: number;
    //虽然重复了，但是类型还是number，所以不会报错
    weight: number;
}
```

```typescript
interface Alarm{
    price: number;
}
interface Alarm{
    price: string;
    //报错：后续属性声明必须属于同一类型。属性“price”的类型必须为“number”，但此处却为类型“string”。ts(2717)
    weight: number;
}
```

接口中的方法合并，和函数的合并一样：

```typescript
interface Alarm{
    price: number;
    alert(s: string): string;
}
interface Alarm{
    weight: number;
    alert(s: string, n: number): string;
}
```

相当于：

```typescript
interface Alarm{
    price: number;
    weight: number;
    alert(s: string): string;
    alert(s: string, n: number): string;
}
```

#### 类的合并

类的合并与借口的合并一样

PS: 但是一般情况下，不建议创建多个同名接口或类，虽然可以自动合并，但是可能会发生意想不到的问题。代码不要写在两个地方，不然不好维护。

## 三七、写在结尾

TypeScript 应用非常广泛，最新的 Vue 和 React 均集成了 TypeScript ，这里推荐大家使用 Vue3 ，Vue3 天然支持 TypeScript。

另一方面，TS 中有很多支持 ES 的语法，关系图：

![image-20220507164550922](https://img-blog.csdnimg.cn/img_convert/bb541a521a9a8d8574a7711ec1caf1a2.png)

最后，多看文档

[TypeScript英文文档](https://www.typescriptlang.org/docs/)

[TypeScript中文文档](https://www.tslang.cn/docs/home.html)