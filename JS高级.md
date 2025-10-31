## JS高级



### 内核渲染

![image-20251027093158748](D:/桌面文件/笔记/My-Notes/img/image-20251027093158748.png)

浏览器内核的作用是将HTML和CSS加载

1. HTML将被Parser解析成DOM Tree（DOM树）|| 同时CSS将被Parser解析成Style Rules（样式规则）
2. DOM Tree加载时遇到JS代码将推给V8引擎来加载并进行操作
3. 在此之后DOM树与样式规则将混合，混成Render Tree（渲染树）
4. Render Tree将通过Layout布局器来进行布局
5. Painting将绘出元素
6. Display显示出绘制完成的页面



### V8引擎

![image-20251027092735587](D:/桌面文件/笔记/My-Notes/img/image-20251027092735587.png)

拿到js代码后会进行以下步骤

1. 浏览器内核将js源代码推入V8引擎
2. 词法分析（Tokenization）将字符流分解为Token流
3. 将Token流进行语法分析（parsing），通过预解析与全解析两个模块分析
4. 根据分析结果生成AST（抽象语法树）
5. 通过lgnition模块将AST转化为bytecode字节码
6. 字节码转化为机器码执行
7. 多次执行的函数将被标记为热函数
8. 热函数将被TurboFan模块直接转化为当前执行平台的对应的机器码
9. 并将优化后的函数机器码添加入lgnition模块生成的字节码对应函数位置
10. 优化后的机器码将优先被使用再生成运行结果
11. 如果优化的函数发生改变则执行去优化
12. 去优化（Deoptimization）：回退到字节码执行



### 变量提升

#### 核心概念定义

##### 1. **执行上下文**

- **GEC（Global Execution Context）**：全局执行上下文，代码执行时的最外层环境
- **FEC（Function Execution Context）**：函数执行上下文，函数调用时创建的执行环境
- **ECStack（Execution Context Stack）**：执行上下文栈，用于管理所有执行上下文的后进先出栈结构

##### 2. **变量环境**

- **GO（Global Object）**：全局对象，在浏览器中为 window 对象，存储全局变量和函数
- **VO（Variable Object）**：变量对象，存储当前上下文中的变量和函数声明
- **AO（Activation Object）**：活动对象，函数执行上下文中的变量对象



#### 包含关系

VO包含GO 和 AO GO是在GEC的VO中  AO是在FEC的VO中

![image-20251028082247345](D:/桌面文件/笔记/My-Notes/img/image-20251028082247345.png)



#### GEC全局执行上下文

其内部包含GO

```js
// GEC 的完整结构示意
GEC = {
    // 1. 变量环境（Variable Environment）
    variableEnvironment: {
        environmentRecord: {
            // 存储 var 声明的变量和函数声明
            // 在全局上下文中指向 GO
            bindings: GO  // 引用全局对象
        },
        outer: null  // 全局上下文没有外部引用
    },
    
    // 2. 词法环境（Lexical Environment）
    lexicalEnvironment: {
        environmentRecord: {
            // 存储 let/const 声明的变量（全局块级作用域）
            bindings: {
                // let/const 变量存储在这里
                // 例如：let blockScoped = "value"
            }
        },
        outer: null
    },
    
    // 3. this 绑定
    thisBinding: GO,  // 在全局上下文中，this 指向全局对象
    
    // 4. 外部环境引用（作用域链顶端）
    outerEnvironment: null,  // 全局上下文是作用域链的终点
    
    // 5. 可执行代码
    code: {
        type: "global",
        instructions: [...]  // 全局代码的字节码指令
    },
    
    // 6. 执行状态
    status: "executing" | "suspended" | "completed",
    
    // 7. 变量声明记录
    declarations: {
        varDeclarations: ["name", "foo"],     // var 声明的标识符
        functionDeclarations: ["foo", "bar"], // 函数声明的标识符  
        letDeclarations: ["blockScoped"],     // let 声明的标识符
        constDeclarations: ["PI"]             // const 声明的标识符
    }
}
```



#### 执行上下文生命周期

##### 阶段一：创建阶段（编译阶段）

```javascript
// 示例代码
var name = 'why';
function foo() {
    console.log('hello');
}
var bar = function() {
    console.log('world');
};
```

**执行过程：**

```
1. 创建 GEC（全局执行上下文）
2. 创建 GO（全局对象）
3. 变量提升：
   - GO.name = undefined
   - GO.foo = function(){...}（完整函数）
   - GO.bar = undefined（函数表达式）
4. 将 GEC 推入 ECStack
```

##### 阶段二：执行阶段（代码执行）

```
ECStack = [GEC]  // 初始状态

↓ 执行 name = 'why'
GO.name = 'why'  // 变量赋值

↓ 执行 bar 赋值  
GO.bar = function(){...}  // 函数表达式赋值

↓ 执行 foo()
创建 FEC（函数执行上下文）推入栈
ECStack = [FEC, GEC]

↓ foo() 执行完毕
FEC 出栈
ECStack = [GEC]  // 回到全局上下文
```

#### 完整执行流程示例

```javascript
var a = 1;
function outer() {
    var b = 2;
    function inner() {
        var c = 3;
        console.log(a + b + c);
    }
    inner();
}
outer();
```

**ECStack 变化过程：**

```
1. 编译阶段: ECStack = [GEC]
   - GO: a = undefined, outer = function

2. 执行 a = 1: ECStack = [GEC]
   - GO.a = 1

3. 调用 outer(): ECStack = [outerFEC, GEC]
   - outerFEC: b = 2, inner = function

4. 调用 inner(): ECStack = [innerFEC, outerFEC, GEC]
   - innerFEC: c = 3

5. inner() 执行完毕: ECStack = [outerFEC, GEC]

6. outer() 执行完毕: ECStack = [GEC]
```

#### 变量提升规则总结

##### 1. **函数声明** - 完全提升

```javascript
// 编译阶段：函数整体提升
foo(); // 可以正常执行
function foo() {
    console.log('hello');
}
```

##### 2. **var 变量** - 声明提升，赋值不提升

```javascript
// 编译阶段：变量声明提升，值为undefined
console.log(name); // undefined
var name = 'why';
```

##### 3. **函数表达式** - 按变量规则提升

```javascript
// 编译阶段：bar 提升为 undefined
console.log(bar); // undefined
var bar = function() {
    console.log('world');
};
```

##### 4. **let/const** - 存在暂时性死区

```javascript
console.log(a); // ReferenceError
let a = 1;
```

#### V8 引擎完整执行流程

```
JS源代码
     |
     v
词法分析 → Token流
     |
     v
语法分析 → AST
     |
     v
创建全局执行上下文 GEC
     |--- 创建 GO 对象
     |--- 变量/函数声明提升
     |--- 推入 ECStack
     |
     v
Ignition → 字节码
     |
     v
执行阶段（逐行执行）
     |--- 变量赋值
     |--- 函数调用（创建新的 FEC）
     |--- 作用域链解析
     |
     v
运行结果
```

#### 关键要点

1. **执行上下文栈**确保代码按正确的作用域顺序执行
2. **变量提升**发生在编译阶段，只提升声明不提升赋值
3. **函数声明**优先于变量声明提升
4. 每个函数调用都会创建新的函数执行上下文
5. 执行完毕的执行上下文会从栈中弹出，释放内存



### 函数提升

在语法分析生成AST抽象语法树后，V8引擎开始创建全局执行上下文（GEC），在创建全局对象（GO）的过程中会为函数声明分配内存并创建完整的函数对象，该对象包含函数的父级作用域引用和函数体代码，同时将全局变量声明初始化为undefined；接着将完整的GEC（包含GO引用）推入ECStack执行上下文栈，此时代码不会立即执行，而是通过Ignition解释器将AST转换为字节码，字节码逐行执行时进行变量赋值操作，当遇到函数调用时会创建对应的函数执行上下文（FEC）并推入ECStack，此时GEC的执行状态暂停，转而执行FEC中的指令，由于ECStack采用后进先出机制，FEC执行完毕后会从栈中弹出，恢复GEC继续执行，整个过程中作用域链的解析基于词法作用域在函数调用时动态构建。

![image-20251027195601281](D:/桌面文件/笔记/My-Notes/img/image-20251027195601281.png)



### 内存管理

**JavaScript 会在定义变量时自动分配内存，但分配方式因数据类型而异。**

#### 两种内存分配方式

##### 基本数据类型

- **存储位置**：直接在栈空间进行分配
- **数据类型**：String、Number、Boolean、Undefined、Null、Symbol、BigInt
- **特点**：访问速度快，大小固定，自动管理

##### 复杂数据类型

- **存储位置**：在堆内存中开辟空间
- **数据类型**：Object、Array、Function、Date 等
- **存储机制**：
  - 在堆内存中分配实际存储空间
  - 将这块空间的**指针（引用地址）** 返回给变量
  - 变量实际存储的是指向堆内存的引用

#### 内存结构

##### 栈结构 (Stack)

- 存储基本数据类型值
- 存储变量的引用（指向堆内存的指针）
- 按值访问，有序排列
- 自动分配和释放

##### 堆结构 (Heap)

- 存储复杂数据类型的实际内容
- 动态分配，大小不固定
- 通过引用（指针）访问
- 需要垃圾回收机制管理

##### 关键理解

变量本身存储在栈中，但对于复杂类型，变量存储的是**堆内存地址的引用**，而非实际数据本身。

![image-20251028152359738](D:/桌面文件/笔记/My-Notes/img/image-20251028152359738.png)

### 垃圾回收机制

#### 定义

- **GC**：Garbage Collection，自动回收程序不再使用的内存空间
- **垃圾**：程序不再需要的内存或之前用过以后不会再用的内存
- **特点**：JS引擎自动执行，对开发者相对无感

```javascript
let obj = {name: "obj"};  // 分配内存
obj = null;               // 原对象成为垃圾，需要回收
```



#### 回收策略

##### 标记清除算法

**流程：**

1. **标记阶段**：从根对象出发，标记所有可达对象
2. **清除阶段**：回收所有未标记的对象

**根对象包括：**

- 全局对象（window）
- 当前执行上下文中的变量
- DOM树

**缺点：**

- 内存碎片化
- 分配速度较慢



##### 引用计数算法

**原理：** 跟踪每个对象的引用次数，引用为0时立即回收

**问题：**

- 无法处理循环引用
- 计数器占用空间大



#### V8引擎回收

##### 分代式垃圾回收

将堆内存分为两个区域：

**新生代**

- **特点**：新产生的对象，容量小（1-8M）
- **算法**：Scavenge算法（Cheney算法）
- **过程**：
  1. 使用区 → 标记活动对象
  2. 复制到空闲区 → 排序整理
  3. 角色互换 → 使用区↔空闲区
- **晋升条件**：
  - 经历多次GC仍存活
  - 空闲区占用超过25%

**老生代**

- **特点**：存活时间长的对象，容量大
- **算法**：标记清除 + 标记整理
- **优化**：解决内存碎片问题



#### 性能优化策略

##### 并行回收

- 主线程执行GC时，开启多个辅助线程同时回收
- 主要用于新生代

##### 增量标记

- 将标记过程分成多个小步骤
- 与JS执行交替进行，减少卡顿
- **三色标记法**：
  - 白色：未标记
  - 灰色：自身标记，引用未标记
  - 黑色：自身和引用都已标记

##### 写屏障机制

- 黑色对象引用白色对象时，强制将白色改为灰色
- 保证增量标记的正确性

##### 惰性清理

- 标记完成后不立即清理
- 按需分批清理，减少单次暂停时间

##### 并发回收

- 辅助线程在后台执行GC
- 主线程继续执行JS，完全无阻塞



#### ❗参考文章

```txt
https://juejin.cn/post/6981588276356317214?searchId=20251009093548BAF5BA141C1496307A9A
```



#### 内存结构

```txt
Global Environment (全局环境)
├── Global Object (全局对象) → window
│   ├── var 声明的变量
│   ├── 内置属性 (document, console, etc.)
│   └── ...
├── Global Environment Record (全局环境记录)
│   ├── Declarative Environment Record (声明式环境记录)
│   │   ├── let 声明的变量
│   │   ├── const 声明的变量  
│   │   └── class 声明的类
│   └── Object Environment Record (对象环境记录)
│       └── var 声明的变量 (与window对象共享)
└── Outer Environment Reference → null
```



### 常见面试题



#### 变量函数提升

##### 代码段 1：全局变量修改

```javascript
var n = 100
function foo() {
    n = 200
}

foo()
console.log(n) // 输出：200
```

**解释**：函数 `foo` 内部修改的是全局变量 `n`，因为函数内部没有用 `var` 重新声明 `n`，所以操作的是外部的 `n`。

---

##### 代码段 2：变量提升

```javascript
var a = 100
function foo() {
    console.log(a) // 输出：undefined
    return
    var a = 100
}

foo()
```

**解释**：由于变量提升，函数内部的 `var a` 会被提升到函数顶部，但赋值不会。所以 `console.log(a)` 输出 `undefined`，`return` 语句后面的代码不会执行。

---

##### 代码段 3：局部变量遮蔽

```javascript
function foo() {
    console.log(n) // 输出：undefined
    var n = 200
    console.log(n) // 输出：200
}

var n = 100
foo()
```

**解释**：函数内部的 `var n` 会提升到函数顶部，在第一个 `console.log` 时 `n` 已声明但未赋值（`undefined`），第二个 `console.log` 时已赋值为 `200`。函数内部的 `n` 遮蔽了外部的全局变量 `n`。

---

##### 代码段 4：隐式全局变量

```javascript
function foo() {
    var a = b = 100
}

foo()
console.log(a) // 报错：a is not defined
console.log(b) // 输出：100
```

**解释**：`var a = b = 100` 等价于 `b = 100; var a = b;`。`b` 没有用 `var` 声明，成为隐式全局变量。`a` 用 `var` 声明，是函数局部变量，外部无法访问。

---

##### 代码段 5：作用域链

```javascript
var n = 100
function foo1() {
    console.log(n) // 输出：100
}

function foo2() {
    var n = 200
    console.log(n) // 输出：200
    foo1()
}

foo2()
console.log(n) // 输出：100
```

**解释**：

1. `foo2()` 内部的 `n` 是局部变量，输出 `200`
2. `foo1()` 在全局作用域定义，它内部的 `n` 指向全局变量 `n`（值为 `100`）
3. 最后的 `console.log` 输出全局变量 `n`（仍然是 `100`）

作用域在函数定义时确定，而不是调用时确定。

