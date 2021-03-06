# 变量类型、定义及操作

> https://www.typescriptlang.org/docs/handbook/2/everyday-types.html

## 01. 类型定义

### 原语类型 primitives types

最小数据类型

```ts
let user: string = "zhangsan" // 字符串
let age: number = 18 // 数字
let gender: boolean = true  // 布尔值

console.log("simple types::: ", gender)
```

### 复合类型 compound type

通过 **原语类型** 组合出来的复合类型

#### 数组: `Array`

1. Array: 类型固定， 长度不固定
2. 虽然 `users[3]` 超出边界但是不会提示错误, 其结果为 undeined
3. `Array<string>` 这种方法不建议使用, `tslint` 会提示错误

```ts
// declare
let users: Array<string> = ["zhangsan", "lisi", "wangwu"]
let ages: number[] = [18, 29, 30]
// usage
console.log("array::: ", users[3])
```

#### 元组: `Tuple`

1. Tuple: 类型固定， 长度固定。 且必须 **一一对应**
2. `userInfo[3]` 超出边界时， 编译器将提示错误。

```ts
let userInfo: [string, number, boolean] = ["zhangsan", 10, true]
// usage
console.log("tuple::: ", userInfo[2])
```

#### 枚举: `Enums`

> 官网: 了解即可 https://www.typescriptlang.org/docs/handbook/enums.html

1. 默认从0开始: Male
2. 可以在任意位置设置新值 : Female
3. 下一个为申明值的字段 +1 : Unknown
4. 枚举值类型可以不一致, 但不要这样做。 : Undefined

```ts
enum Gender {
    Male, // 0 
    Female = 10, //10
    Unknown, // 11
    Undefined = "undefined", // undefined
}
enum Colors {
    Red = "red",
    Green = "green",
}

// usage
let myGender: Gender = Gender.Male
if (myGender === Gender.Male) {
    console.log("hey man")
}
// 虽然值类型不一样，但是可以比较， 因为他们都是 Enums 的元素。
if (myGender === Gender.Undefined) {
    console.log("myGender::: check true")
} else {
    console.log("myGender::: check failed")
}
```

#### 对象对象: `Object Types`

1. 声明类型的时候，有 `=` 号。
2. 可以使用 `?` 表示为 **可选字段**
    + 实例化时如果没有赋值， 则默认值为 `undefined`

```ts
// 注意这里有 = 
type Student = {
    Name: string
    Age?: number // ? 表示可以省略
}

// usage
let stu01: Student = {
    Name: "zhangsan",
    Age: 18
}
let stu02: Student = {
    Name: "wangwu"
    // 注意这里: stu02 中 age 未赋值， 默认值为: undefined。

}

console.log("object::: stu01 =>", stu01)
console.log("object::: stu02 =>", stu02)

```

#### 接口类型: `interface`

> 官方描述: 
> An interface declaration is another way to name an object type


1. 生命接口时， 没有 `=`
2. 可以使用 `?` 表示为 **可选字段**

```ts
// 定义对象
type Teacher = {
    Name: string
    Salary: number
}

// 定义接口
// 注意没有 = 号
interface Person {
    Name: string
    Age?: number // ? 表示可以省略
}
```

与 `golang` 中的 `interface` 类似， `interface` 是 `object` 的抽象集合。 符合 `interface` 字段的 `object` 实例， 就可以赋值给 `interface` 实例的

[接口与类型别名的差异](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces)

```ts
let p1: Person = stu01
let p2: Person = teacher01

console.log("interface::: p1(stu01) =>", p1)
console.log("interface::: p2(teacher01) =>", p2)
```

虽然 `Teacher(object)` 中有 `Salary` 没有 `Age`， 但是由于  `Person(interface)` 中 `Age` 是可选字段， 因此 `Teacher` 就满足 `Person` 的接口抽象。

#### 联合类型: `Union Types`

1. 联合类型， 使用 `|` 联合一个或多个类型
2. **仅能** 使用 **所有类型** 都具有的方法。 **类型收窄(`narrowing`)**
3. 可以使用 `typeof` 进行类型判断 `if (typeof id === "string") { ... }`

> [使用联合类型注意事项](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#working-with-union-types)

```ts
// Teacher 之前定义的 object
// Person: 之前定义的 interface
type UnionType = string | number | null | Teacher | Person

let utString: UnionType = "zhangsan"
let utNumber: UnionType = 18
let utNull: UnionType = null
let utTeacher: UnionType = { Name: "zhangsan", Salary: 10 }
let utPerson: UnionType = { Name: "wangwu" } // interface

console.log("union-type::: utString =>", utString)
console.log("union-type::: utNull =>", utNull)
console.log("union-type::: utPerson =>", utPerson)
console.log("union-type::: utTeacher =>", utTeacher)
```



### 其他类型

#### 任意类型: `any`


非数据类型， 但有特殊含义的类型

#### 不存在类型: `never`

#### 空类型: `void`
#### `null` / `undefined`

1. 就是 **任意类型** 的意思
2. 虽然好用，但是尽量不要用。 除非明确知道需要自己在做什么
3. 编译器将跳过对 `any` 类型的检查
    + `tsconfig.json` 中打开 `noImplicitAny` 后， `any` 将会提示错误。

```ts
let obj: any = { x: 0 };
// None of the following lines of code will throw compiler errors.
// Using `any` disables all further type checking, and it is assumed 
// you know the environment better than TypeScript.
obj.foo();
obj();
obj.bar = 100;
obj = "hello";
const n: number = obj;
```

## 02. 类型的行为

### 类型别名: `type alias`

为一个 **已知类型** 或 **匿名类型** 设置一个 **名字**

1. 使用 type 关键字声明
2. 使用 = 连接具体类型
3. 就是取别名， 仅仅是取别名。 同一个类型的不同别名之间可以直接赋值

```ts
type MyString = string
type MyUnion = string | number
type MyInerface = {
    Salary: number
}
type MyObject = {
    Name: MyString
    Salary: MyInerface
}

let myName: MyString = "zhangsan"
console.log("type alias::: myName =>", myName)

// (伪)不同类型可以赋值
type MyAnotherString = string
let myAnotherName = "zhuge"
myName = myAnotherName
console.log("type alias::: myName(<- myAnotherName) =>", myName)
```

### 类型断言 `type assertions`

**类型断言** 是将一个类型 转换 成另一个类型。

1. **`可以`** 将 **模糊类型** 转换为 **精确类型** : `any -> number`
2. **`可以`** 将 **精确类型** 转换为 **模糊类型** : `number -> any`
3. **`不可以`** 将 **精确类型** 转为为 **精确类型** : `number -> string`
    + 编译器会提示错误， 但 **依旧会** 正常编译

```ts
// 类型断言

// 模糊转精确, 字面类型一致
// any -> number
let a: any = 100
let a1 = a as number
console.log("type assertion::: a1 =>", a1)

// 模糊转精确， 字面类型不一致
// any -> number
let s: any = "zhangsan"
let s1 = a as number
console.log("type assertion::: s1 =>", s1)

// 精确转模糊
// number -> any
// s1 是 number
let s2 = s1 as any

// 精确转精确
let num: number = 100
let str1 = num as string // 错误
```

### 类型注释 `type annotation`

> https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-annotations-on-variables

1. 使用 `var: type` 对变量进行注解
2. 函数声明
    1. 传参部分 **定义方式相同**
    2. 返回结果 **直接定义类型** 。

```ts
// 类型注解

// 变量声明
let yourName: string = "zhangsan"
const yourAge: number = 100


// 函数
function getAge(name: string): (number) {
    console.log("type annotation -> funcion::: name =>", name)
    return 26
}

let hisAge, hisName = getAge(yourName)
console.log("type annotation -> call funcion ::: hisAge =>", hisAge)
```