# **Python笔记**



### 变量与数据类型

**1. 数据类型**  

- **基础类型**：  
  - 数值型：`int`（整型）、`float`（浮点）、`complex`（复数）  
  - 布尔型：`bool`（`True/False`）  
  - 字符串：`str`  
- **容器类型**：  
  - `list`（列表）、`tuple`（元组）、`dict`（字典）、`set`（集合）  
  - `bytes`（字节）、`NoneType`（`None`）  

**2. 变量操作**  
- **定义变量**：  
  ```python
  a = 10      # 单变量  
  b, c = 20, 30  # 多变量  
  name, age = "Alice", 25  # 不同类型  
  ```
- **类型转换**：  
  - `int(x)`：转整数（字符串需全数字）  
  - `float(x)`：转浮点（支持小数点）  
  - `str(x)`：转字符串  
  - `bool(x)`：转布尔（0/空值为`False`，非空为`True`）  
- **变量特性**：  
  - 动态类型（无需声明类型）  
  - 可重新赋值（`x = 10; x = "str"`）  
  - 删除变量：`del x`  

**3. 常量**  
- 全局常量用大写命名：  
  ```python  
  PI = 3.14159  
  ```

---

### 特殊符号与格式化输出
**1. 特殊符号**  
- `\n`：换行  
- `\t`：制表  
- `\\`：转义反斜杠  
- `\'`/`\"`：转义单/双引号  

**2. 格式化输出**  
- **旧式 `%`**：  
  ```python  
  print("%s is %d years old" % ("Alice", 25))  
  ```
- **`format()`**：  
  ```python  
  print("Name: {}, Age: {}".format("Bob", 30))  
  print("Name: {n}, Age: {a}".format(n="Bob", a=30))  
  ```
- **f-string（推荐）**：  
  ```python  
  name = "Charlie"  
  age = 35  
  print(f"{name} is {age + 1} next year")  
  ```

---

### 运算符与表达式
**1. 算术运算符**  
| 运算符 | 说明       |
| ------ | ---------- |
| `+`    | 加         |
| `-`    | 减         |
| `*`    | 乘         |
| `/`    | 除（浮点） |
| `//`   | 整除       |
| `%`    | 取余       |
| `**`   | 幂         |

**2. 赋值运算符**  
- `=`：简单赋值  
- 复合赋值：`a += 1` → `a = a + 1`  

**3. 关系运算符**  
| 运算符 | 说明     |
| ------ | -------- |
| `==`   | 等于     |
| `!=`   | 不等于   |
| `>`    | 大于     |
| `<`    | 小于     |
| `>=`   | 大于等于 |
| `<=`   | 小于等于 |

**4. 逻辑运算符**  
| 运算符 | 说明           |
| ------ | -------------- |
| `and`  | 与（全真为真） |
| `or`   | 或（一真为真） |
| `not`  | 非（取反）     |
- **短路原则**：  
  - `A and B`：若A为`False`，直接返回A，不计算B  
  - `A or B`：若A为`True`，直接返回A，不计算B  

---

**四、注意事项**  

1. **类型转换**：  
   - 字符串转数字需确保内容合法（如`int("12.3")`会报错）  
2. **变量赋值**：  
   - 多变量赋值需元素数量匹配，否则报错  
3. **逻辑运算**：  
   - `0, 0.0, "", None, [], {}`等视为`False`，非空为`True`  
4. **优先级**：  
   - `()` > `**` > `*, /, //, %` > `+,-` > `==, !=, >,<` > `not` > `and` > `or`  



以下是将异常捕获笔记整合到您现有笔记中的完整版本，保持原有标题层级和格式的一致性：

---



---

### 异常处理与错误管理

**1. 异常的基本概念**  
- **什么是异常？**  
  程序执行过程中发生的 **错误或意外事件**，例如：  
  - `ZeroDivisionError`（除以零）、`NameError`（变量未定义）、`TypeError`（类型不匹配）、`FileNotFoundError`（文件未找到）。  
- **作用**：中断程序正常流程，提供错误处理机制，避免崩溃。

---

**2. 异常处理结构**  
**语法**：  
```python
try:
    # 可能引发异常的代码
except 异常类型 as 异常变量:
    # 异常处理逻辑
else:
    # 未发生异常时执行
finally:
    # 无论是否异常，均执行（常用于资源清理）
```

- **关键部分说明**：  
  - `try`：包含可能引发异常的代码。  
  - `except`：定义如何处理特定类型异常，`as` 将异常对象赋值给变量。  
  - `else`：仅当未发生异常时执行。  
  - `finally`：无论是否异常均执行（如关闭文件、释放资源）。

---

**3. 异常捕获的进阶用法**  

**3.1 捕获特定异常**  
```python
try:
    print(1/0)
except ZeroDivisionError as e:
    print(f"除以零错误: {e}")  # 输出: division by zero
```

**3.2 捕获多个异常**  
```python
try:
    print(x)
except (NameError, TypeError) as e:
    print(f"发生错误: {e}")  # 一次捕获多个异常类型
```

**3.3 捕获所有异常**  
```python
try:
    # 可能出错的代码
except Exception as e:
    print(f"捕获到异常: {e}")
```
⚠️ **注意**：  
- 捕获 `Exception` 会忽略 `SystemExit`、`KeyboardInterrupt` 等系统级异常。  
- **不建议盲目捕获所有异常**，可能掩盖严重错误。

---

**4. 异常信息的获取**  
通过异常对象 `e` 可获取详细信息：  
- `str(e)`：直接输出错误消息。  
- `type(e).__name__`：获取异常类型名称。  
- `e.__traceback__`：获取堆栈跟踪（调试用）。  

**示例**：  
```python
try:
    print(1/0)
except Exception as e:
    print(f"错误类型: {type(e).__name__}")
    print(f"错误信息: {e}")
```
**输出**：  
```
错误类型: ZeroDivisionError
错误信息: division by zero
```

---

**5. 自定义异常**  
定义自定义异常类，继承 `Exception`：  
```python
class MyCustomError(Exception):
    def __init__(self, message):
        self.message = message
        super().__init__(f"自定义错误: {message}")

# 抛出异常
raise MyCustomError("发生了一些问题")
```

---

**6. 常见错误与最佳实践**  

**常见错误**：  
- **空的 `except` 块**：  
  ```python
  try:
      # 代码
  except:
      pass  # 不推荐！会隐藏错误
  ```
- **未处理异常信息**：  
  ```python
  except Exception as e:
      print("发生错误")  # 未利用 `e` 的详细信息
  ```

**最佳实践**：  
- **仅捕获需要处理的异常**：  
  ```python
  try:
      # 可能出错的代码
  except SpecificError as e:
      # 处理特定错误
  ```
- **记录异常信息**：使用 `logging` 模块记录详细日志。  
- **资源管理**：用 `with` 语句代替手动管理文件/数据库连接。  
- **避免在 `finally` 中忽略异常**：  
  如果 `finally` 中的代码可能引发异常，需额外处理。

---

**7. 示例代码**  

**示例1：结合 `else` 和 `finally`**  
```python
try:
    x = 10 / 2
except ZeroDivisionError as e:
    print("除以零错误！")
else:
    print("计算成功，结果为:", x)
finally:
    print("执行完毕")
```

**示例2：文件操作**  
```python
try:
    with open("data.txt", "r") as file:
        content = file.read()
except FileNotFoundError as e:
    print(f"文件未找到: {e}")
else:
    print("文件读取成功")
```

---

**8. 异常层级结构**  
Python 异常继承自基类 `BaseException`，常见子类包括：  
```
BaseException
├── Exception
│   ├── ArithmeticError
│   │   └── ZeroDivisionError
│   ├── LookupError
│   │   ├── IndexError
│   │   └── KeyError
│   ├── TypeError
│   ├── ValueError
│   └── NameError
├── GeneratorExit
├── KeyboardInterrupt
└── SystemExit
```

---

**总结**  
- **核心语法**：`try-except` 捕获异常，`as` 绑定异常对象。  
- **原则**：捕获具体异常，避免盲目捕获 `Exception`。  
- **资源管理**：善用 `finally` 或 `with` 语句。  
- **调试**：通过异常对象获取详细信息，结合日志记录。



### 进制转换

> ​	计算机在内存中以二进制【0和1】的形式存储数据，在二进制的基础上，计算机还支持八进制和十六进制这两种进制
>
> ​	一个二进制表示一个比特(bit)，也称为位，计算机处理数据的最小单位为字节【Byte】,1字节= 8位  ，比如：0010   1010
>
> ​	但是，我们生活中习惯使用十进制，所以当人与计算机之间进行交互的时候就要涉及到进制之间的转换
>
> ​	进制的表示有特定的符号集和进位制：
>
> ​		二进制：0和1，逢二进一
>
> ​		八进制：0~7
>
> ​		十进制：0~9，逢十进一
>
> ​		十六进制：0~9，a~f/A~F

#### 1.进制转换的适用场景

> 1. 编程与开发
>    低级编程：在嵌入式系统、驱动程序和硬件接口设计中，二进制和十六进制表示法非常常用。例如，内存地址、寄存器值通常用十六进制表示。
>      数据存储与传输：在网络协议中，数据包的格式和校验码常使用二进制或十六进制表示。
> 2. 数学与算法
>    位运算：在实现高效算法时，位运算可以显著提高性能。例如，位掩码操作、快速乘除法等。
>      密码学：许多加密算法依赖于特定的进制表示和转换来实现数据的安全性。
> 3. 教育与学习
>    基础教学：帮助学生理解不同进制之间的关系和转换方法，增强对数字系统的理解。
>      逻辑电路设计：在数字电路设计中，二进制是最基本的表示方式，理解和掌握进制转换有助于设计高效的逻辑电路。
> 4. 数据压缩与编码
>    压缩算法：某些数据压缩算法利用了不同进制之间的转换来减少数据量。例如，霍夫曼编码等。
>      字符编码：如ASCII、UTF-8等字符编码标准，它们本质上是对字符进行不同进制的编码表示。

#### 2.整数表示

> ```python
> # 二进制，八进制，十进制和十六进制在Python中都可以表示
> 
> # 8.1不同进制的整数表示
> # 8.1.1.二进制
> # 表示方式：用0b开头，b:bin
> n1 = 0b1110
> print(n1)  # 14
> 
> # 8.1.2八进制
> # 表示方式：用0o表示，o:oct
> n2 = 0o016
> print(n2)
> 
> # 8.1.3十进制
> n3=14
> print(n3)
> 
> # 8.1.4.十六进制
> # 表示方式：用0x表示，x:hex
> n4 = 0x0E
> print(n4)
> 
> ```

#### 3.代码实现进制转换

> |  函数  | 说明                   |
> | :----: | :--------------------- |
> | bin(x) | 将数字x转换为二进制    |
> | oct(x) | 将数字x转换为八进制    |
> | hex(x) | 将数字 x转换为十六进制 |

> ```python
> # 8.2代码实现进制转换
> #8.2.1将十进制转换为二进制、八进制、十六进制
> n5=12
> print(n5,"转为二进制：",bin(n5))
> print(n5,"转为八进制：",oct(n5))
> print(n5,"转为十六进制：",hex(n5))
> 
> #8.2.2将二进制、八进制、十六进制转换为十进制
> #ps:int(value,base):将一个表示特定进制数值的字符串转换为十进制整数
>  # value:表示要转换的数值的字符串
>  # base:表示待转换的字符串数值的进制，默认为10进制
> n6=0b1110
> print(n6,"转为十进制：",int(n6)) #int()函数中base参数默认为10进制
> print("十进制110转换位十进制为：",int("110",base=10))
> print("二进制0b1110转换位十进制为：",int("0b1110",base=2))
> print("八进制0o016转换位十进制为：",int("0o016",base=8))
> print("十六进制0x0E转换为十进制为：",int("0x0E",base=16))
> ```

#### 4.位运算符

> | 运算符 | 说明 |
> | :----: | :--: |
> |   &    |  与  |
> |   \|   |  或  |
> |   ^    | 异或 |
> |   ~    | 取反 |
> |   <<   | 左移 |
> |   >>   | 右移 |

> ```python
> # 与
> print(6 & 3)
> print(8 
> # 8.3位运算符（拓展了解）
> # 8.3.1位运算符
> # &:按位与运算符，两个二进制位都为1时，结果才为1
> print("&按位与运算",0b1110 & 0b1101)#执行完&运算之后为0b1100,转为十进制为12
> # |:按位或运算符，两个二进制位至少有一个为1时，结果才为1
> print("|按位或运算",0b1110 | 0b1101)#执行完|运算之后为0b1111,转为十进制为15
> # ^:按位异或运算符，两个二进制位不同时，结果才为1
> print("^按位异或运算",0b1110 ^ 0b1101)#执行完^运算之后为0b0011,转为十进制为3
> # ~:按位取反运算符，可以用~x = -(x + 1)这个公式表达
> print("~按位取反运算",~-5) #输出4
> print("~按位取反运算",~5) #输出-6
> # <<:左移运算符，将二进制数左移n位
> # >>:右移运算符，将二进制数右移n位
> # 左移与右移
> print(6 << 2) #6转变为二进制是0110，左移2位，结果为：011000
> print(6 >> 2) #6转变为二进制是0110，右移1位，结果为011，再向右移一位，结果为001
> print(8 << 3) #8转变为二进制是1000，左移3位，结果为：1000000
> print(8 >> 3) #8转变为二进制是1000，右移1位结果为0100，再向右移一位，结果为0010，再向右移一位，结果为0001
> print(8 >> 2)#输出2，8的二进制是1000，右移1位，结果为0100，再向右移一位，结果为0010，
> 
> ```

### Math模块方法列表

#### **1. 常量**  

- `math.pi`：圆周率 π（约 3.1415926535...）  
- `math.e`：自然常数 e（约 2.7182818284...）  

#### **2. 常用函数**  

| 函数名               | 用途                             | 示例代码                                        |
| -------------------- | -------------------------------- | ----------------------------------------------- |
| `math.sqrt(x)`       | 计算平方根                       | `math.sqrt(16) → 4.0`                           |
| `math.pow(x, y)`     | 计算 x 的 y 次幂                 | `math.pow(2, 3) → 8.0`                          |
| `math.log(x, base)`  | 计算以 base 为底的对数           | `math.log(100, 10) → 2.0`                       |
| `math.log10(x)`      | 计算以 10 为底的对数（快捷方式） | `math.log10(100) → 2.0`                         |
| `math.factorial(x)`  | 计算 x 的阶乘                    | `math.factorial(5) → 120`                       |
| `math.ceil(x)`       | 向上取整                         | `math.ceil(3.14) → 4`                           |
| `math.floor(x)`      | 向下取整                         | `math.floor(3.14) → 3`                          |
| `math.fabs(x)`       | 取绝对值                         | `math.fabs(-3.14) → 3.14`                       |
| `math.isnan(x)`      | 检查是否为非数字（NaN）          | `math.isnan(3.14) → False`                      |
| `math.isclose(a, b)` | 判断两个数是否近似相等           | `math.isclose(3.14, 3.141, abs_tol=0.3) → True` |
| `math.trunc(x)`      | 截断为整数（去除小数部分）       | `math.trunc(3.14) → 3`                          |
| `math.radians(deg)`  | 将角度转换为弧度                 | `math.radians(30) → 0.523...`                   |
| `math.sin(x)`        | 计算正弦（x 为弧度）             | `round(math.sin(math.radians(30)), 1) → 0.5`    |

---

### Random模块方法列表

#### **1. 基础方法**  

| 函数名                 | 用途                             | 示例代码                     |
| ---------------------- | -------------------------------- | ---------------------------- |
| `random.randint(a, b)` | 生成 [a, b] 范围内的随机整数     | `random.randint(1, 5) → 3`   |
| `random.random()`      | 生成 [0.0, 1.0) 范围内的随机小数 | `random.random() → 0.789...` |

#### **2. 进阶方法**  

| 函数名                                | 用途                                     | 示例代码                           |
| ------------------------------------- | ---------------------------------------- | ---------------------------------- |
| `random.uniform(a, b)`                | 生成 [a, b] 范围内的随机小数             | `random.uniform(1, 100) → 45.6`    |
| `random.randrange(start, stop, step)` | 生成指定范围和步长的随机整数             | `random.randrange(0, 100, 2) → 42` |
| `random.choice(seq)`                  | 从序列（列表、字符串等）中随机选一个元素 | `random.choice([1,2,3]) → 2`       |



---



### 数据集合

| **数据结构**           | **是否可变** | **是否有序**          | **是否允许重复**       | **定义符号**      | **常见操作**                                                 |
| ---------------------- | ------------ | --------------------- | ---------------------- | ----------------- | ------------------------------------------------------------ |
| **列表（List）**       | ✅ 可变       | ✅ 有序                | ✅ 允许重复             | `[]` 或 `list()`  | - 添加：`append()`<br>- 修改：`list[索引] = 新值`<br>- 删除：`remove()`/`pop()`/`del`<br>- 切片：`list[start:end]` |
| **元组（Tuple）**      | ❌ 不可变     | ✅ 有序                | ✅ 允许重复             | `()` 或 `tuple()` | - 访问：`tuple[索引]`<br>- 拼接：`tuple1 + tuple2`<br>- 不可修改（如需修改需转为列表） |
| **集合（Set）**        | ✅ 可变       | ❌ 无序                | ❌ 不允许重复           | `{}` 或 `set()`   | - 添加：`add()`<br>- 删除：`remove()`/`discard()`<br>- 运算：`union()`（并集）、`intersection()`（交集）、`difference()`（差集） |
| **字典（Dictionary）** | ✅ 可变       | ✅ 有序（Python 3.7+） | 键 ❌ 唯一，值 ✅ 可重复 | `{}` 或 `dict()`  | - 添加/修改：`dict[key] = value`<br>- 删除：`pop()`/`del`<br>- 访问：`dict[key]`/`get()`<br>- 遍历：`keys()`/`values()`/`items()` |

---





### 列表

#### **1. 基础操作**  

| 方法/语法              | 用途         | 示例代码                |
| ---------------------- | ------------ | ----------------------- |
| `list = []`            | 创建空列表   | `list001 = []`          |
| `list = [x1, x2, ...]` | 创建非空列表 | `list002 = [1, 2, "a"]` |

#### **2. 增删改查**  

| 方法/语法           | 用途                      | 示例代码                     |
| ------------------- | ------------------------- | ---------------------------- |
| `list.append(x)`    | 在末尾添加元素            | `list_num.append(998)`       |
| `list.insert(i, x)` | 在索引 i 处插入元素       | `list_num.insert(0, 999)`    |
| `list[i] = x`       | 修改索引 i 处的元素       | `list_num[0] = 1000`         |
| `del list[i]`       | 删除索引 i 处的元素       | `del list_num[0]`            |
| `list.remove(x)`    | 删除第一个匹配的元素      | `list_num.remove(100)`       |
| `list.pop(i)`       | 删除并返回索引 i 处的元素 | `list_num.pop(0)`            |
| `list[a:b]`         | 切片操作（获取子列表）    | `list_num[0:3] → [12,24,35]` |
| `x in list`         | 判断元素是否在列表中      | `5 in list1 → True`          |
|                     |                           |                              |

#### **3. 更多操作**  

| 方法/语法       | 用途                    | 示例代码                      |
| --------------- | ----------------------- | ----------------------------- |
| `list1 + list2` | 合并两个列表            | `list3 = list1 + list2`       |
| `list * n`      | 重复列表 n 次（浅复制） | `list2 * 2 → [4,5,6,4,5,6]`   |
| `for x in list` | 遍历列表元素            | `for k in list_num: print(k)` |



#### 4. 列表切片

- 列表切片允许通过索引访问列表的一部分。

- 示例：

  ```python
  list1 = [1, 2, 3, 4, 5, 6, 7, 8, 9]
  print(list1[2:])  # 输出: [3, 4, 5, 6, 7, 8, 9]
  print(list1[:5])  # 输出: [1, 2, 3, 4, 5]
  print(list1[2:5])  # 输出: [3, 4, 5]
  print(list1[::2])  # 输出: [1, 3, 5, 7, 9]
  print(list1[::-1])  # 输出: [9, 8, 7, 6, 5, 4, 3, 2, 1]
  ```



#### 5. 深拷贝与浅拷贝

- **浅拷贝**：创建一个新的对象，但内部的对象仍然是引用原来的对象。

- **深拷贝**：创建一个新的对象，包括其内部的所有嵌套对象。

- 示例：

  ```python
  import copy
  
  list2 = [1, 2, 3, 4, 5]
  list3 = list2  # 浅拷贝
  list4 = copy.deepcopy(list2)  # 深拷贝
  
  list2[0] = 100
  print(list3)  # 输出: [1, 2, 3, 4, 5]
  print(list4)  # 输出: [1, 2, 3, 4, 5]
  ```



#### 6. 列表推导式

1. **语法**  
   `[表达式 for 变量 in 可迭代对象 if 条件]`  
   *示例*：  
   ```python
   [x*2 for x in range(5)]          # [0, 2, 4, 6, 8]
   [x for x in range(10) if x%2]    # [1,3,5,7,9]
   [a+b for a in 'abc' for b in '12']  # ['a1','a2','b1','b2','c1','c2']
   ```

2. **典型场景**  
   - 生成序列：`[n**2 for n in range(1,6)]` → `[1,4,9,16,25]`  
   - 过滤数据：`[n**2 for n in range(1,10) if n%2==1]` → `[1,9,25,49,81]`  
   - 类型筛选：`[x for x in lst if isinstance(x, (int, float))]`

3. **优点与注意**  
   - ✅ 简洁高效，适合简单逻辑。  
   - ⚠️ 避免多层嵌套（如 `for` + `if` 多层），影响可读性。

---



#### 7. 二维列表

1. **定义与操作**  
   - 定义：`matrix = [[1,2],[3,4]]`  
   - 遍历：  
     ```python
     for row in matrix:
         for num in row:
             print(num)
     ```
   - 修改：`matrix[0][1] = 99`  
   - 删除：`del matrix[1][0]` 或 `del matrix[0]`

2. **典型应用**  
   - **学生成绩表**：  
     ```python
     scores = [["张三", 85, 90], ["李四", 78, 88]]
     for s in scores:
         avg = sum(s[1:])/len(s[1:])
         print(f"{s[0]} 平均分: {avg:.1f}")
     ```
   - **图像二值化**：  
     ```python
     image = [[0,255,255],[0,0,255]]
     binary = [[1 if p>0 else 0 for p in row] for row in image]
     ```

3. **列表推导式处理**  
   - 展平二维列表：  
     ```python
     [x for row in matrix for x in row]  # [1,2,3,4]
     ```
   - 元素变换：  
     ```python
     [[x*2 for x in row] for row in matrix]  # [[2,4],[6,8]]
     ```

#### 8. 单词列表

- **定义**：由字符串分割生成的单词集合，通常为列表形式（`['word1', 'word2', ...]`）。
- **作用**：用于文本处理（如词频统计、清洗、分析）。
- **示例**：
  ```python
  "hello world".split()  # ['hello', 'world']
  ```

---

##### `split()` 方法详解
- **功能**：按指定分隔符将字符串拆分为列表。
- **语法**：
  ```python
  str.split(sep=None, maxsplit=-1)
  ```
  - `sep`：分隔符（默认为空白符，如空格、换行）。
  - `maxsplit`：最大分割次数（默认不限制）。
- **常见用法**：
  ```python
  "a,b,c".split(',')       # ['a', 'b', 'c']
  "apple orange".split()   # ['apple', 'orange']
  "1:2:3".split(':', 1)    # ['1', '2:3']  # 最多分割一次
  ```



#####  示例代码
```python
text = "hello world hello"
words = text.split()  # ['hello', 'world', 'hello']
from collections import Counter
print(Counter(words))  # {'hello': 2, 'world': 1}
```



### 元组

---

#### **1. 元组的基本使用**

- **定义**：元组是不可变的有序序列，使用小括号 `()` 定义。
- **初始化规则**：
  
  - 空元组：`tuple2 = ()`
  - 单元素元组：必须加逗号（否则会被识别为普通变量）。
  - 示例：
    ```python
    tuple1 = (1, 2, 3, 4, 5)        # 多元素元组
    tuple2 = ()                     # 空元组
    tuple3 = (22,)                  # 单元素元组（必须加逗号）
    tuple4 = (22)                   # 非元组，会被识别为整数 22
    ```

#### **2. 查询和非查询**
- **不可变性**：元组一旦创建，不能修改、删除或添加元素。
- **基本操作**：
  
  - 获取长度：`len(tuple5)`
  - 索引访问：`tuple5[0]`
  - 遍历元素：`for i in tuple5: print(i)`
  - 示例：
    ```python
    tuple5 = (11, 22, 33, 44, 55)
    print(len(tuple5))     # 输出: 5
    print(tuple5[0])       # 输出: 11
    for i in tuple5:
        print(i)           # 输出: 11 22 33 44 55
    ```



#### **3. 元组的“添加”**
- **不可变性限制**：元组无法直接添加元素（会报错）。
- **替代方法**：
  - 通过拼接生成新元组：`tuple6 + (66,)`
  - 示例：
    ```python
    tuple6 = (11, 22, 33, 44, 55)
    # tuple6.append(66)  # 报错：AttributeError
    new_tuple = tuple6 + (66,)  # 合法，生成新元组
    ```

#### **4. 元组切片操作**
- **语法**：`tuple[start:stop:step]`，与列表切片相同。
- **示例**：
  
  ```python
  tuple7 = (1, 2, 3, 4, 5)
  print(tuple7[1:3])  # 输出: (2, 3)
  ```

#### **5. 成员运算符**
- **`in` 和 `not in`**：检查元素是否存在于元组中。
- **示例**：
  ```python
  tuple8 = (1, 2, 3, 4, 5)
  print(2 in tuple8)        # 输出: True
  print(6 not in tuple8)    # 输出: True
  ```

#### **6. 常用内置函数**
- **`len()`**：返回元组长度。
- **`max()` / `min()`**：返回最大值或最小值。
- **示例**：
  ```python
  tuple9 = (1, 2, 3, 4, 5)
  print(len(tuple9))  # 输出: 5
  print(max(tuple9))  # 输出: 5
  print(min(tuple9))  # 输出: 1
  ```



---

#### **7. 常见问题**
1. **如何修改元组？**  
   
   - 直接修改元素会报错，可通过转换为列表再转换回元组：
     ```python
     my_tuple = (1, 2, 3)
     new_list = list(my_tuple)
     new_list.append(4)
     new_tuple = tuple(new_list)
     ```
   
2. **如何查找元组中所有匹配项？**  
   
   - 使用列表推导式：
     ```python
     tuple10 = (1, 2, 3, 2, 4)
     indices = [i for i, x in enumerate(tuple10) if x == 2]
     print(indices)  # 输出: [1, 3]
     ```
   
3. **元组排序**：  
   - 使用 `sorted()` 函数返回排序后的列表：
     ```python
     numbers = (3, 1, 4, 1, 5)
     sorted_numbers = sorted(numbers)  # [1, 1, 3, 4, 5]
     ```

### 字典

---

#### **1. 字典的基本使用**

- **定义**：字典是无序的键值对集合，使用大括号 `{}` 定义，键必须是不可变类型（如字符串、整数、元组），值可以是任意类型。

- **初始化规则**：

  - 空字典：`dict1 = {}`

  - 单个键值对：`dict2 = {"name": "张三"}`

  - 示例：

    ```python
    dict1 = {"name": "张三", "age": 18}  # 多键值对
    dict2 = {}                          # 空字典
    dict3 = dict(x=10, y=20)            # 使用 dict() 构造函数
    ```

#### **2. 键值对的访问**

- **通过键访问值**：

  ```python
  info_dict = {"name": "赵四", "age": 20, "gender": "male"}
  age = info_dict["age"]  # 输出: 20
  ```

- **使用 `.get()` 方法**：

  ```python
  age1 = info_dict.get("age")        # 输出: 20
  hobby = info_dict.get("hobby")     # 输出: None（键不存在时不报错）
  ```

#### **3. 遍历字典**

- **获取所有键**：

  ```python
  for key in info_dict.keys():
      print(key, info_dict[key])  # 输出键和对应的值
  ```

- **获取所有值**：

  ```python
  for value in info_dict.values():
      print(value)  # 输出: 赵四 20 male
  ```

- **获取键值对**：

  ```python
  for key, value in info_dict.items():
      print(key, value)  # 输出: name 赵四 age 20 gender male
  ```

#### **4. 字典的操作**

- **添加键值对**：

  ```python
  info_dict["hobby"] = "reading"
  print(info_dict)  # 输出: {'name': '赵四', 'age': 20, 'gender': 'male', 'hobby': 'reading'}
  ```

- **修改键值对**：

  ```python
  info_dict["age"] = 21
  print(info_dict)  # 输出: {'name': '赵四', 'age': 21, 'gender': 'male'}
  ```

- **删除键值对**：

  ```python
  del info_dict["gender"]
  print(info_dict)  # 输出: {'name': '赵四', 'age': 20}
  ```

- **合并字典**：

  ```python
  dict_a = {"a": 1, "b": 2}
  dict_b = {"c": 3, "d": 4}
  dict_a.update(dict_b)
  print(dict_a)  # 输出: {'a': 1, 'b': 2, 'c': 3, 'd': 4}
  ```

#### **5. 常见应用场景**

- **统计列表元素出现次数**：

  ```python
  list1 = [11, 22, 11, 33, 44, 55, 66, 55, 66]
  count_dict = {}
  for ele in list1:
      count_dict[ele] = list1.count(ele)
  print(count_dict)  # 输出: {11: 2, 22: 1, 33: 1, 44: 1, 55: 2, 66: 2}
  ```

- **学生信息处理**：

  ```python
  students = [
      {"name": "小花", "age": 19, "score": 90, "gender": "女"},
      {"name": "明明", "age": 20, "score": 40, "gender": "男"},
      {"name": "华仔", "age": 18, "score": 90, "gender": "女"},
      {"name": "静静", "age": 16, "score": 90, "gender": "不明"},
      {"name": "Tom", "age": 17, "score": 59, "gender": "不明"},
      {"name": "Bob", "age": 18, "score": 90, "gender": "男"}
  ]
  
  # 统计不及格学生数量
  fail_count = sum(1 for student in students if student["score"] < 60)
  print(f"不及格学生数量: {fail_count}")
  
  # 删除性别不明的学生
  students = [student for student in students if student["gender"] != "不明"]
  ```

#### **6. 注意事项**

- **键的唯一性**：重复键会被覆盖。

  ```python
  dict1 = {"name": "张三", "name": "李四"}
  print(dict1)  # 输出: {'name': '李四'}
  ```

- **键的不可变性**：键必须是不可变类型（如字符串、整数、元组），值可以是任意类型。

---

### 集合

---

#### **1. 集合的基本使用**

- **定义**：集合是无序、不重复的可变序列，使用大括号 `{}` 或 `set()` 创建。

- **初始化规则**：

  - 空集合：`empty_set = set()`

  - 去重集合：`set1 = set([1, 2, 2, 3])`  # 输出: {1, 2, 3}

  - 示例：

    ```python
    set1 = {1, 2, 3}
    set2 = set("hello")  # 输出: {'h', 'e', 'l', 'o'}
    ```

#### **2. 集合的操作**

- **添加元素**：

  ```python
  set1 = {1, 2, 3}
  set1.add(4)          # 添加单个元素
  print(set1)          # 输出: {1, 2, 3, 4}
  ```

- **删除元素**：

  ```python
  set1.remove(2)       # 如果元素不存在会报错
  set1.discard(5)      # 如果元素不存在不会报错
  ```

#### **3. 集合运算**

- **并集**：`set1 | set2` 或 `set1.union(set2)`

- **交集**：`set1 & set2` 或 `set1.intersection(set2)`

- **差集**：`set1 - set2` 或 `set1.difference(set2)`

- **对称差集**：`set1 ^ set2` 或 `set1.symmetric_difference(set2)`

  ```python
  set_a = {1, 2, 3}
  set_b = {3, 4, 5}
  
  print(set_a | set_b)  # 并集: {1, 2, 3, 4, 5}
  print(set_a & set_b)  # 交集: {3}
  print(set_a - set_b)  # 差集: {1, 2}
  print(set_a ^ set_b)  # 对称差集: {1, 2, 4, 5}
  ```

#### **4. 应用场景**

- **去除列表重复元素**：

  ```python
  lst = [1, 2, 2, 3, 4, 4, 5]
  unique_lst = list(set(lst))  # 转换后顺序可能改变
  ```

- **快速判断元素是否存在**：

  ```python
  set1 = {1, 2, 3}
  print(2 in set1)  # 输出: True
  ```

- **集合运算**：

  ```python
  # 用户A和用户B的共同关注
  user_a_followers = {"Alice", "Bob", "Charlie"}
  user_b_followers = {"Bob", "David", "Eve"}
  common_followers = user_a_followers & user_b_followers  # 输出: {"Bob"}
  ```

#### **5. 注意事项**

- **不可变元素**：集合中的元素必须是不可变类型（如整数、字符串、元组）。

  ```python
  set1 = {1, 2, [3, 4]}  # 报错：列表是不可哈希的
  ```

- **空集合创建**：空集合必须用 `set()`，不能用 `{}`（后者是空字典）。

### 简单算法

#### 冒泡排序

- 冒泡排序是一种简单的排序算法，重复地遍历要排序的列表，比较相邻的元素并根据需要交换它们的位置。

- 示例：

  ```python
  list001 = [34, 45, 6, 74, 45, 5, 6, 7, 10, 67]
  for i in range(len(list001) - 1):
      for j in range(len(list001) - 1 - i):
          if list001[j] > list001[j + 1]:
              list001[j], list001[j + 1] = list001[j + 1], list001[j]
  print(list001)  # 输出: [5, 6, 6, 7, 10, 34, 45, 45, 67, 74]
  ```


---

#### 选择排序

- 选择排序是一种简单直观的排序算法，首先从待排序的数据元素中选出最小（或最大）的一个元素，存放在序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（或最大）元素，然后放到已排序序列的末尾。

- 示例：

  ```python
  list001 = [3, 1, 5, 2, 4]
  for i in range(len(list001) - 1):
      for j in range(i + 1, len(list001)):
          if list001[i] > list001[j]:
              list001[i], list001[j] = list001[j], list001[i]
  print(list001)  # 输出: [1, 2, 3, 4, 5]
  ```


---
