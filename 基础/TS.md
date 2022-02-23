### 一、你觉得使用ts的好处是什么?
- TypeScript是JavaScript的加强版，它给JavaScript添加了可选的静态类型和基于类的面向对象编程，它拓展了JavaScript的语法。
- Typescript 是纯面向对象的编程语言，包含类和接口的概念.
- TS 在开发时就能给出编译错误， 而 JS 错误则需要在运行时才能暴露。
- 作为强类型语言，你可以明确知道数据的类型。代码可读性极强。
- ts中有很多很方便的特性, 比如可选链.
### 二、type 和 interface的异同
用interface描述数据结构，用type描述类型
```
type 可以声明基本类型别名，联合类型，元组等类型
// 基本类型别名
type Name = string

// 联合类型
interface Dog {
    wong();
}
interface Cat {
    miao();
}

type Pet = Dog | Cat

// 具体定义数组每个位置的类型
type PetList = [Dog, Pet]

// 当你想获取一个变量的类型时，使用 typeof
let div = document.createElement('div');
type B = typeof div

// type 支持类型映射
type Keys = "firstname" | "surname"

type DudeType = {
  [key in Keys]: string
}

const test: DudeType = {
  firstname: "Pawel",
  surname: "Grzybek"
}
```
都可以描述一个对象或者函数
```
interface User {
  name: string
  age: number
}

interface SetUser {
  (name: string, age: number): void;
}

type User = {
  name: string
  age: number
};

type SetUser = (name: string, age: number)=> void;
```
都允许拓展（extends）并且两者并不是相互独立的，也就是说 interface 可以 extends type, type 也可以 extends interface 。
```
// interface extends interface
interface Name { 
  name: string; 
}
interface User extends Name { 
  age: number; 
}

// type extends type
type Name = { 
  name: string; 
}
type User = Name & { age: number  };

// interface extends type
type Name = { 
  name: string; 
}
interface User extends Name { 
  age: number; 
}

// type extends interface
interface Name { 
  name: string; 
}
type User = Name & { 
  age: number; 
}
```
三、如何基于一个已有类型, 扩展出一个大部分内容相似, 但是有部分区别的类型?

通过Pick和Omit
```
interface Test {
    name: string;
    sex: number;
    height: string;
}

type Sex = Pick<Test, 'sex'>;

const a: Sex = { sex: 1 };

type WithoutSex = Omit<Test, 'sex'>;

const b: WithoutSex = { name: '1111', height: 'sss' };
```
Partial, Required
通过泛型

