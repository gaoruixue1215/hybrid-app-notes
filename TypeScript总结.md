# TypeScript 总结

## 1、项目搭建

- 使用 TypeScript 模板创建新项目

```
react-native init MyApp --template react-native-template-typescript
```

- 现有项目添加 TypeScript

  1. ```
     yarn add --dev typescript @types/jest @types/react @types/react-native @types/react-test-renderer
     ```

  2. 在项目根目录下创建一个 TypeScript 配置文件（**tsconfig.json**）

     ```
     {
       "compilerOptions": {
         "allowJs": true,
         "allowSyntheticDefaultImports": true,
         "esModuleInterop": true,
         "isolatedModules": true,
         "jsx": "react",
         "lib": ["es6"],
         "moduleResolution": "node",
         "noEmit": true,
         "strict": true,
         "target": "esnext"
       },
       "exclude": [
         "node_modules",
         "babel.config.js",
         "metro.config.js",
         "jest.config.js"
       ]
     }
     ```

  3. 在项目根目录创建一个**jest.config.js**文件

     ```
     module.exports = {
       preset: 'react-native',
       moduleFileExtensions: ['ts', 'tsx', 'js', 'jsx', 'json', 'node'],
     };
     ```

  4. 重命名一个 JavaScript 文件为 ***.tsx**

## 2、TypeScript 语法

### （1）基础类型
#### number
>数字类型
```tsx
let num: number = 100;
```
#### string
>字符串类型
```tsx
let name: string = "Runoob";
```
#### boolean
>布尔类型
```tsx
let isDone: boolean = false;
```

#### Array<number>（number[ ]）
>数组类型
```tsx
//两种方式可以定义数组
//1、以在元素类型后面接上[]，表示由此类型元素组成的一个数组
let list: number[] = [1, 2, 3];
//2、使用数组泛型，Array<元素类型>
let list: Array<number> = [1, 2, 3];
```

#### object
>对象类型
```tsx
//let obj:object={name:'zhangsan'}
let obj:{name:string}={name:'zhangsan'}
```

#### Tuple（元组 ）
>元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。
```tsx
let tupleArr:[string,number,boolean]=['ss',2,true];
```

#### enum（枚举）
>枚举类型
```tsx
//默认情况下，从0开始为元素编号,顺延 也可以手动的指定成员的数值,或者，全部都采用手动赋值
enum Lev {one=100,two,three};
```

#### any
>任意类型，声明为 any 的变量可以赋予任意类型的值。
```tsx
let a:any = 'any type'
```

#### void
>用于标识方法返回值的类型，表示该方法没有返回值。
```tsx
function hello(): void {
    alert("Hello Runoob");
}
```

#### null
>某种程度上来说，void类型像是与any类型相反，它表示没有任何类型。 

#### undefined
```tsx
let u: undefined = undefined;
```


### （2）接口

> interface ：描述一个对象的取值规范，不实现具体的对象

- 属性接口

```tsx
interface User {
  name: string;// 必选属性
  age?: number;//? 可选属性，表示不是必须的参数
  [propName:string]:any//属性不确定，可有可无,任意类型
}

const user1: User = {
  name: "lili",
  age: 18, //可以不写
  job:'程序员'//可以不写
};
```

- 函数接口

```tsx
interface SearchFunc {
  (source: string, subString: string): boolean;
}

let myFunc: SearchFunc = function(source: string, subString: string){////形参名字可以不一样,类型必须一样，比如可以写sub:string
    ...
    return true;
}
```

- 类接口

```tsx
interface User {
  name: string;
  age?: number;
}
  
class user1 implements User{
    name='zhangsan'  
}
```

- 继承
> 接口继承接口
```tsx
interface Shape {
    color: string;
}

interface Square extends Shape {
    sideLength: number;
}

let square = {} as Square;
square.color = "blue";
square.sideLength = 10;
```
>一个接口可以继承多个接口，创建出多个接口的合成接口。
```tsx
interface Shape {
    color: string;
}
interface PenStroke {
    penWidth: number;
}
interface Square extends Shape, PenStroke {
    sideLength: number;
}

let square = {} as Square;
square.color = "blue";
square.sideLength = 10;
square.penWidth = 5.0;
```
#### TypeScript 在 React Naitve 中的应用示例一
```tsx
const App = ()=>{
	return <View>
		<Demo1 name='lalala'/>
	</View>
}


interface Props{
    name:string;
    data:{
        id:string,
        title:string
    }
}
interface State{
    title:string;
}

export default class Demo1 extends Component<Props,State> {
    constructor(props:Props){
        super(props);
        this.state={
            title:'typescript'
        }
    }
    componentDidMount(){
        this.setState({title:'kkkksss'})
    }
    render() {
        return (
            <View>
                <Text>{this.props.name}</Text>
                {/* lalala */}
                <Text>{this.state.title}</Text>
                {/* kkkksss */}
            </View>
        )
    }
}
```

### （3）类

- 定义
```tsx
interface User {
  name: string;
  age?: number;
}
  
class user1 implements User{
    name='zhangsan'  
}
```
- 继承
```tsx
interface User2 extends User1{//接口继承类
    job:string
}
let user:User2={
    name:'zhangsan',
    age:20,
    job:'程序员'
}
```
- 静态属性和静态方法（static）
> 创建类的静态属性，这些属性存在于类本身上面而不是类的实例
```tsx
class Person{
    name:string;
    age:number;
    constructor(name:string,age:number){
        this.name=name;
        this.age=age;
    }
}
class Worker extends Person{
    //静态属性
    static money:number;
    private job:string='程序员';
    constructor(name:string,age:number,job:string){
        super(name,age);
        this.job=job
    }
}
Worker.money=1000;
let user1 = new Worker('zhang',18,'程序员');
console.log(user1);//{"age": 18, "job": "程序员", "name": "zhang"}
console.log(Worker.money)//1000
```
- 访问修饰符
  - public：在TypeScript里，成员都默认为public。
  - private：当成员被标记成private时，它就不能在声明它的类的外部访问
  - protected：成员在派生类中仍然可以访问，构造函数也可以被标记成protected。 这意味着这个类不能在包含它的类外被实例化，但是能被继承。

### （4）泛型

> 泛型（Generics）是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性。
>
> 使用泛型来创建可重用的组件，一个组件可以支持多种类型的数据

- 泛型函数

```tsx
//类型变量T，它是一种特殊的变量，只用于表示类型而不是值。
//这个函数会返回任何传入它的值，使返回值的类型与传入参数的类型是相同的
function identity<T>(arg:T):T{
    return arg;
}
console.log(identity<string>('params'))//params

function getMsg<U>(msg:U[]):U[]{
    return msg;
}
var arr = [1,2,3,4,5]
console.log(getMsg(arr))//[1, 2, 3, 4, 5]
```

- 泛型接口

```tsx
interface GenericIdentityFn<T> {
    (arg: T): T;
}
let myIdentity: GenericIdentityFn<number> = function(arg){
    return 100
};
console.log(myIdentity(100))//100
```

- 泛型类

```tsx
class AddData<T>{
    list:T[] = [];
    add(data:T):T[]{
        this.list.push(data);
        return this.list;
    }
}
let data1 = new AddData<string>()
data1.list.push('泛型类')
console.log(data1)//{"list": ["泛型类"]}
```

### （5）装饰器

> *装饰器*是一种特殊类型的声明，它能够被附加到类声明、方法、属性或参数上。 装饰器使用`@expression`这种形式，`expression`求值后必须为一个函数，它会在运行时被调用，被装饰的声明信息做为参数传入。
> 装饰器其实就是一个函数，在函数里可以写一些新的逻辑,包裹后面修饰的内容，将新的逻辑传递到被修饰的内容中去
>高阶组件--其实就是一个函数，就是装饰器

#### 配置

```
yarn add -D @babel/plugin-proposal-decorators

//添加配置
// 创建 .babelrc 文件
{
    "presets": ["module:metro-react-native-babel-preset"],
    "plugins":[["@babel/plugin-proposal-decorators", { "legacy": true }]]
}

// tsconfig.json中添加 "experimentalDecorators": true
{
    "compilerOptions": {
        "target": "ES5",
        "experimentalDecorators": true
    }
}
```

#### 定义

```tsx
// 普通装饰器（无参数）
function helloWord(target: any) {
    console.log('hello Word!');
}
@helloWord
class HelloWordClass {

}
// 装饰器工厂 （带参数）
function helloWord(p:string) {
    return function (target) {
         console.log(p)
    }
}
@helloWord('hello')
class HelloWordClass {

}
//装饰器工厂 （带参数）
function addUrl(url:string){
    return function(target:any){ 
        target.prototype.url = url;
    }
}
@addUrl('http://www.baidu.com')
class HomeServer{
    url:string|undefined;
    getData(){
        console.log(this.url)
    }
}
let home = new HomeServer();
home.getData();
```

#### 分类

- 类装饰器

  > 参数是类的构造函数
  >
  > 如果类装饰器返回一个值，它会使用提供的构造函数来替换类的声明

- 方法装饰器

  > 方法装饰器表达式会在运行时当作函数被调用，传入下列3个参数：
  >
  > 1. 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
  > 2. 成员的名字。
  > 3. 成员的属性描述符。

  ```tsx
  function enumerable(value: boolean) {
    return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        descriptor.enumerable = value;
    };
  }
  class Greeter {
      greeting: string;
      constructor(message: string) {
          this.greeting = message;
      }

      @enumerable(false)
      greet() {
          return "Hello, " + this.greeting;
      }
  }
  console.log(new Greeter('Danny').greet());//Hello, Danny
  ```

- 属性装饰器

  > 属性装饰器表达式会在运行时当作函数被调用，传入下列2个参数：
  >
  > 1、对于静态成员来说是类的构造函数，对于实例成员是类的原型对象
  >
  > 2、成员的名字

- 参数装饰器

  > 参数装饰器表达式会在运行时当作函数被调用，传入下列3个参数：
  >
  > 1. 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
  > 2. 成员的名字。
  > 3. 参数在函数参数列表中的索引。

#### 装饰器组合

> 当多个装饰器应用在一个声明上时会进行如下步骤的操作：
>
> 1. 由上至下依次对装饰器表达式求值。
> 2. 求值的结果会被当作函数，由下至上依次调用

#### TypeScript 在 React Naitve 中的应用示例二
```tsx
import React, { Component } from 'react'
import { Text, View } from 'react-native'
function setStatusBar(color:string){
    return function(WrapComponent:any){
        return class extends Component{
            render(){
                return (
                    <>
                        <View style={{height:30,backgroundColor:color}}>
                        </View>
                        <WrapComponent />
                    </>
                )
            }
        }
    }
}
@setStatusBar('red')
export default class ListItem extends Component {
    render() {
        return (
            <View>
                <Text>装饰器</Text>
            </View>
        )
    }
}
```
