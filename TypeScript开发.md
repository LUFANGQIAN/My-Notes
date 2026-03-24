

# TypeScript开发

## 编译

要将 `.ts` 文件编译为 `.js` 文件，需要配置 TypeScript 的编译环境

编译有两种解决方案

首先是命令行编译 使用tsc指令

其次是自动编译



### 命令行编译

#### **第一步：创建一个 `demo.ts` 文件**

示例代码：

```typescript
const person = {
  name: '李四',
  age: 18
}
console.log(`我叫${person.name}，我今年${person.age}岁了`)
```

#### **第二步：全局安装 TypeScript**

执行以下命令：

```bash
npm i typescript -g
```

#### **第三步：使用命令编译 `.ts` 文件**

运行以下命令进行编译：

```bash
tsc demo.ts
```

------

✅ **说明**：

- `tsc` 是 TypeScript 编译器的命令。
- 编译成功后会生成对应的 `demo.js` 文件。
- 此方法适用于简单的 TypeScript 文件编译，适合学习和测试用途。

------

> 💡 提示：若需更复杂的配置（如模块、输出路径等），建议使用 `tsconfig.json` 配置文件。





### 自动化编译

实现 TypeScript 文件的自动化编译，即当 `.ts` 文件发生变化时，自动将其编译为 `.js` 文件。步骤如下：

#### **第一步：创建 TypeScript 编译控制文件**

执行命令：

```bash
tsc --init
```

- 该命令会在项目根目录生成一个 `tsconfig.json` 配置文件。
- 该文件包含编译时的各种配置项（如目标版本、模块系统、输出路径等）。
- 默认情况下，编译目标（`target`）是 ES7，可根据需要手动修改为其他版本（如 ES6、ES2020 等）。

------

#### **第二步：监视目录中的 `.ts` 文件变化**

执行命令：

```bash
tsc --watch
```

- 启动监听模式，实时监控项目中所有 `.ts` 文件的变化。
- 当检测到 `.ts` 文件被修改并保存后，会自动触发编译，生成对应的 `.js` 文件。

------

#### **第三步：小优化 —— 出错时不生成 `.js` 文件**

使用以下命令：

```bash
tsc --noEmitOnError --watch
```

- 添加 `--noEmitOnError` 参数，表示：**如果编译过程中出现错误，则不生成 `.js` 文件**。
- 这样可以避免在代码有语法或类型错误时，仍生成不可靠的 JavaScript 文件。
- ✅ 提示：也可以通过修改 `tsconfig.json` 中的 `noEmitOnError` 配置项来实现相同效果。

> **备注**：
> 在 `tsconfig.json` 中添加：
>
> ```json
> "compilerOptions": {
>   "noEmitOnError": true
> }
> ```
>
> 即可达到与命令行参数一致的效果。

------

### ✅ 总结

| 步骤 | 操作                          | 作用                             |
| ---- | ----------------------------- | -------------------------------- |
| 1    | `tsc --init`                  | 生成 `tsconfig.json` 配置文件    |
| 2    | `tsc --watch`                 | 实时监听并自动编译 `.ts` 文件    |
| 3    | `tsc --noEmitOnError --watch` | 出错时不生成 `.js`，提高代码质量 |

> 💡 建议：开发阶段使用 `--watch` 模式，配合 `--noEmitOnError` 可有效提升开发体验和代码健壮性。



## 类型声明

### 定义

TypeScript的类型说明是通过在变量、函数参数或返回值后使用冒号加类型名（如`let name: string = "Alice";`）来定义类型，用于在编译时进行类型检查，避免运行时错误，提升代码可靠性和可维护性。



### 食用方法

#### 变量声明

语法：`变量名:类型 = 赋值`



#### 函数声明

语法：`function 函数名(参数名: 参数类型): 返回值类型 {函数体}`



#### 字面量声明

语法：`变量名: '指定具体值' = '指定具体值'`

> 指定具体的值不可改变



#### 例子

```ts
// 变量类型声明
let name: string = "Alice";

// 函数参数和返回类型
function greet(person: string): string {
    return "Hello, " + person;
}

// 字面量类型声明（指定具体值）
let color: 'red' = 'red';  // 只能赋值为 'red'
```



### 类型详解

#### unknown 类型

##### 定义

`unknown` 是 TypeScript 中的**安全容器类型**，表示"未知类型"。它允许任何类型赋值给它，但**必须经过类型检查或类型断言后才能使用**，是替代 `any` 的安全选择。

> **与 any 的核心区别**：  
>
> - `any`：可以**直接操作**（编译不报错，运行时可能崩溃）  
> - `unknown`：**必须先进行类型检查/断言**才能操作（类型安全）

##### 食用方法

| 操作           | unknown                      | any                      |
| -------------- | ---------------------------- | ------------------------ |
| 赋值           | ✅ 所有类型可以赋值给 unknown | ✅ 所有类型可以赋值给 any |
| 读取属性       | ❌ 必须断言或类型检查         | ✅ 直接操作               |
| 赋值给其他类型 | ❌ 不能直接赋值               | ✅ 可以赋值给任何类型     |

##### 例子

```ts
// 安全接收外部数据（API 响应）
const response: unknown = { name: "Alice", age: 30 };

// ✅ 正确：类型检查后操作
if (typeof response === "object" && response !== null) {
  console.log((response as { name: string }).name); // 正确
}

// ❌ 错误：未进行类型检查
// console.log(response.name); // 类型错误

// ✅ 正确：类型断言
const user = response as { name: string; age: number };
console.log(user.name);

// ❌ 错误：不能直接赋值给 string
// const name: string = response; // 类型错误
```

**提示**

- **优先使用 unknown**：处理不确定数据（API 响应、用户输入）时，用 `unknown` 替代 `any`
- **强制类型检查**：使用 `unknown` 时，必须进行 `typeof` 检查或 `as` 断言
- **与 any 的选择**：
  `any` = 无类型安全，`unknown` = 有类型安全

------

#### never 类型

##### 定义

`never` 是 TypeScript 中的**底端类型**，表示"永远不存在的值"。它用于表示**函数永远不会返回**的情况（如抛出异常、无限循环）。

> **核心特点**：  
>
> - `never` 是**所有类型的子类型**（即任何类型都可以赋值给 `never`）  
> - `never` **不能赋值给任何类型**（除了 `never` 本身）  
> - 用于**类型穷尽检查**（确保处理了所有可能的情况）

##### 食用方法

| 场景           | 说明               | 例子                                                         |
| -------------- | ------------------ | ------------------------------------------------------------ |
| 抛出异常的函数 | 函数永远不会返回   | `function throwError(msg: string): never { throw new Error(msg); }` |
| 无限循环函数   | 函数永远不会结束   | `function infiniteLoop(): never { while(true) { } }`         |
| 类型穷尽检查   | 确保处理了所有情况 | `switch` 语句中处理所有分支后，`default` 的类型为 `never`    |

##### 例子

```ts
// 1. 抛出异常的函数
function throwError(msg: string): never {
  throw new Error(msg);
}

// 2. 无限循环函数
function infiniteLoop(): never {
  while (true) {
    console.log("Looping...");
  }
}

// 3. 类型穷尽检查（经典用法）
type Shape = "circle" | "square" | "triangle";

function getArea(shape: Shape): number {
  switch (shape) {
    case "circle":
      return Math.PI * 2 * 2;
    case "square":
      return 4 * 4;
    // 未处理 "triangle" 的情况
    default:
      const _exhaustiveCheck: never = shape;
      return _exhaustiveCheck; // 类型错误！提示我们添加 "triangle" 分支
  }
}
```

**提示**

- **never 的本质**：表示"不可能发生"的值，所以它不能被赋值给任何类型
- **类型穷尽检查**：在 `switch` 语句中，当处理了所有可能的类型后，`default` 的类型会变成 `never`，如果漏了分支，编译器会报错
- **不常用但重要**：在类型系统中扮演关键角色，特别是用于类型编程和防御性编程

------

#### void 类型

##### 定义

`void` 表示**没有返回值**的类型，通常用于函数的返回类型。它与 `undefined` 有区别，`void` 表示"没有值"，而 `undefined` 表示"未定义的值"。

> **核心特点**：
>
> - 函数没有返回值时，返回类型为 `void`
> - `void` 变量只能赋值为 `undefined`（但通常我们不直接赋值）
> - `void` 不能赋值给其他类型（除了 `undefined` 和 `any`）

##### 食用方法

| 操作           | void                           | undefined                         |
| -------------- | ------------------------------ | --------------------------------- |
| 函数返回       | ✅ 无返回值函数                 | ❌ 无返回值函数应返回 void         |
| 变量赋值       | ✅ `let v: void = undefined;`   | ✅ `let u: undefined = undefined;` |
| 赋值给其他类型 | ❌ 不能赋值给 string、number 等 | ❌ 不能赋值给 string、number 等    |

##### 例子

```ts
// 1. 无返回值的函数
function logMessage(message: string): void {
  console.log(message);
  // 不需要 return 语句
}

// 2. void 变量（不常用）
let v: void = undefined; // 正确
// let s: string = v; // 错误：不能将 void 赋值给 string

// 3. 与 undefined 的区别
function getUndefined(): undefined {
  return undefined;
}

// 无返回值的函数默认返回 void
function noReturn(): void {
  // 不返回任何东西
}

// 4. void 与 undefined 的赋值
let u: undefined = undefined;
let v: void = undefined; // 正确

// 但不能反过来
// let u2: undefined = v; // 错误：不能将 void 赋值给 undefined
```

**提示**

- **函数返回类型**：无返回值的函数应明确声明 `void`，而不是不写类型
- **与 undefined 的区别**：`void` 表示"没有值"，`undefined` 表示"未定义的值"
- **变量赋值**：`void` 变量通常只赋值为 `undefined`，但很少需要显式赋值
- **常见使用场景**：处理副作用的函数（如日志、状态修改）的返回类型

------



#### object 类型

##### 定义

在 TypeScript 中，可以定义一个对象类型，并允许其包含任意属性。例如，定义一个 `person` 对象类型如下：

```typescript
let person: {
  name: string;          // 必须属性，类型为 string
  age?: number;         // 可选属性，类型为 number
  [key: string]: any;   // 任意属性，键为 string，值为 any 类型
}
```

##### 核心特点

- **必须属性**：如 `name`，定义时必须提供该属性。
- **可选属性**：如 `age`，定义时可以提供也可以不提供。
- **任意属性**：如 `[key: string]: any`，允许添加任意键值对，键为字符串，值可以是任意类型。

##### 使用方法

| 操作         | 必须属性     | 可选属性       | 任意属性         |
| ------------ | ------------ | -------------- | ---------------- |
| **定义对象** | 必须提供     | 可提供可不提供 | 可任意添加       |
| **访问属性** | 可直接访问   | 可直接访问     | 需类型断言或 any |
| **类型检查** | 类型严格匹配 | 类型可选匹配   | 类型灵活匹配     |

##### 例子

```typescript
// 定义 person 对象类型
let person: {
  name: string;
  age?: number;
  [key: string]: any;
}

// 创建 person 实例
person = { name: 'tom', age: 18, gender: '男' };

// 访问属性
console.log(person.name);   // 'tom'
console.log(person.age);    // 18
console.log(person.gender); // '男'

// 类型检查
// person = { name: 'tom', age: '18' }; // 错误：age 类型应为 number
person = { name: 'tom', job: 'developer' }; // 正确：job 为任意属性
```

##### 提示

- **必须属性**：在对象实例中必须提供，且类型需严格匹配。
- **可选属性**：在对象实例中可提供可不提供，提供时类型需匹配。
- **任意属性**：在对象实例中可任意添加，键为字符串，值类型灵活，但需注意不要与已定义属性冲突。







### 速查表

| **类型名称**   | **说明**                            | **示例**                                                  | **关键规范**                                    |
| -------------- | ----------------------------------- | --------------------------------------------------------- | ----------------------------------------------- |
| **基本类型**   |                                     |                                                           |                                                 |
| `number`       | 数字（含整数/浮点数）               | `let age: number = 25;`                                   | 避免用 `any` 替代                               |
| `string`       | 字符串                              | `let name: string = "Alice";`                             | 优先用模板字符串（`string` 类型）               |
| `boolean`      | 布尔值                              | `let isActive: boolean = true;`                           | 不可赋值 `0`/`1`（需显式转为 `boolean`）        |
| `null`         | 空值（需单独声明）                  | `let n: null = null;`                                     | 与 `undefined` 不同（需 `--strictNullChecks`）  |
| `undefined`    | 未定义（需单独声明）                | `let u: undefined = undefined;`                           | 与 `null` 一起需启用严格模式                    |
| **特殊类型**   |                                     |                                                           |                                                 |
| `void`         | 无返回值（函数）                    | `function log(): void { console.log(); }`                 | 仅用于函数返回，不用于变量                      |
| `never`        | 永不有值（异常/死循环/限制函数   ） | `function error(): never { throw new Error(); }`          | 无法赋值给其他类型（如 `string`）多用在报错终结 |
| `unknown`      | 未知类型（安全替代 `any`）          | `let data: unknown = "hello";`                            | **必须先类型断言**： `(data as string).length`  |
| **复合类型**   |                                     |                                                           |                                                 |
| `object`       | 非原始类型（含 `null`）             | `let obj: object = { name: "A" };`                        | **不推荐**（无法访问属性）→ 用 `Record`/接口    |
| `{} `          | 空对象（非 `object`）               | `let empty: {} = {};`                                     | 无法赋值 `null`/`undefined`                     |
| `Array<T>`     | 数组（泛型）                        | `let nums: number[] = [1, 2];`                            | 优先用 `number[]` 而非 `Array<number>`          |
| `Tuple`        | 元组（固定长度+类型）               | `let point: [number, number] = [1, 2];`                   | 用于明确顺序的结构化数据                        |
| `Union`        | 联合类型（多种可能）                | `let id: string                                           | number = "123";`                                |
| `Intersection` | 交叉类型（同时满足多个类型）        | `type Extended = Base & { extra: string };`               | 用 `&` 合并，如 `interface A & B`               |
| **字面量类型** |                                     |                                                           |                                                 |
| `Literal`      | 字面量约束（值固定）                | `let color: "red"                                         | "blue" = "red";`                                |
| `typeof`       | 取变量类型                          | `const user = { id: 1 };` `type UserId = typeof user.id;` | 从变量/函数推导类型（**推荐**）                 |
| `keyof`        | 取对象键名类型                      | `type Keys = keyof { name: string; age: number };`        | 得到 `name                                      |



## 类型推断

### 定义

TypeScript的类型推断是编译器在**没有显式类型注解**的情况下，根据初始化值或上下文**自动推断变量、函数参数或返回值类型**的能力。它解决了显式类型声明的冗余问题，使代码更简洁，同时在编译时提供类型安全检查，避免运行时错误，提升开发效率和代码可维护性。

> **面试题解答**：
> *问：TS的类型推断是什么？它解决了什么问题？*
> *答：类型推断是TS编译器自动推断类型的能力，无需手动写类型注解。它解决了代码冗余问题（减少重复的类型声明），同时保持类型安全，让开发者更专注于业务逻辑，而非类型书写。*

------

### 食用方法

#### 变量声明

语法：`变量名 = 初始化值`（**不写类型**，由编译器推断）

#### 函数声明

语法：`function 函数名(参数名) {函数体}`（**不写参数和返回类型**，由上下文推断）

#### 字面量类型

语法：`const 变量名 = '指定具体值'`（**使用 `const` 时推断为字面量类型**，避免 `let` 的宽泛推断）

> **关键区别**：  
>
> - `const` + 字面量 → 推断为**字面量类型**（如 `'red'`）  
> - `let` + 字面量 → 推断为**基础类型**（如 `string`）

------

### 例子

```ts
// 变量类型推断（自动推断为 string）
let name = "Alice";  // 不写 :string，编译器推断

// 函数类型推断（参数和返回类型自动推断）
function greet(person) {
    return "Hello, " + person; // person 推断为 string，返回值推断为 string
}

// 字面量类型推断（使用 const 推断为 'red'）
const color = 'red';  // 类型推断为 'red'（不可变）
// ❌ 不能赋值：color = 'blue'; // 类型错误！
```

> 在 JavaScript 中的这些内置构造函数：Number、String、Boolean ，它们用于创建对应的包装对象，在日常开发时很少使用，在 TypeScript 中也是同理，所以在 TypeScript 中进行类型声明时，通常都是用小写的 number、string、boolean





## 类型断言

### 定义

TypeScript的类型断言（Type Assertion）是开发者**手动指定一个值的类型**的机制，用于覆盖编译器的类型推断。它解决了在类型不明确（如 `unknown`、DOM 操作或动态数据）时，编译器无法正确推断类型的问题，但需谨慎使用，避免绕过类型安全检查导致运行时错误。

> **面试题解答**：
> *问：TS中类型断言是什么？它解决了什么问题？*
> *答：类型断言允许开发者主动告诉编译器“这个值的类型是X”，覆盖自动类型推断。它解决了类型推断不准确（如 API 返回的动态数据）或需要强制类型转换（如 DOM 操作）时的问题，但过度使用可能破坏类型安全。*

------

### 食用方法

#### 语法

1. **尖括号语法**（不推荐在 JSX 中使用）：`<类型>值`  
2. **`as` 语法**（推荐）：`值 as 类型`

> **关键区别**：  
>
> - `as` 语法是 TypeScript 专属，避免与 JSX 冲突  
> - 两种语法等价，但 `as` 是官方推荐写法

#### 使用场景

- 处理 `unknown` 类型（如 API 响应）
- DOM 操作（获取元素）
- 类型推断不准确时（如联合类型）

------

### 例子

```ts
// 1. 处理 unknown 类型（常见场景）
let data: unknown = { name: "Alice" };
const name = (data as { name: string }).name; // 断言为 { name: string }

// 2. DOM 操作（典型场景）
const input = document.getElementById('username') as HTMLInputElement;
input.value = 'Hello'; // 编译器知道 input 是 HTMLInputElement

// 3. 联合类型时明确类型
type Status = 'success' | 'error';
function handleStatus(status: Status) {
  if (status === 'success') {
    console.log((status as 'success').toUpperCase()); // 断言为字面量类型
  }
}

// 4. 避免过度使用（危险示例）
const value = "123";
const num = value as number; // ❌ 断言错误！实际是字符串
console.log(num * 2); // 246（但类型是 number，实际运行结果可能意外）
```

> **重要提示**：  
>
> - 类型断言**只影响编译时检查**，不改变运行时类型（`typeof value` 仍是 `string`）  
> - 仅当**100%确定类型**时才使用，否则会隐藏潜在错误  
> - 优先使用 `as` 语法，避免尖括号在 JSX 中冲突





## 创建函数

### 定义

TypeScript 的函数类型用于**明确指定函数的参数类型和返回值类型**，确保调用函数时参数类型正确，返回值类型符合预期。它解决了类型不明确导致的运行时错误，提升代码可读性和可维护性。

> **关键区别**：
> 函数类型是**函数的类型描述**，而非函数本身（如 `() => string` 是类型，`function greet() { ... }` 是函数）

------

### 核心特点

- **参数类型**：必须与函数参数类型匹配
- **返回类型**：必须与函数返回值类型匹配
- **可选参数**：使用 `?` 标记，参数可省略
- **剩余参数**：使用 `...` 语法，接收任意数量参数
- **类型安全**：编译器会检查参数和返回值类型

------

### 食用方法

| 语法                                | 说明         | 示例                                   |
| ----------------------------------- | ------------ | -------------------------------------- |
| `参数: 类型 => 返回值类型`          | 箭头函数类型 | `(name: string) => string`             |
| `function (参数: 类型): 返回值类型` | 命名函数类型 | `function greet(name: string): string` |
| `参数?: 类型`                       | 可选参数     | `(name?: string) => void`              |
| `...参数: 类型[]`                   | 剩余参数     | `(...numbers: number[]) => number`     |

------

### 例子

```ts
// 1. 基础函数类型
const greet: (name: string) => string = (name) => `Hello, ${name}`;
console.log(greet("Alice")); // ✅ "Hello, Alice"

// 2. 可选参数
const formatName: (first: string, last?: string) => string = 
  (first, last) => last ? `${first} ${last}` : first;
console.log(formatName("Alice")); // ✅ "Alice"
console.log(formatName("Alice", "Smith")); // ✅ "Alice Smith"

// 3. 剩余参数
const sum: (...numbers: number[]) => number = (...numbers) => numbers.reduce((a, b) => a + b, 0);
console.log(sum(1, 2, 3)); // ✅ 6

// 4. 函数类型作为参数
const process = (callback: (value: number) => string) => callback(42);
console.log(process((n) => `Number: ${n}`)); // ✅ "Number: 42"
```

------

### 提示

- **类型推断**：如果函数有返回值，编译器会自动推断返回类型（但建议显式声明）

- **避免 `any`**：不要用 `(any) => any` 代替具体类型

- 函数类型别名：用 

  ```
  type
  ```

   定义可复用的函数类型

  ```ts
  type Greeting = (name: string) => string;
  const greet: Greeting = (name) => `Hi, ${name}`;
  ```

- 与普通函数区别：

  函数类型是类型描述，普通函数是实现

  ```ts
  // 类型 (描述)
  type Add = (a: number, b: number) => number;
  
  // 函数 (实现)
  const add: Add = (a, b) => a + b;
  ```



## 创建数组

### 定义

TypeScript 的数组类型用于**明确指定数组中元素的类型**，确保数组元素类型安全。它解决了类型不明确导致的运行时错误，提升代码可读性和可维护性。

> **关键区别**：
> 数组类型是**数组的类型描述**（如 `string[]`），而非数组本身（如 `["a", "b"]`）

------

### 核心特点

- **元素类型统一**：数组中所有元素必须是同一类型
- **类型安全**：编译器检查数组元素类型
- **两种写法**：`类型[]`（推荐）或 `Array<类型>`
- **避免 `any[]`**：优先使用 `unknown[]` 替代 `any[]`

------

### 食用方法

| 语法                      | 说明                        | 示例                          |
| ------------------------- | --------------------------- | ----------------------------- |
| `类型[]`                  | 数组类型（推荐写法）        | `string[]`                    |
| `Array<类型>`             | 数组类型（泛型写法）        | `Array<string>`               |
| `类型[]` 与 `Array<类型>` | 两种写法等价，推荐 `类型[]` | `string[]` 和 `Array<string>` |

------

### 例子

```ts
// ✅ 推荐：string[] 类型
const names: string[] = ["Alice", "Bob"];
console.log(names[0]); // ✅ "Alice"

// ❌ 错误：类型不匹配
// names.push(123); // 类型错误：不能将 number 赋值给 string

// ✅ 泛型写法（等价）
const ages: Array<number> = [25, 30];
console.log(ages[0]); // ✅ 25

// ✅ 任意类型数组（安全写法）
const data: unknown[] = [1, "two", true];
console.log(data[0]); // ✅ 1

// ❌ 不推荐：any[] 会绕过类型检查
const unsafeData: any[] = [1, "two", true];
unsafeData.push({}); // ✅ 但失去类型安全
```

------

### 提示

- **推荐 `类型[]`**：比 `Array<类型>` 更简洁，是官方推荐写法

- **避免 `any[]`**：用 `unknown[]` 替代，保持类型安全

- 联合类型数组：当元素类型可能不同，用联合类型

  ```ts
  const values: (string | number)[] = ["a", 1, "b", 2];
  ```

- 类型推断：初始化时有值，编译器会自动推断类型

  ```ts
  const numbers = [1, 2, 3]; // 类型推断为 number[]
  ```

> **最佳实践**：
> **永远使用 `类型[]` 声明数组，避免 `any[]`！**
> `string[]` 比 `Array<string>` 更简洁，且是 TypeScript 官方推荐写法。



## 创建元组

### 定义

TypeScript 的元组（Tuple）是**特有类型**，用于表示**固定数量且类型各异的数组**。它与数组的核心区别在于：**成员类型可以不同**，且**必须显式声明类型**（否则会被推断为联合类型数组）。

> **关键区别**：  
>
> - 数组：`string[]`（所有元素同类型）  
> - 元组：`[string, number]`（元素类型可不同）

------

### 核心特点

| 特性               | 说明                                                    | 限制                       |
| ------------------ | ------------------------------------------------------- | -------------------------- |
| **成员类型可不同** | 元组元素可为不同类型（如 `[string, number]`）           | -                          |
| **必须显式声明**   | 不声明会被推断为联合类型数组（如 `(string               | number)[]`）               |
| **可选成员**       | 尾部成员可加 `?`（如 `[string, number?]`）              | 可选成员必须在尾部         |
| **扩展运算符**     | 用 `...` 表示不定数量成员（如 `[string, ...number[]]`） | `...` 可在任意位置         |
| **只读元组**       | 用 `readonly [type]` 或 `Readonly<[type]>` 声明         | 普通元组是只读元组的子类型 |

------

### 食用方法

| 语法                   | 说明         | 示例                                                         |
| ---------------------- | ------------ | ------------------------------------------------------------ |
| `[类型1, 类型2, ...]`  | 基础元组声明 | `const user: [string, number] = ['Alice', 25]`               |
| `[类型1, 类型2?, ...]` | 可选尾部成员 | `const person: [string, number?] = ['Bob']`                  |
| `[类型1, ...类型[]]`   | 不定数量成员 | `const numbers: [string, ...number[]] = ['a', 1, 2, 3]`      |
| `readonly [类型]`      | 只读元组     | `const readonlyUser: readonly [string, number] = ['Alice', 25]` |

------

### 例子

```ts
// 基础元组
const user: [string, number] = ['Alice', 25];
console.log(user[0]); // ✅ "Alice"（类型 string）
console.log(user[1]); // ✅ 25（类型 number）

// 可选成员（尾部）
const person: [string, number?] = ['Bob'];
console.log(person[1]); // ✅ undefined（类型 number | undefined）

// 不定数量成员
const numbers: [string, ...number[]] = ['a', 1, 2, 3];
console.log(numbers[1]); // ✅ 1（类型 number）

// 只读元组
const readonlyUser: readonly [string, number] = ['Alice', 25];
// readonlyUser[0] = 'Bob'; // ❌ 不能修改
```

------

### 提示

- **不要越界访问**：元组长度固定，越界访问会报错

  ```ts
  const user: [string, number] = ['Alice', 25];
  console.log(user[2]); // ❌ Tuple type '[string, number]' has no element at index '2'
  ```

- **可选成员必须在尾部**：不能在可选成员后加必填成员

  ```ts
  // ❌ 错误：可选成员不能在必填成员前
  const wrong: [string?, number] = ['Alice'];
  ```

- **扩展运算符位置灵活**：`...` 可在元组任意位置

  ```ts
  const mixed: [string, ...number[], boolean] = ['a', 1, 2, 3, true];
  ```

- **类型推断**：元组类型推断遵循规则：

  - 无可选/扩展 → 确定长度
  - 有可选 → 推断可能长度
  - 有扩展 → 无法推断（视为数组）

> **一句话总结**：
> **元组 = 固定数量 + 类型各异的数组，必须显式声明类型，可选成员在尾部，扩展运算符位置灵活。**





## 创建枚举

### 定义

TypeScript 枚举（Enum）是**特有类型**，用于定义一组**命名的常量集合**。它解决了魔法数字/字符串问题，提高代码可读性和类型安全性，使代码更易维护。

> **关键区别**：
> 枚举是**类型**（如 `enum Status { ... }`），而非**值**（如 `Status.Pending`）

### 核心类型

#### 1. 数字枚举（默认类型）

**特点**：

- 默认从 `0` 开始递增
- 可自定义起始值
- **支持反向映射**（从值到名称）

```ts
enum Direction {
  Up,      // 0
  Down,    // 1
  Left,    // 2
  Right    // 3
}

// 自定义起始值
enum StatusCode {
  OK = 200,     // 200
  NotFound = 404, // 404
  Error = 500    // 500
}
```

**使用**：

```ts
let dir: Direction = Direction.Up;
console.log(Direction.Up); // 0
console.log(Direction[0]); // "Up"（反向映射）
```

------

#### 2. 字符串枚举

**特点**：

- 每个成员必须手动赋值为字符串
- **不支持反向映射**
- 适合语义清晰的场景（API、日志）

```ts
enum Color {
  Red = "RED",
  Green = "GREEN",
  Blue = "BLUE"
}
```

**使用**：

```ts
let color: Color = Color.Red;
console.log(color); // "RED"
// console.log(Color["RED"]); // 错误：字符串枚举不支持反向映射
```

------

#### 3. 异构枚举（不推荐）

**特点**：

- 混合使用数字和字符串
- 代码可读性差，易出错

```ts
enum Mixed {
  No = 0,
  Yes = "YES", // 混合类型
}
```

> 💡 **最佳实践**：避免使用异构枚举

------

### 高级特性

#### 1. 常量枚举（`const enum`）

**特点**：

- 编译时内联，**不生成额外代码**
- 不能包含计算成员
- 适合性能敏感场景

```ts
const enum Status {
  Pending = 1,
  Approved,
  Rejected
}

// 编译后直接内联
const status = Status.Approved; // 直接变成 2
```

------

#### 2. 计算枚举

**特点**：

- 成员值可以是表达式
- **必须放在最后**
- 通常用于位运算

```ts
enum FileAccess {
  None = 0,
  Read = 1 << 1,    // 2
  Write = 1 << 2,   // 4
  ReadWrite = Read | Write // 6
}
```

------

#### 3. 类型安全使用

**特点**：

- 枚举类型可作为接口属性
- 编译器能捕获类型错误

```ts
enum ShapeKind {
  Circle,
  Square
}

interface Circle {
  kind: ShapeKind.Circle; // 只能是 Circle
  radius: number;
}

const c: Circle = {
  kind: ShapeKind.Square, // ❌ 类型错误！
  radius: 100
};
```

------

### 提示

- **不要用 `any` 代替枚举**：枚举提供类型安全
- **优先使用字符串枚举**：语义更清晰
- **数字枚举支持反向映射**：`Direction[0]` → `"Up"`
- **字符串枚举不支持反向映射**：`Color["RED"]` 无效
- **常量枚举是性能优化**：在需要时使用 `const enum`

> **一句话总结**：
> **枚举 = 命名常量集合，数字枚举支持反向映射，字符串枚举语义清晰，常量枚举性能优化。**



## 创建Type（类型别名）

### 定义

TypeScript 的类型别名（`type`）用于**给现有类型起一个新名字**，提高代码可读性和可维护性。它**不创建新类型**，只是原类型的"别名"，别名和原类型完全等价。

> **核心区别**：
> 类型别名是**类型引用**，不是新类型（如 `type MyString = string`，`MyString` 本质是 `string`）

------

### 核心特点

| 特点             | 说明                                 | 例子                                      |
| ---------------- | ------------------------------------ | ----------------------------------------- |
| **语义化命名**   | 用描述性名字代替复杂类型             | `type ID = string                         |
| **类型复用**     | 一次定义，多处使用                   | `type User = { name: string }`            |
| **类型组合**     | 可组合基本类型、联合类型、交叉类型   | `type NameOrNumber = string               |
| **支持所有类型** | 适用于基本类型、对象、函数、元组等   | `type Func = (a: number) => string`       |
| **套娃机制**     | 别名可以引用其他别名（"别名的别名"） | `type Str = string; type UserName = Str;` |

------

### 食用方法

| 类型         | 语法                           | 示例                                            |
| ------------ | ------------------------------ | ----------------------------------------------- |
| **基本类型** | `type 别名 = 基本类型`         | `type ID = string;`                             |
| **对象类型** | `type 别名 = { 属性: 类型 }`   | `type User = { name: string; age: number }`     |
| **联合类型** | `type 别名 = 类型1 | 类型2`    | `type Status = 'pending' | 'success' | 'error'` |
| **函数类型** | `type 别名 = (参数) => 返回值` | `type Greeting = (name: string) => string`      |
| **元组类型** | `type 别名 = [类型1, 类型2]`   | `type Point = [number, number]`                 |

------

### 例子

```ts
// 1. 基本类型别名
type ID = string | number;
let userId: ID = "u123";
userId = 123; // ✅ 有效

// 2. 对象类型别名
type User = {
  name: string;
  age: number;
  email?: string; // 可选属性
};
const user: User = { name: "Alice", age: 30 };

// 3. 联合类型别名
type Status = "pending" | "success" | "error";
let status: Status = "pending";

// 4. 函数类型别名
type Greeting = (name: string) => string;
const greet: Greeting = (name) => `Hello, ${name}`;
console.log(greet("Alice")); // ✅ "Hello, Alice"

// 5. 套娃机制（别名的别名）
type Str = string;
type UserName = Str;
let nick: UserName = "Alice"; // ✅ 本质是 string
```

------

### 提示

- **与接口（Interface）区别**：

  - `type` 可以用于**所有类型**（包括基本类型、联合类型、元组等）
  - `interface` 专用于**对象类型**，不支持联合类型和元组
  - `type` 不能扩展，但可以使用交叉类型（`&`）组合

- **不要用别名重名**：别名不能和变量名重名

  ```ts
  type name = string; // ✅ 有效
  let name = "Alice"; // ❌ 无效：别名和变量名冲突
  ```

- **最佳实践**：

  - 用语义化名称：`type UserID = string | number` 比 `type ID = string | number` 更好
  - 避免过度套娃：简单类型不需要多层别名
  - 优先使用类型别名：比接口更灵活，适用于所有类型

> **一句话总结**：
> **类型别名 = 为类型起新名字，提高可读性，不创建新类型，支持所有类型。**



##  创建类

### 定义

类是**创建对象的蓝图**。就像“汽车设计图”可以造出多辆汽车，类可以创建多个具有相同结构的对象。
TypeScript 的类在 JavaScript 基础上增加了**类型安全**和**面向对象特性**（封装、继承等）。

> 💡 **初学者理解**：  
>
> - **类** = 设计图（描述“有什么属性/方法”）  
> - **对象** = 按设计图造出的具体产品（如“我的红色汽车”）

------

### 核心分类

#### 简单类（只有属性）

```ts
// 定义一个“人”类
class Person {
  name: string; // 声明属性（必须写类型！）
  age: number;
}

// 创建对象（实例）
const alice = new Person();
alice.name = "Alice"; // 赋值
alice.age = 25;
console.log(alice.name); // "Alice"
```

✅ **关键点**：  

- 属性必须**提前声明类型**（TypeScript 要求）  
- 用 `new` 关键字创建对象  
- 通过 `对象.属性` 访问/修改

------

#### 方法类（对象能做什么）

```ts
class Person {
  name: string;
  age: number;
  
  // 定义方法（函数）
  sayHello(): void {
    console.log(`Hello, I'm ${this.name}`);
  }
}

const bob = new Person();
bob.name = "Bob";
bob.sayHello(); // "Hello, I'm Bob"
```

✅ **关键点**：  

- 方法写在类内部  
- 用 `this` 指向当前对象  
- 方法也需要声明返回类型（`void` 表示无返回值）

------



#### 构造函数类（创建时自动初始化）

```ts
class Person {
  name: string;
  age: number;
  
  // 构造函数：创建对象时自动执行
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
  
  sayHello(): void {
    console.log(`Hello, I'm ${this.name}, ${this.age} years old`);
  }
}

// 创建时直接传参数
const tom = new Person("Tom", 30);
tom.sayHello(); // "Hello, I'm Tom, 30 years old"
```

✅ **关键点**：  

- `constructor` 是特殊方法，**创建对象时自动调用**  
- 参数在 `new Person(...)` 时传入  
- 避免创建后手动赋值（更安全高效）

------



### 访问修饰符（控制谁能访问）

| 修饰符      | 作用范围            | 解释                                 |
| ----------- | ------------------- | ------------------------------------ |
| `public`    | 任何地方都能访问    | “公开的，大家都能看”（默认）         |
| `private`   | 仅类内部能访问      | “私人的，外人不能碰”                 |
| `protected` | 类内部 + 子类能访问 | “受保护的，家人能看”                 |
| `readonly`  | 只读属性            | 属性无法被修改，可以和其他修饰符联用 |

```ts
class Person {
  public name: string;      // 公开（默认可省略）
  private secret: string;   // 私有
  
  constructor(name: string, secret: string) {
    this.name = name;
    this.secret = secret;
  }
  
  tellSecret(): void {
    console.log(`My secret: ${this.secret}`); // ✅ 类内部可访问
  }
}

const lily = new Person("Lily", "I love cats");
console.log(lily.name);    // ✅ "Lily"（public 可访问）
// console.log(lily.secret); // ❌ 错误！private 属性外部不可访问
lily.tellSecret();         // ✅ "My secret: I love cats"
```

------

### 简化写法

```ts
// 传统写法（重复！）
class Person {
  name: string;
  age: number;
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
}

// ✨ 参数属性写法（推荐！）
class Person {
  constructor(
    public name: string,  // 自动声明 + 赋值
    private age: number   // 自动声明为私有 + 赋值
  ) {}
}

const amy = new Person("Amy", 28);
console.log(amy.name); // ✅ "Amy"
// console.log(amy.age); // ❌ 私有属性不可访问
```

✅ **关键点**：  

- 在 `constructor` 参数前加 `public/private/readonly`  
- TypeScript **自动完成**：声明属性 + 赋值  
- **强烈推荐**！减少重复代码，初学者必学技巧

------

### 继承

#### 定义

继承是面向对象编程的核心特性之一，允许一个类（子类）**复用另一个类（父类）的属性和方法**，并在此基础上添加新功能或修改行为。在 TypeScript 中，使用 `extends` 关键字实现。

> 💡 **理解**：  
>
> - 父类 = 基础模板（如“人”）  
> - 子类 = 特殊类型（如“学生”），既有人的共性，又有自己的特性

------

#### 核心语法

```ts
class 子类 extends 父类 {
  // 新增属性
  newProperty: 类型;
  
  constructor(参数) {
    super(父类参数); // 必须调用！
    this.newProperty = 参数;
  }
  
  // 新增方法
  newMethod(): void { /* ... */ }
  
  // 重写父类方法（可选）
  override parentMethod(): void { /* ... */ }
}
```

#### override重写标记

在重写父类方法时 要使用override标记

`override` 是 TypeScript 的“重写安全锁”：在子类方法前添加此关键字，编译器会严格校验父类是否存在该方法——拼写错误当场报错，杜绝“以为重写了实则新增”的隐蔽 bug，让继承更可靠 ✨

------

#### 例子

```ts
// 父类：Person
class Person {
  constructor(
    public name: string,
    public age: number
  ) {}
  
  greet(): void {
    console.log(`Hello, I'm ${this.name}`);
  }
}

// 子类：Student 继承 Person
class Student extends Person {
  grade: string; // 新增属性
  
  constructor(name: string, age: number, grade: string) {
    super(name, age); // ✅ 必须调用父类构造函数
    this.grade = grade; // ✅ 初始化子类新增属性
  }
  
  study(): void { // 新增方法
    console.log(`${this.name}正在努力学习中……`);
  }
  
  // 重写父类方法（可选）
  greet(): void {
    console.log(`Hi, I'm ${this.name}, a student in grade ${this.grade}`);
  }
}
```

------

#### 使用示例

```ts
const student = new Student("Alice", 20, "Grade 10");
student.greet();   // "Hi, I'm Alice, a student in grade Grade 10"（重写后）
student.study();   // "Alice正在努力学习中……"
console.log(student.age); // 20（继承自父类）
```

------

#### 🌟 提示

1. **必须调用 `super()`**  

   - 在子类构造函数中，**必须先调用 `super()`** 才能使用 `this`  
   - `super()` 调用父类构造函数，初始化父类属性

2. **子类可以添加新属性/方法**  

   - 比如 `Student` 添加了 `grade` 属性和 `study()` 方法

3. **可以重写父类方法**  

   - 用相同名字的方法覆盖父类行为（如 `greet()`）

4. **继承所有公共成员**  

   - 子类自动获得父类的 `public` 属性和方法  
   - `private` 属性不能直接访问（但可通过方法间接访问）

5. **多层继承**  

   ```ts
   class Teacher extends Person { /* ... */ }
   class GraduateStudent extends Student { /* ... */ } // 可以继续扩展
   ```

6. **最佳实践**  

   - 优先使用 **参数属性** 简化代码  
   - 将通用逻辑放在父类，特殊逻辑放在子类  
   - 避免过深继承链（建议不超过 3 层）

> ✅ **一句话总结**：
> **继承 = 复用 + 扩展，子类用 `extends` 继承父类，`super()` 初始化父类，再添加自己独有的属性和方法。**



## 创建抽象类

### 定义

抽象类是**不能被实例化的类**，用于定义**基类骨架**，强制子类实现特定方法。它解决了“父类定义接口，子类实现细节”的问题，是实现**多态**的核心机制。

> 💡 **理解**：  
>
> - 抽象类 = **设计图纸**（不能直接造房子，必须按图纸建具体房子）  
> - 普通类 = **具体房子**（可以直接住人）

------

### 核心特点

| 特点               | 说明                                             | 例子                                           |
| ------------------ | ------------------------------------------------ | ---------------------------------------------- |
| **不能实例化**     | `new AbstractClass()` 会报错                     | `const a = new AbstractClass(); // ❌`          |
| **可包含抽象方法** | 用 `abstract` 修饰，**无实现**（必须在子类实现） | `abstract getName(): string;`                  |
| **可包含普通方法** | 有实现，可被子类继承或重写                       | `sayHello() { console.log("Hello"); }`         |
| **强制子类实现**   | 子类**必须**实现所有抽象方法                     | `class Concrete extends AbstractClass { ... }` |

------

### 例子：抽象类 + 子类实现

```ts
// ✅ 抽象类（定义骨架）
abstract class Shape {
  abstract getArea(): number; // 必须由子类实现
  
  // 普通方法（可继承）
  describe(): string {
    return `This shape has area: ${this.getArea()}`;
  }
}

// ✅ 子类实现抽象方法
class Circle extends Shape {
  constructor(private radius: number) {
    super();
  }
  
  // ✅ 必须实现抽象方法
  getArea(): number {
    return Math.PI * this.radius ** 2;
  }
}

class Square extends Shape {
  constructor(private side: number) {
    super();
  }
  
  getArea(): number {
    return this.side ** 2;
  }
}

// ✅ 使用（多态调用）
const shapes: Shape[] = [new Circle(5), new Square(4)];
shapes.forEach(shape => console.log(shape.describe()));
// 输出：
// This shape has area: 78.53981633974483
// This shape has area: 16
```

------

### 💡 关键提示

1. **抽象类 = 基类骨架**  

   - 不能 `new` 实例化（`abstract class` 不能被直接构造）  
   - 但可以**被继承**（`class Sub extends AbstractClass`）

2. **抽象方法 = 必须实现**  

   - 用 `abstract` 修饰，**不能有函数体**  
   - 子类**必须**实现（否则编译报错）

3. **与接口（Interface）区别**  

   | 特点               | 抽象类           | 接口                 |
   | ------------------ | ---------------- | -------------------- |
   | 可包含实现         | ✅                | ❌                    |
   | 可包含状态（属性） | ✅                | ❌                    |
   | 可继承             | ✅                | ❌（用 `implements`） |
   | 适合场景           | 有共同实现的基类 | 仅定义契约           |

4. **多态的核心**  

   ```ts
   const shape: Shape = new Circle(5); // 父类引用指向子类对象
   shape.getArea(); // 运行时调用 Circle 的实现（多态）
   ```

5. **不要滥用**  

   - 仅当**需要共享实现**时用抽象类（如 `Shape` 有 `describe()`）  
   - 仅**定义契约**时用接口（如 `interface Shape { getArea(): number }`）







## 创建接口类

### 定义

接口是 TypeScript 中**定义对象结构**的类型声明，用于**描述对象应该包含哪些属性和方法**，但**不提供实现**。它像一份"契约"，规定了对象必须遵守的结构，是实现**类型安全**和**代码可维护性**的核心工具。

> 💡 **理解**：  
>
> - 接口 = **设计图纸**（规定“对象必须有这些属性和方法”）  
> - 对象 = **按图纸建造的房子**（必须包含图纸要求的元素）

------

### 核心特点

| 特点             | 接口（Interface）                                        | 类型别名（`type`）               |
| ---------------- | -------------------------------------------------------- | -------------------------------- |
| **定义对象结构** | ✅ 专为对象设计                                           | ✅ 也支持对象                     |
| **支持扩展**     | ✅ `interface A { ... }`  `interface B extends A { ... }` | ❌ 需用交叉类型 `&`               |
| **合并声明**     | ✅ 同名接口自动合并                                       | ❌ 重复定义会报错                 |
| **适合场景**     | 对象结构描述（API、组件Props）                           | 任意类型（基本类型、联合类型等） |

> 接口不仅可以规范类的结构 还能作为定义类型使用 类似于模板 主要规定对象的结构



### 食用方法

#### 1. 基础接口（定义对象结构）

```ts
// 定义接口
interface User {
  name: string;
  age: number;
  email?: string; // 可选属性
}

// 使用接口（必须满足结构）
const user: User = { name: "Alice", age: 30 };
// ✅ 正确：email 可省略
// ❌ 错误：缺少 name 或 age

// 可选属性（安全访问）
console.log(user.email); // ✅ 返回 string | undefined
```

#### 2. 接口扩展（继承结构）

```ts
interface Person {
  name: string;
  age: number;
}

// 扩展 Person 接口
interface Student extends Person {
  grade: string;
  school: string;
}

const student: Student = {
  name: "Bob",
  age: 15,
  grade: "10th",
  school: "High School"
};
```

#### 3. 可选属性（`?` 语法）

```ts
interface Config {
  host: string;
  port?: number; // 可选
  timeout: number;
}

// ✅ 正确：port 可省略
const config1: Config = { host: "localhost", timeout: 5000 };

// ❌ 错误：缺少 timeout
// const config2: Config = { host: "localhost" };
```

#### 4. 只读属性（`readonly`）

```ts
interface Product {
  readonly id: string; // 不能修改
  name: string;
  price: number;
}

const product: Product = { id: "p123", name: "Book", price: 25 };
// ✅ 正确
product.price = 30; 

// ❌ 错误：不能修改 id
// product.id = "p456";
```



### 使用场景

#### 接口定义类（实现类的结构契约）

当需要定义**类的结构规范**时，接口确保类必须包含特定属性和方法。类通过 `implements` 实现接口，避免重复定义。

```typescript
interface Greetable {
  name: string;
  greet(): void;
}

class Person implements Greetable {
  constructor(public name: string) {}
  greet() { console.log(`Hello, ${this.name}`); }
}

// ✅ 正确：Person 实现了 Greetable
const person = new Person("Alice");
person.greet(); // "Hello, Alice"
```

> **为什么用接口？**
> 保证多个类（如 `Student`, `Teacher`）都遵循相同结构，提升代码一致性。

------

#### 接口定义函数（函数类型签名）

当需要**约束函数的输入/输出结构**时，接口定义函数的类型签名，比 `type` 更清晰。

```typescript
interface Fetcher {
  (url: string, options?: RequestInit): Promise<Response>;
}

const fetcher: Fetcher = async (url, options) => {
  return fetch(url, options);
};

// ✅ 正确：符合 Fetcher 结构
fetcher("https://api.example.com");
```

> **为什么用接口？**
> 使函数类型声明更可读，且支持扩展（如 `interface AdvancedFetcher extends Fetcher { ... }`）。

------

#### 接口的继承（扩展结构）

当**多个接口有共同属性**时，用 `extends` 扩展接口，避免重复定义。

```typescript
interface BaseConfig {
  host: string;
  timeout: number;
}

// ✅ 扩展 BaseConfig
interface APIConfig extends BaseConfig {
  endpoint: string;
  version: string;
}

const apiConfig: APIConfig = {
  host: "api.example.com",
  timeout: 5000,
  endpoint: "/users",
  version: "v1"
};
```

> **为什么用接口继承？**
> 组织复杂的配置结构，例如 `APIConfig` 是 `BaseConfig` 的扩展，`WebSocketConfig` 也可继承 `BaseConfig`。

------

#### 接口用于 API 响应（类型安全）

在**HTTP 请求/响应**中，接口定义数据结构，确保数据符合预期。

```typescript
interface UserResponse {
  id: number;
  name: string;
  email: string;
  isActive: boolean;
}

async function fetchUser(): Promise<UserResponse> {
  const response = await fetch("/api/user");
  return response.json(); // ✅ TypeScript 自动检查返回结构
}

fetchUser().then(user => {
  console.log(user.name); // ✅ 类型安全
  // console.log(user.address); // ❌ 编译报错！
});
```

> **为什么用接口？**
> 防止因后端返回结构变化导致的运行时错误，编译时即可捕获问题。

------

#### 接口用于组件 Props（前端框架）

在 React/Vue 等框架中，接口定义**组件的 Props 类型**，确保传入的属性正确。

```typescript
interface ButtonProps {
  label: string;
  variant?: "primary" | "secondary";
  disabled?: boolean;
}

const Button = ({ label, variant = "primary", disabled = false }: ButtonProps) => (
  <button className={`btn-${variant}`} disabled={disabled}>
    {label}
  </button>
);

// ✅ 正确：符合 ButtonProps
<Button label="Submit" variant="primary" />

// ❌ 错误：缺少 label
// <Button variant="secondary" />
```

> **为什么用接口？**
> 提供清晰的组件文档，避免传入错误的 Props 类型。

------

#### 接口 vs 类型别名：场景选择

| 场景                                       | 接口 | 类型别名 | 选择理由                  |
| ------------------------------------------ | ---- | -------- | ------------------------- |
| 定义对象结构（API/组件）                   | ✅    | ✅        | 接口支持扩展/合并，更直观 |
| 定义联合类型（如 `string | number`）       | ❌    | ✅        | 类型别名更适合复杂类型    |
| 定义函数类型（如 `(a: number) => string`） | ❌    | ✅        | 类型别名更简洁            |
| 需要扩展/合并接口                          | ✅    | ❌        | 接口自动合并，无需 `&`    |

> **最佳实践**：
> **"结构描述用接口，复杂类型用类型别名"**
> 例如：`User` 用接口，`ID = string \| number` 用类型别名。



### 综合例子

```TS
// ✅ 正确：满足接口所有要求
const user: UserInterface = {
  name: "张三",
  gender: "男", // 初始化后不可改
  age: 25,     // 可选，可以有
  run: (n) => console.log(`奔跑${n}米`) // 符合函数类型
};

// ✅ 正确：可选属性可以省略
const user2: UserInterface = {
  name: "李四",
  gender: "女"
};

// ❌ 错误：缺少必选属性
// const user3: UserInterface = { gender: "男" }; 

// ❌ 错误：修改只读属性
// user.gender = "女"; 

// ❌ 错误：方法参数类型错误
// user.run("100"); 
```



### 💡提示

1. **接口 = 对象的结构契约**  

   - 用 `interface` 定义对象**必须包含的属性和方法**  
   - 用 `type` 定义**任意类型**（包括基本类型、联合类型等）

2. **接口扩展比类型别名更友好**  

   ```ts
   // 接口扩展（自动合并）
   interface A { x: number }
   interface A { y: string } // ✅ 自动合并成 { x: number, y: string }
   
   // 类型别名扩展（需用 &）
   type A = { x: number }
   type A = A & { y: string } // ❌ 需要 & 符号
   ```

3. **接口与类配合**（核心用法！）

   ```ts
   interface Greetable {
     greet(): void;
   }
   
   class Person implements Greetable {
     name: string;
     constructor(name: string) { this.name = name }
     greet() { console.log(`Hello, ${this.name}`); }
   }
   
   // ✅ 正确：Person 实现了 Greetable
   const p = new Person("Alice");
   p.greet();
   ```

4. **接口 vs 类型别名：选哪个？**  

   | 场景          | 接口   | 类型别名 |
   | ------------- | ------ | -------- |
   | 定义对象结构  | ✅ 推荐 | ✅ 也可用 |
   | 定义联合类型  | ❌      | ✅ 推荐   |
   | 定义函数类型  | ❌      | ✅ 推荐   |
   | 需要扩展/合并 | ✅ 推荐 | ❌        |

5. **不要用接口定义基础类型**  

   ```ts
   // ❌ 错误：接口不用于基础类型
   interface ID { string } 
   // ✅ 正确：用类型别名
   type ID = string;
   ```

------

> ✅ **一句话总结**：
> **接口 = 对象结构的契约，用 `interface` 定义对象必须有什么，用 `implements` 让类遵守契约，扩展用 `extends`，是 TypeScript 类型安全的基石！**



## 泛型

### 定义

在 TypeScript 中，**类型安全**是核心价值。然而，当编写可复用的函数或类时，若使用 `any` 类型（如 `function identity(arg: any): any`），将**完全丧失类型安全**。泛型（Generics）正是为解决此问题而生：**允许在不牺牲类型安全的前提下，编写可处理多种类型的组件**。

> **核心问题**：
> 未使用泛型时，必须用 `any` 或联合类型（如 `string | number`），导致：
>
> - 类型错误在运行时才暴露
> - 无法利用 TypeScript 的类型检查优势
> - 代码可维护性下降

------

### 基础概念

#### 1. 泛型的定义

**泛型**是**类型参数化**的机制，通过在声明时使用**类型变量**（如 `T`），使函数/类/接口能够**适应任意类型**，同时保持类型安全。

> **关键区别**：
>
> - **非泛型**：类型固定（如 `string`）
> - **泛型**：类型可变（如 `T`），在调用时确定

#### 2. 泛型的语法

```typescript
// 声明泛型函数
function identity<T>(arg: T): T {
  return arg;
}
```

- `T` 是类型参数（Type Parameter），代表任意类型
- 调用时，`T` 被实际类型替换（如 `string`）

------

### 泛型函数详解

#### 步骤 1：普通函数的缺陷（无泛型）

```typescript
function identity(arg: any): any {
  return arg;
}

// 调用
const output = identity("hello"); // 类型为 any
console.log(output.length); // ✅ 运行时通过，但类型不安全
```

> **问题**：`output` 类型为 `any`，无法在编译阶段捕获错误（如 `output.length` 在 `number` 类型下会报错）。

------

#### 步骤 2：引入泛型（正确用法）

```typescript
function identity<T>(arg: T): T {
  return arg;
}

// 调用（类型自动推断）
const output = identity("hello"); // 类型为 string
console.log(output.length); // ✅ 编译通过（类型安全）
```

> **关键点**：
>
> 1. `T` 是类型参数，声明在函数名后（`identity<T>`）
> 2. 调用时 `T` 自动推断为 `string`
> 3. 返回值类型与输入类型一致（`string` → `string`）

------

#### 步骤 3：显式指定类型参数

```typescript
function identity<T>(arg: T): T {
  return arg;
}

// 显式指定 T 为 number
const output = identity<number>(123); 
console.log(output.toFixed(2)); // ✅ 编译通过
```

> **何时显式指定**？
>
> - 当类型无法推断时（如 `identity(123)` 无法区分 `number` 和 `string`）
> - 为提高代码可读性（明确类型意图）

------

### 泛型进阶

#### 1. 泛型类（Class）

**场景**：创建可存储任意类型数据的类（如 `Box`）。

```typescript
class Box<T> {
  private value: T;

  constructor(value: T) {
    this.value = value;
  }

  getValue(): T {
    return this.value;
  }
}

// 使用
const stringBox = new Box<string>("hello");
console.log(stringBox.getValue().length); // ✅ string 类型

const numberBox = new Box<number>(42);
console.log(numberBox.getValue().toFixed(2)); // ✅ number 类型
```

> **核心**：
>
> - `Box<T>` 表示 `Box` 类可以处理任意类型 `T`
> - `value` 和 `getValue` 的类型基于 `T` 自动确定

------

#### 2. 泛型接口（Interface）

**场景**：定义通用数据结构（如 `Stack`）。

```typescript
interface Stack<T> {
  push(item: T): void;
  pop(): T | undefined;
}

class StringStack implements Stack<string> {
  private items: string[] = [];

  push(item: string): void {
    this.items.push(item);
  }

  pop(): string | undefined {
    return this.items.pop();
  }
}

// 使用
const stack = new StringStack();
stack.push("A");
console.log(stack.pop()); // ✅ "A"（类型 string）
```

> **关键**：  
>
> - `Stack<T>` 是泛型接口，`StringStack` 实现时指定 `T = string`
> - 保证了 `push` 和 `pop` 的类型一致性

------

#### 3. 约束泛型（Constraints）

**问题**：若想要求泛型必须有特定属性（如 `length`），需使用 `extends`。

```typescript
function loggingIdentity<T>(arg: T[]): T[] {
  console.log(arg.length); // ✅ T 必须有 length 属性
  return arg;
}

// 使用
loggingIdentity(["a", "b"]); // ✅ 正确
loggingIdentity(123); // ❌ 编译错误：number 没有 length 属性
```

> **约束语法**：
>
> ```typescript
> function fn<T extends { length: number }>(arg: T): T {
>   // T 必须包含 length 属性
> }
> ```
>
> **常见约束**：
>
> - `T extends string`：要求 `T` 是字符串
> - `T extends number[]`：要求 `T` 是数字数组

------

#### 4. 多个类型参数

**场景**：处理关联类型（如 `Pair`）。

```typescript
class Pair<K, V> {
  constructor(private key: K, private value: V) {}

  getKey(): K { return this.key; }
  getValue(): V { return this.value; }
}

// 使用
const pair = new Pair<string, number>("age", 30);
console.log(pair.getKey()); // ✅ "age"（类型 string）
console.log(pair.getValue()); // ✅ 30（类型 number）
```

> **用法**：`Pair<K, V>` 表示两个独立的类型参数

------

### 常见错误实践

#### ❌ 错误 1：过度使用 `any`

```typescript
// 错误示范：丧失类型安全
function badIdentity(arg: any): any {
  return arg;
}
```

> **正确做法**：始终使用泛型（`<T>`）替代 `any`

------

#### ❌ 错误 2：忽略类型约束

```typescript
function getFirst<T>(arr: T[]): T {
  return arr[0]; // ❌ 无约束，arr 可能为空
}

// 调用
getFirst([]); // ❌ 运行时错误：undefined.length
```

> **正确做法**：添加约束并处理边界

```typescript
function getFirst<T>(arr: T[]): T | undefined {
  return arr.length > 0 ? arr[0] : undefined;
}
```

------

#### ✅ 最佳实践

1. **优先使用泛型**，避免 `any`
2. **为类型参数使用有意义的名称**（`T` 为通用，`K` 为键，`V` 为值）
3. **添加必要的约束**（如 `T extends { length: number }`）
4. **保持简单**：泛型应解决类型问题，而非复杂化代码

------

### 典型应用场景

| 场景             | 代码示例                                    | 优势                         |
| ---------------- | ------------------------------------------- | ---------------------------- |
| **数据容器**     | `Array<T>`, `Box<T>`                        | 存储任意类型数据，类型安全   |
| **API 响应处理** | `fetchData<T>(url: string): Promise<T>`     | 确保返回数据结构匹配预期类型 |
| **工具函数**     | `map<T, U>(arr: T[], fn: (t: T) => U): U[]` | 复用逻辑，保持类型一致性     |
| **组件 Props**   | `interface Props<T> { data: T }`            | 前端框架中灵活传递数据类型   |

------

### 核心价值

> **泛型 = 类型安全 + 代码复用**
> 通过类型参数（`T`），在**编译阶段**确保：
>
> 1. 函数/类/接口能处理多种类型
> 2. 类型错误被提前捕获
> 3. 无需重复定义相同逻辑

> **一句话终极总结**：
> **泛型是 TypeScript 的类型安全引擎，它使代码成为“通用模板”，而非“类型陷阱”。**
> 从 `identity<T>` 开始，逐步掌握泛型，将彻底改变你编写可维护代码的方式。

------

> **本笔记已覆盖泛型核心概念**：
> 从基础语法 → 进阶约束 → 实际场景 → 常见错误。
> **实践建议**：在项目中尝试将 `any` 替换为泛型，体验类型安全的提升。



## 类装饰器

------

### 定义

**类装饰器 = 为类添加功能的"钩子"**
在**类定义时**自动执行，无需修改类内部代码，用于日志、注册、修改类结构等。

> 💡 **类比**：
> 就像给房子贴"安全认证"标签——房子本身不变，但增加了安全属性。

------

### 核心语法

```typescript
// 1. 定义装饰器函数（接收类构造函数）
function logClass(target: Function) {
  console.log(`类 ${target.name} 被装饰`);
}

// 2. 应用装饰器（类定义前）
@logClass
class User {
  constructor(public name: string) {}
}

// ✅ 调用：类定义时自动执行装饰器
new User("Alice"); // 控制台输出：类 User 被装饰
```

------

### 参数解析

| 参数               | 说明                         | 例子                                 |
| ------------------ | ---------------------------- | ------------------------------------ |
| `target`           | **类的构造函数**（不是实例） | `target.name` → `"User"`             |
| `target.prototype` | 类的原型对象（可修改方法）   | `target.prototype.greet = () => ...` |

------

### 🌰 案例

#### 案例 1：基础日志（最常用）

```typescript
function logClass(target: Function) {
  console.log(`[LOG] 类 ${target.name} 已加载`);
}

@logClass
class Product {
  constructor(public id: string, public name: string) {}
}

// 输出：[LOG] 类 Product 已加载
```

#### 案例 2：修改类行为（高级用法）

```typescript
function addId(target: Function) {
  // 1. 保存原构造函数
  const original = target;
  
  // 2. 创建新构造函数
  const newConstructor = function (...args: any[]) {
    // 3. 添加 id 属性
    this.id = Date.now().toString(36) + Math.random().toString(36).substr(2, 5);
    return original.apply(this, args);
  };
  
  // 4. 继承原类属性
  Object.assign(newConstructor, original);
  return newConstructor;
}

@addId
class Book {
  constructor(public title: string, public author: string) {}
}

const book = new Book("TS Guide", "Qwen");
console.log(book.id); // ✅ 生成唯一ID（如：a1b2c3）
```



### 返回值替换类

当装饰器装饰一个类并且装饰器的返回值返回了一个类时，装饰器的返回值会替换掉这个类

```ts
function Demo(target: Function) {
    return class {
        test() {
            console.log(200);
            console.log(300);
            console.log(400);
        }
    }
}

@Demo
class Person {
    test() {
        console.log(100);
    }
}

const person = new Person();
person.test();// ✅ 这才是会输出 200 300 400
```





------

### 关键点

1. **执行时机**  

   - ✅ **类定义时**执行（不是实例化时）  
   - ❌ 不会在 `new User()` 时触发

2. **不能返回新类**（但可以修改原类）  

   ```typescript
   // 错误：返回新类会导致类型错误
   function badDecorator(target: Function) {
     return class { /* ... */ }; // ❌ 会破坏类型
   }
   ```

3. **保留原类特性**  

   - 通过 `Object.assign(newConstructor, original)` 保持 `name`/`prototype` 等属性

4. **必须启用实验性功能**  

   ```json
   // tsconfig.json
   {
     "compilerOptions": {
       "experimentalDecorators": true
     }
   }
   ```

------

### 总结

> **类装饰器 = `@decorator` 修饰类，接收类构造函数 `target`，在类定义时执行，用于日志/注册/扩展类行为。**

------

> 💡 **实践建议**：  
>
> 1. 从 `logClass` 开始（最简单）  
> 2. 用 `console.log(target.name)` 确认执行时机  
> 3. 优先用于**非侵入式功能**（如日志、注册）  
> 4. **不要**在类装饰器中修改类的构造逻辑（除非必要）  
> 5. 90% 场景用 `logClass` 和 `addId` 两类即可





## 装饰器工厂

------

### 定义

**装饰器工厂 = 接收参数并返回装饰器的函数**
通过 `@LogInfo(3)` 形式调用，**先用参数创建装饰器，再应用到类**。

> 💡 **本质**：
> `@LogInfo(3)` = `LogInfo(3)` 返回装饰器函数，再应用到类。

------

### 例子

```typescript
function LogInfo(n: number) {
  return function(target: Function) {
    target.prototype.introduce = function() {
      for (let i = 0; i < n; i++) {
        console.log(`我的名字：${this.name}，我的年龄：${this.age}`);
      }
    };
  };
}

@LogInfo(3) // ⚙️ 工厂调用（传入参数 3）
class Person {
  constructor(public name: string, public age: number) {}
}

// 使用
const p = new Person("张三", 25);
p.introduce(); // ✅ 输出 3 次个人信息
```

------

### 执行流程

1. **`@LogInfo(3)` 执行**
   → 调用 `LogInfo(3)` → 返回装饰器函数 `function(target: Function)`
2. **装饰器函数执行**
   → `target` = `Person` 类的构造函数
   → 在 `Person.prototype` 上添加 `introduce` 方法
3. **实例调用**
   → `p.introduce()` 触发新方法 → 输出 3 次

------

### 类似PY

| Python 语法          | TypeScript 语法                     | 作用                   |
| -------------------- | ----------------------------------- | ---------------------- |
| `@decorator(3)`      | `@LogInfo(3)`                       | 创建带参数的装饰器     |
| `def decorator(n):`  | `function LogInfo(n: number):`      | 工厂函数（返回装饰器） |
| `def inner(target):` | `return function(target: Function)` | 真正的装饰器函数       |

> ✅ **结论**：
> **TypeScript 装饰器工厂 = Python 的带参数装饰器**
> 你学过的 Python 装饰器直接迁移即可！

------

### 总结

> **装饰器工厂 = `function 参数() { return 装饰器函数 }`**
> 用 `@工厂(参数)` 调用，**在类原型上动态添加方法**，实现功能增强。

------

> 💡 **实践建议**：  
>
> 1. 从 `LogInfo` 这类带参数工厂开始（最常用）  
> 2. 重点修改 `target.prototype`（类原型）  
> 3. **不要**在装饰器中修改类的构造逻辑（除非必要）  
> 4. 90% 场景用工厂模式 + `target.prototype` 即可





## 装饰器组合

当多个装饰器同时使用的时候，优先执行装饰器工厂，多个工厂从上到下，让其返回装饰器，然后类似洋葱模型，从离被装饰类最近的装饰逐步向外装饰，也就是从下到上

为了完善这个 TypeScript 装饰器的例子，我们可以在现有代码的基础上添加一个类和一个方法，以便观察装饰器的实际执行顺序。以下是补充完善后的代码示例：

```typescript
// 装饰器
function test1(target: Function) {
  console.log('test1');
}

// 装饰器工厂
function test2() {
  console.log('test2工厂');
  return function (target: Function) {
    console.log('test2');
  }
}

// 装饰器工厂
function test3() {
  console.log('test3工厂');
  return function (target: Function) {
    console.log('test3');
  }
}

// 装饰器
function test4(target: Function) {
  console.log('test4');
}

// 定义一个类并应用装饰器
@test1
@test2()
@test3()
@test4
class ExampleClass {
  constructor() {
    console.log('类被实例化');
  }

  exampleMethod() {
    console.log('方法被调用');
  }
}

// 实例化类
const example = new ExampleClass();
example.exampleMethod();
```

### 执行顺序解释

1. **装饰器工厂的执行顺序**：
   - `@test2()` 和 `@test3()` 是装饰器工厂，它们在应用到类上的时候会立即执行，因此会先输出 `test2工厂` 和 `test3工厂`。
2. **装饰器的执行顺序**：
   - 装饰器的执行顺序是从下到上，即 `@test4` -> `@test3()` -> `@test2()` -> `@test1`。
   - 因此，接下来的输出顺序为 `test4` -> `test3` -> `test2` -> `test1`。
3. **类的实例化**：
   - 最后，当我们实例化 `ExampleClass` 时，会输出 `类被实例化`。
4. **方法的调用**：
   - 调用 `example.exampleMethod()` 时，会输出 `方法被调用`。

### 预期输出

根据上述解释，完整的输出顺序应该是：

```txt
test2工厂
test3工厂
test4
test3
test2
test1
类被实例化
方法被调用
```

这样，你就得到了一个完整的 TypeScript 装饰器执行顺序的示例。



## 属性装饰器

------

### 定义

**属性装饰器 = 为类属性添加功能的"钩子"**
在**属性定义时**自动执行，用于**修改属性行为**（如添加元数据、设置只读、验证等）。

> 💡 **类比**：
> 属性装饰器就像给商品贴"质量认证"标签——商品本身不变，但增加了额外信息。

------

### 基础语法

```typescript
// 1. 定义属性装饰器（接收 target + propertyKey）
function readonly(target: any, propertyKey: string) {
  Object.defineProperty(target, propertyKey, {
    writable: false // 使属性不可修改
  });
}

// 2. 应用装饰器（属性定义前）
class Person {
  @readonly
  name: string = "Alice";
}

// 使用
const p = new Person();
p.name = "Bob"; // ❌ 运行时错误：Cannot assign to read only property
```

------

### 参数解析

| 参数          | 说明                                                | 例子                              |
| ------------- | --------------------------------------------------- | --------------------------------- |
| `target`      | **类的原型对象**（实例属性） **类本身**（静态属性） | `target.name` → `Person` 类的原型 |
| `propertyKey` | 属性名（字符串）                                    | `propertyKey` → `"name"`          |

------

### 🌰 案例

#### 案例 1：基础只读属性（最常用）

```typescript
function readonly(target: any, propertyKey: string) {
  Object.defineProperty(target, propertyKey, {
    writable: false
  });
}

class User {
  @readonly
  id: string = "user-123";
}

const user = new User();
user.id = "new-id"; // ❌ 运行时错误：Cannot assign to read only property
```

#### 案例 2：添加元数据（高级用法）

```typescript
function logProperty(target: any, propertyKey: string) {
  console.log(`属性 ${propertyKey} 被装饰`);
}

class Product {
  @logProperty
  price: number = 100;
}

// 输出：属性 price 被装饰
```

#### 案例 3：验证属性（输入校验）

```typescript
function emailValidator(target: any, propertyKey: string) {
  let value: string;
  const getter = () => value;
  const setter = (newValue: string) => {
    if (!newValue.includes('@')) {
      throw new Error('邮箱格式错误');
    }
    value = newValue;
  };
  Object.defineProperty(target, propertyKey, {
    get: getter,
    set: setter,
    enumerable: true
  });
}

class Contact {
  @emailValidator
  email: string = "valid@domain.com";
}

const contact = new Contact();
contact.email = "invalid"; // ❌ 报错：邮箱格式错误
contact.email = "valid@domain.com"; // ✅
```

------

### 关键点

1. **执行时机**  

   - ✅ **属性定义时**执行（不是实例化时）
   - ❌ 不会在 `new User()` 时触发

2. **不能返回值**  

   ```typescript
   function badDecorator(target: any, propertyKey: string) {
     return { ... }; // ❌ 会忽略返回值
   }
   ```

3. **修改属性描述符**  

   - 通过 `Object.defineProperty(target, propertyKey, descriptor)` 实际修改属性
   - 不能用 `return` 返回新描述符

4. **静态属性支持**  

   ```typescript
   class Config {
     @readonly
     static API_KEY = "abc123";
   }
   Config.API_KEY = "new"; // ❌ 报错
   ```

------

### 总结

> **属性装饰器 = `@decorator` 修饰属性，接收 `target`（原型）和 `propertyKey`（属性名），在属性定义时修改其行为（如只读/验证）。**

------

> 💡 **实践建议**：  
>
> 1. 从 `readonly` 开始（最简单）  
> 2. 用 `Object.defineProperty` 修改属性描述符  
> 3. **不要**在装饰器中修改属性初始值（用 `set` 代替）  
> 4. 90% 场景用 `readonly` 和 `logProperty` 两类即可  
> 5. **重点**：装饰器只影响**运行时行为**，**不改变类型**（类型需用 `readonly` 关键字）



## 方法装饰器

------

### 定义

**方法装饰器 = 为类方法添加功能的"钩子"**
在**方法定义时**自动执行，用于**修改方法行为**（如添加日志、参数验证、性能统计等）。

> 💡 **类比**：
> 方法装饰器就像给操作添加"安全提示"——操作本身不变，但增加了额外检查。

------

### 基础语法

```typescript
// 1. 定义方法装饰器（接收 target + methodName + descriptor）
function logMethod(target: any, methodName: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;
  descriptor.value = function (...args: any[]) {
    console.log(`调用方法: ${methodName}，参数: ${args}`);
    return originalMethod.apply(this, args);
  };
  return descriptor;
}

// 2. 应用装饰器（方法定义前）
class Calculator {
  @logMethod
  add(a: number, b: number): number {
    return a + b;
  }
}

// 使用
const calc = new Calculator();
calc.add(2, 3); // ✅ 输出: 调用方法: add，参数: 2,3
```

------

### 参数解析

| 参数         | 说明                                                | 例子                                   |
| ------------ | --------------------------------------------------- | -------------------------------------- |
| `target`     | **类的原型对象**（实例方法） **类本身**（静态方法） | `target.add` → `Calculator` 的原型方法 |
| `methodName` | 方法名（字符串）                                    | `methodName` → `"add"`                 |
| `descriptor` | 方法描述符（包含 `value`、`writable` 等属性）       | `descriptor.value` → 原始方法函数      |

------

### 🌰 案例

#### 案例 1：基础日志（最常用）

```typescript
function logMethod(target: any, methodName: string, descriptor: PropertyDescriptor) {
  const original = descriptor.value;
  descriptor.value = function (...args: any[]) {
    console.log(`方法 ${methodName} 被调用，参数: ${args}`);
    return original.apply(this, args); 
  };
  return descriptor;
}

class UserService {
  @logMethod
  login(username: string, password: string) {
    console.log(`登录: ${username}`);
    return true;
  }
}

new UserService().login("user", "pass"); // 输出: 方法 login 被调用，参数: user,pass
```

#### 案例 2：方法性能统计

```typescript
function performance(target: any, methodName: string, descriptor: PropertyDescriptor) {
  const original = descriptor.value;
  descriptor.value = function (...args: any[]) {
    const start = performance.now();
    const result = original.apply(this, args);
    const end = performance.now();
    console.log(`方法 ${methodName} 耗时: ${end - start}ms`);
    return result;
  };
  return descriptor;
}

class MathService {
  @performance
  calculate(n: number) {
    let sum = 0;
    for (let i = 0; i < n; i++) {
      sum += i;
    }
    return sum;
  }
}

new MathService().calculate(1000000); // 输出: 方法 calculate 耗时: Xms
```

#### 案例 3：方法参数验证

```typescript
function validateAge(target: any, methodName: string, descriptor: PropertyDescriptor) {
  const original = descriptor.value;
  descriptor.value = function (age: number) {
    if (age < 0) {
      throw new Error("年龄不能为负数");
    }
    return original.apply(this, [age]);
  };
  return descriptor;
}

class Person {
  @validateAge
  setAge(age: number) {
    console.log(`年龄设置为: ${age}`);
  }
}

new Person().setAge(25); // ✅ 正常
new Person().setAge(-5); // ❌ 报错: 年龄不能为负数
```

------

### 关键点

1. **执行时机**  

   - ✅ **方法定义时**执行（不是调用时）
   - ❌ 不会在 `service.login()` 时触发

2. **必须返回 descriptor**  

   ```typescript
   function badDecorator(target: any, methodName: string, descriptor: PropertyDescriptor) {
     // ❌ 不能省略 return descriptor
     descriptor.value = ...;
     // return descriptor; // ✅ 必须返回
   }
   ```

3. **修改方法描述符**  

   - 通过 `descriptor.value` 替换原方法
   - 不能直接修改 `target[methodName]`

4. **静态方法支持**  

   ```typescript
   class Util {
     static formatNumber(n: number) { ... }
   }
   // 装饰器同样适用于静态方法
   ```

------

### 总结

> **方法装饰器 = `@decorator` 修饰方法，接收 `target`（原型）、`methodName`（方法名）、`descriptor`（方法描述符），在方法定义时修改其行为（如日志/验证/性能统计）。**

------

> 💡 **实践建议**：  
>
> 1. 从 `logMethod` 开始（最常用）  
> 2. 通过 `descriptor.value` 替换原方法  
> 3. **不要**在装饰器中直接调用方法（用 `apply`）  
> 4. 90% 场景用 `logMethod` 和 `validate` 两类即可  
> 5. **重点**：装饰器只影响**方法执行逻辑**，**不改变类型**

------

## 访问器装饰器

------

### 定义

**访问器装饰器 = 为类的 getter/setter 添加功能的"钩子"**
在**访问器定义时**自动执行，用于**修改属性的获取/设置行为**（如添加日志、验证、缓存等）。

> 💡 **类比**：
> 访问器装饰器就像给门锁加"监控摄像头"——门本身不变，但增加了进出记录。

------

### 基础语法

```typescript
// 1. 定义访问器装饰器（接收 target + accessorsName + descriptor）
function logAccessor(target: any, accessorName: string, descriptor: PropertyDescriptor) {
  const originalGet = descriptor.get;
  const originalSet = descriptor.set;
  
  descriptor.get = function() {
    console.log(`获取属性: ${accessorName}`);
    return originalGet ? originalGet.apply(this) : undefined;
  };
  
  descriptor.set = function(value) {
    console.log(`设置属性: ${accessorName} = ${value}`);
    if (originalSet) {
      originalSet.apply(this, [value]);
    }
  };
  
  return descriptor;
}

// 2. 应用装饰器（访问器定义前）
class User {
  private _name: string = "";
  
  @logAccessor
  get name(): string {
    return this._name;
  }
  
  @logAccessor
  set name(value: string) {
    this._name = value;
  }
}

// 使用
const user = new User();
user.name = "Alice"; // ✅ 输出: 设置属性: name = Alice
console.log(user.name); // ✅ 输出: 获取属性: name
```

------

### 参数解析

| 参数           | 说明                                                    | 例子                                |
| -------------- | ------------------------------------------------------- | ----------------------------------- |
| `target`       | **类的原型对象**（实例访问器） **类本身**（静态访问器） | `target.name` → `User` 的原型       |
| `accessorName` | 访问器名（字符串，如 `"name"`）                         | `accessorName` → `"name"`           |
| `descriptor`   | 访问器描述符（包含 `get`、`set` 等属性）                | `descriptor.get` → 原始 getter 函数 |

------

### 🌰 案例

#### 案例 1：基础日志（最常用）

```typescript
function logAccessor(target: any, accessorName: string, descriptor: PropertyDescriptor) {
  const originalGet = descriptor.get;
  const originalSet = descriptor.set;
  
  descriptor.get = function() {
    console.log(`获取: ${accessorName}`);
    return originalGet ? originalGet.call(this) : undefined;
  };
  
  descriptor.set = function(value) {
    console.log(`设置: ${accessorName} = ${value}`);
    if (originalSet) {
      originalSet.call(this, value);
    }
  };
  
  return descriptor;
}

class Profile {
  private _username: string = "";
  
  @logAccessor
  get username(): string {
    return this._username;
  }
  
  @logAccessor
  set username(value: string) {
    this._username = value;
  }
}

const profile = new Profile();
profile.username = "john_doe"; // 输出: 设置: username = john_doe
console.log(profile.username); // 输出: 获取: username
```

#### 案例 2：访问器验证（输入校验）

```typescript
function validateEmail(target: any, accessorName: string, descriptor: PropertyDescriptor) {
  const originalSet = descriptor.set;
  
  descriptor.set = function(value: string) {
    if (!value.includes('@')) {
      throw new Error('邮箱格式错误');
    }
    if (originalSet) {
      originalSet.call(this, value);
    }
  };
  
  return descriptor;
}

class Contact {
  private _email: string = "";
  
  @validateEmail
  set email(value: string) {
    this._email = value;
  }
  
  get email(): string {
    return this._email;
  }
}

const contact = new Contact();
contact.email = "invalid"; // ❌ 报错: 邮箱格式错误
contact.email = "valid@domain.com"; // ✅
```

#### 案例 3：访问器缓存（性能优化）

```typescript
function cached(target: any, accessorName: string, descriptor: PropertyDescriptor) {
  const originalGet = descriptor.get;
  let cache: any;
  
  descriptor.get = function() {
    if (cache === undefined) {
      cache = originalGet.call(this);
    }
    return cache;
  };
  
  return descriptor;
}

class DataFetcher {
  private data: any;
  
  constructor() {
    // 模拟数据加载
    this.data = { id: 1, name: "Test" };
  }
  
  @cached
  get fetchData() {
    return this.data;
  }
}

const fetcher = new DataFetcher();
console.log(fetcher.fetchData); // ✅ 加载数据
console.log(fetcher.fetchData); // ✅ 直接返回缓存
```

------

### 关键点

1. **执行时机**  

   - ✅ **访问器定义时**执行（不是调用时）
   - ❌ 不会在 `user.name = "Alice"` 时触发

2. **必须返回 descriptor**  

   - 与方法装饰器相同，必须返回修改后的 `descriptor`

3. **同时处理 get 和 set**  

   - 通过 `descriptor.get` 和 `descriptor.set` 分别修改

4. **静态访问器支持**  

   ```typescript
   class Config {
     static get apiKey() { ... }
     static set apiKey(value) { ... }
   }
   // 装饰器同样适用
   ```

------

### 总结

> **访问器装饰器 = `@decorator` 修饰 getter/setter，接收 `target`（原型）、`accessorName`（访问器名）、`descriptor`（访问器描述符），在访问器定义时修改其行为（如日志/验证/缓存）。**

------

> 💡 **实践建议**：  
>
> 1. 从 `logAccessor` 开始（最常用）  
> 2. 通过 `descriptor.get` 和 `descriptor.set` 修改访问器  
> 3. **不要**在装饰器中直接使用 `this`（用 `call` 保证上下文）  
> 4. 90% 场景用 `logAccessor` 和 `validate` 两类即可  
> 5. **重点**：装饰器只影响**访问器行为**，**不改变类型**

------

## 参数装饰器

------

### 定义

**参数装饰器 = 为类方法的参数添加功能的"钩子"**
在**参数定义时**自动执行，用于**修改参数行为**（如添加元数据、参数验证、类型转换等）。

> 💡 **类比**：
> 参数装饰器就像给输入框加"输入提示"——输入框本身不变，但增加了输入规则。

------

### 基础语法

```typescript
// 1. 定义参数装饰器（接收 target + methodName + parameterIndex）
function logParam(target: any, methodName: string, parameterIndex: number) {
  console.log(`参数 ${parameterIndex} 被装饰（方法: ${methodName}）`);
}

// 2. 应用装饰器（参数定义前）
class Calculator {
  add(@logParam a: number, @logParam b: number): number {
    return a + b;
  }
}

// 使用
const calc = new Calculator();
calc.add(1, 2); // ✅ 输出: 参数 0 被装饰（方法: add）, 参数 1 被装饰（方法: add）
```

------

### 参数解析

| 参数             | 说明                                                | 例子                                   |
| ---------------- | --------------------------------------------------- | -------------------------------------- |
| `target`         | **类的原型对象**（实例方法） **类本身**（静态方法） | `target.add` → `Calculator` 的原型方法 |
| `methodName`     | 方法名（字符串）                                    | `methodName` → `"add"`                 |
| `parameterIndex` | 参数索引（从 0 开始）                               | `parameterIndex` → `0`（第一个参数）   |

------

### 🌰 案例

#### 案例 1：基础日志（最常用）

```typescript
function logParam(target: any, methodName: string, parameterIndex: number) {
  console.log(`参数 ${parameterIndex} 被装饰（方法: ${methodName}）`);
}

class User {
  login(@logParam username: string, @logParam password: string) {
    console.log(`登录: ${username}`);
  }
}

new User().login("user", "pass"); 
// 输出: 参数 0 被装饰（方法: login）, 参数 1 被装饰（方法: login）
```

#### 案例 2：参数验证（输入校验）

```typescript
function validateEmail(target: any, methodName: string, parameterIndex: number) {
  const originalMethod = target[methodName];
  target[methodName] = function(...args: any[]) {
    if (parameterIndex === 0 && !args[0].includes('@')) {
      throw new Error('邮箱格式错误');
    }
    return originalMethod.apply(this, args);
  };
}

class Contact {
  @validateEmail
  register(@validateEmail email: string, password: string) {
    console.log(`注册: ${email}`);
  }
}

new Contact().register("invalid", "pass"); // ❌ 报错
new Contact().register("valid@domain.com", "pass"); // ✅
```

#### 案例 3：参数元数据（高级用法）

```typescript
function required(target: any, methodName: string, parameterIndex: number) {
  // 为参数添加元数据
  const metadata = Reflect.getMetadata('design:paramtypes', target, methodName) || [];
  metadata[parameterIndex] = true; // 标记为必填
  Reflect.defineMetadata('design:paramtypes', metadata, target, methodName);
}

class UserService {
  login(@required username: string, password: string) {
    console.log(`登录: ${username}`);
  }
}

// 获取元数据
const metadata = Reflect.getMetadata('design:paramtypes', UserService.prototype, 'login');
console.log(metadata); // [true, false]
```

------

### 关键点

1. **执行时机**  
   - ✅ **参数定义时**执行（不是调用时）
   - ❌ 不会在 `service.login(...)` 时触发
2. **不能直接修改参数**  
   - 参数装饰器**无法**在运行时修改参数值（如 `args[0] = "new"`）
   - 必须**替换整个方法**（通过 `target[methodName] = ...`）
3. **必须配合 `Reflect` 使用**（用于元数据）  
   - 如案例 3 使用 `Reflect` 保存元数据
4. **不支持静态方法**（TypeScript 限制）  
   - 静态方法的参数装饰器**无法工作**（需使用 `@Reflect` 等库）

------

### 总结

> **参数装饰器 = `@decorator` 修饰方法参数，接收 `target`（原型）、`methodName`（方法名）、`parameterIndex`（参数索引），在参数定义时添加元数据或验证，但需替换整个方法。**

------

> 💡 **实践建议**：  
>
> 1. 从 `logParam` 开始（最简单）  
> 2. **不要**试图在装饰器中直接修改参数值  
> 3. 90% 场景用 `logParam` 和 `required` 两类即可  
> 4. **重点**：参数装饰器**不能直接修改参数**，需替换方法  
> 5. **谨慎使用**：参数装饰器使用较少，除非需要元数据（如框架）
