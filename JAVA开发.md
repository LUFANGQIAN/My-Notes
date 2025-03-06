# JAVA开发





## 碎碎念



### 关于类型转换

------

Q：关于C与JAVA对待数据类型赋值的严格性方面，貌似我将一个浮点型的字面量赋值给一个整数类型的变量，java会报错，而C会截取小数部分只保留整数部分，并不会报错，只会失真

A：是的，你观察得非常正确。Java 和 C 在处理不同类型数据赋值时确实有不同的行为。下面是关于这两种语言在类型转换方面的详细解释：

Java 的严格类型检查

在 Java 中，类型转换是非常严格的。如果你尝试将一个浮点型字面量直接赋值给一个整型变量，编译器会报错，因为这会导致精度丢失。你需要显式地进行类型转换。

示例

```java
public class TypeConversionExample {
    public static void main(String[] args) {
        // 错误：无法将 double 赋值给 int
        // int a = 3.14; // 编译错误

        // 正确：显式类型转换
        int a = (int) 3.14; // 小数部分被截断，a 的值为 3
        System.out.println("a: " + a); // 输出 3
    }
}
```

C 的隐式类型转换

在 C 语言中，如果将一个浮点型字面量赋值给一个整型变量，编译器不会报错，而是会自动进行类型转换，小数部分会被截断，只保留整数部分。这种转换可能会导致数据失真，但编译器不会阻止这种操作。

示例

```c
#include <stdio.h>

int main() {
    // 正确：C 会自动截断小数部分
    int a = 3.14; // 小数部分被截断，a 的值为 3
    printf("a: %d\n", a); // 输出 3

    return 0;
}
```

详细说明

- **Java**:
  - **严格类型检查**：Java 编译器会强制要求类型匹配。如果类型不匹配，编译器会报错。
  - **显式类型转换**：如果你需要将浮点型转换为整型，必须使用类型转换操作符 `(int)` 显式地进行转换。例如：
    ```java
    double d = 3.14;
    int i = (int) d; // 小数部分被截断
    ```

- **C**:
  - **隐式类型转换**：C 语言允许隐式的类型转换。如果将浮点型赋值给整型变量，编译器会自动截断小数部分，只保留整数部分。
  - **数据失真**：这种隐式转换可能会导致数据失真，但编译器不会报错。例如：
    ```c
    double d = 3.14;
    int i = d; // 小数部分被截断
    ```

总结

- **Java**:
  - 类型检查严格，不允许隐式类型转换（从浮点型到整型）。
  - 需要显式类型转换，使用 `(int)` 操作符。

- **C**:
  - 允许隐式类型转换，编译器会自动截断小数部分。
  - 可能导致数据失真，但编译器不会报错。

理解这些差异对于编写正确的代码非常重要。希望这些解释对你有所帮助！如果你有其他问题或需要进一步的示例，请告诉我。

------











### 关于隐式转换

------

Q：难道说JAVA没有隐式转换嘛，或者严谨的说只有在特定的情况下才会有隐式转换

A：是的，你的理解是正确的。在 Java 中，只有在提升精度或扩大容器大小（即从较小的数据类型转换为较大的数据类型）时，才会发生隐式类型转换，并且这种转换不会导致数据丢失或精度损失，因此不会报错。

隐式类型转换（自动类型提升）的情况

1. **整数类型**：
   - `byte` -> `short` -> `int` -> `long`
   - 例如：
     ```java
     byte b = 10;
     short s = b; // byte -> short，隐式转换
     int i = s;   // short -> int，隐式转换
     long l = i;  // int -> long，隐式转换
     ```

2. **浮点类型**：
   - `float` -> `double`
   - 例如：
     ```java
     float f = 3.14f;
     double d = f; // float -> double，隐式转换
     ```

3. **混合运算**：
   - 在表达式中，如果操作数的类型不同，Java 会自动将较小类型的变量提升为较大类型的变量。
   - 例如：
     ```java
     int a = 5;
     double b = 3.14;
     double result = a + b; // a 会被自动提升为 double 类型
     ```

不会发生隐式转换的情况

1. **从较大类型到较小类型**：
   
   - 例如，从 `double` 到 `int` 或从 `long` 到 `int`。
   - 这种情况下需要显式类型转换，因为可能会导致数据丢失或精度损失。
   - 例如：
     ```java
     double d = 3.14;
     int i = (int) d; // 显式转换，小数部分被截断
     ```
   
2. **从浮点类型到整数类型**：
   - 例如，从 `float` 到 `int` 或从 `double` 到 `int`。
   - 这种情况下也需要显式类型转换。
   - 例如：
     ```java
     float f = 3.14f;
     int i = (int) f; // 显式转换，小数部分被截断
     ```

示例代码

```java
public class TypeConversionExample {
    public static void main(String[] args) {
        // 自动类型提升
        byte b = 10;
        short s = b; // byte -> short，隐式转换
        int i = s;   // short -> int，隐式转换
        long l = i;  // int -> long，隐式转换
        System.out.println("b: " + b); // 输出 10
        System.out.println("s: " + s); // 输出 10
        System.out.println("i: " + i); // 输出 10
        System.out.println("l: " + l); // 输出 10

        // 浮点类型到双精度类型
        float f = 3.14f;
        double d = f; // float -> double，隐式转换
        System.out.println("f: " + f); // 输出 3.14
        System.out.println("d: " + d); // 输出 3.14

        // 混合运算
        int x = 5;
        double y = 3.14;
        double result = x + y; // x 会被自动提升为 double 类型
        System.out.println("result: " + result); // 输出 8.14

        // 从较大类型到较小类型，需要显式转换
        double largeNumber = 123.456;
        int truncatedNumber = (int) largeNumber; // 显式转换，小数部分被截断
        System.out.println("truncatedNumber: " + truncatedNumber); // 输出 123

        // 从浮点类型到整数类型，需要显式转换
        float smallFloat = 3.14f;
        int smallInt = (int) smallFloat; // 显式转换，小数部分被截断
        System.out.println("smallInt: " + smallInt); // 输出 3
    }
}
```



总结

- **隐式类型转换**：只会在从小类型到大类型（如 `int` 到 `long`、`float` 到 `double`）时发生，不会导致数据丢失或精度损失。
- **显式类型转换**：从大类型到小类型（如 `double` 到 `int`），或者从浮点类型到整数类型，需要使用显式类型转换 `(type)`，否则会导致编译错误。

理解这些规则有助于编写正确和安全的代码。希望这些解释对你有所帮助！如果你有其他问题或需要进一步的示例，请告诉我。













## 基础框架

```java
public class HelloWorld{
    public static void main(String[] args)
    {
        //关于下面这两行实现的效果其实是一样的
        //print与println只是带ln会默认在语句结尾加一个换行符
        
        System.out.print("Yeah,It's me again,HelloWorld \n");
        System.out.println("Yeah,It's me again,HelloWorld");
    }
}
```

> 注意：java对大小写敏感，注意System不要打错了



## 注释

定义：在程序的指定位置添加说明性信息（就是解释代码用的）

注释分类两种：单行注释  与  多行注释 还有个文档注释

> 祖传的注释方法



### 单行注释

使用  **`//`**  双斜杠进行注释

```java
//这是单行注释，双斜杠后整行的字符都是注释，不会被编译使用
```



### 多行注释

使用  **`/*`** 作为开头，**`*/`** 作为结尾

```java
/*
	在两个符号内
		无论行数		或位置
都将被作为注释忽略掉，不会
被编译
*/
```



### 文档注释

使用  **`/**`** 作为开头，**`**/`** 作为结尾

这个比较特殊，这种注释可以使用java的文档工具将文档注释自动生成技术文档





## 字面量

这东西就是可以直接使用的常量，比如`int = 42；`中 42 就是字面量，并且他是一个整形字面量

其他的数据类型相同，但是有个比较特殊的，就是空型：NULL，他无法直接打印,也就是说这行代码 `System.out.println(null)` 会报错







## 转义字符

Java 支持使用转义字符（Escape Sequences）来表示一些特殊字符。转义字符以反斜杠 `\` 开头，后面跟着一个或多个字符，用来表示一些不能直接输入的字符，或者具有特殊意义的字符。

以下是一些常见的 Java 转义字符及其含义：

1. **换行符**：`\n`
   
   - 用于在输出中插入一个新行。
   ```java
   System.out.println("Hello\nWorld");
   // 输出：
   // Hello
   // World
   ```
   
2. **回车符**：`\r`
   - 用于将光标移动到当前行的开头，但不换行。
   ```java
   System.out.println("Hello\rWorld");
   // 输出：
   // World
   ```

3. **制表符**：`\t`
   - 用于在输出中插入一个水平制表符，通常用于对齐文本。
   ```java
   System.out.println("Name:\tJohn Doe");
   // 输出：
   // Name:    John Doe
   ```

4. **退格符**：`\b`
   - 用于将光标向后移动一个字符位置。
   ```java
   System.out.println("Hello\bWorld");
   // 输出：
   // HellWorld
   ```

5. **双引号**：`\"`
   - 用于在字符串中插入一个双引号。
   ```java
   System.out.println("He said, \"Hello!\"");
   // 输出：
   // He said, "Hello!"
   ```

6. **单引号**：`\'`
   - 用于在字符串中插入一个单引号。
   ```java
   System.out.println("It's a sunny day.");
   // 输出：
   // It's a sunny day.
   ```

7. **反斜杠**：`\\`
   - 用于在字符串中插入一个反斜杠。
   ```java
   System.out.println("C:\\Windows\\System32");
   // 输出：
   // C:\Windows\System32
   ```

8. **Unicode 字符**：`\uXXXX`
   - 用于表示 Unicode 字符，其中 `XXXX` 是四位十六进制数。
   ```java
   System.out.println("\u0041"); // 输出 'A'
   ```

9. **八进制字符**：`\ddd`
   - 用于表示八进制字符，其中 `ddd` 是一到三个八进制数字。
   ```java
   System.out.println("\101"); // 输出 'A' (ASCII 值 65 的八进制表示)
   ```

> 转义字符在字符串字面量中非常有用，特别是在需要处理特殊字符或格式化输出时通过使用这些转义字符，可以更灵活地控制输出的内容和格式







## 变量

当然，以下是一份关于 Java 变量的简洁笔记，你可以直接用作参考：



### 变量声明
- **语法**:
  ```java
  数据类型 变量名;
  ```
- **示例**:
  ```java
  int age; // 声明一个整型变量
  double salary; // 声明一个双精度浮点型变量
  String name; // 声明一个字符串变量
  ```

### 变量初始化
- **单独初始化**:
  ```java
  age = 25; // 给 age 赋值为 25
  salary = 5000.50; // 给 salary 赋值为 5000.50
  name = "John Doe"; // 给 name 赋值为 "John Doe"
  ```
- **声明时初始化**:
  ```java
  int age = 25;
  double salary = 5000.50;
  String name = "John Doe";
  ```



###  变量的作用域
- **局部变量**:
  - 在方法、构造器或块中声明。
  - 作用域仅限于声明的块内。
  - 必须在使用前初始化。
  - **示例**:
    ```java
    public void someMethod() {
        int localVar = 10; // 局部变量
        System.out.println(localVar);
    }
    ```

- **实例变量**:
  - 在类中声明但在方法外声明。
  - 作用域是整个类。
  - 自动初始化为其类型的默认值（例如，`int` 类型的默认值是 0）。
  - **示例**:
    ```java
    public class MyClass {
        int instanceVar; // 实例变量
    
        public void someMethod() {
            System.out.println(instanceVar); // 输出默认值 0
        }
    }
    ```

- **静态变量** (类变量):
  - 使用 `static` 关键字声明。
  - 属于类而不是类的实例。
  - 在类加载时初始化，并且在整个程序运行期间都存在。
  - **示例**:
    ```java
    public class MyClass {
        static int staticVar = 100; // 静态变量
    
        public static void main(String[] args) {
            System.out.println(MyClass.staticVar); // 输出 100
        }
    }
    ```

- **常量**:
  - 使用 `final` 关键字声明。
  - 一旦初始化后就不能再改变其值。
  - 通常使用大写字母表示常量名。
  - **示例**:
    ```java
    public class MyClass {
        final int MAX_AGE = 100; // 常量
    
        public void someMethod() {
            System.out.println(MAX_AGE); // 输出 100
        }
    }
    ```

###  示例代码
```java
public class VariableExample {
    // 实例变量
    int instanceVariable; 
    // 作用域是整个类，每个对象都有自己的实例

    // 静态变量
    static int staticVariable = 100; 
    // 作用域是整个类，所有对象共享同一个静态变量

    public static void main(String[] args) {
        // 局部变量
        int localVariable = 10; 
        // 作用域仅限于 main 方法内部

        // 创建对象
        VariableExample example = new VariableExample();
        example.instanceVariable = 20; 
        // 为实例变量赋值

        // 输出局部变量
        System.out.println("Local Variable: " + localVariable); 
        // 输出局部变量 localVariable 的值

        // 输出实例变量
        System.out.println("Instance Variable: " + example.instanceVariable); 
        // 输出实例变量 instanceVariable 的值

        // 输出静态变量
        System.out.println("Static Variable: " + staticVariable); 
        // 输出静态变量 staticVariable 的值
    }
}
```





## 数据类型

Java中的数据类型分为两大类：基本数据类型（Primitive Data Types）和引用数据类型（Reference Data Types）。下面是这两类数据类型的详细列表。

### 基本数据类型

| 数据类型 | 关键字  | 大小   | 默认值         | 描述                                   |
| -------- | ------- | ------ | -------------- | -------------------------------------- |
| 整型     | byte    | 8位    | 0              | 最小的整数类型，用于存储较小的数值范围 |
|          | short   | 16位   | 0              | 存储较大的整数，比int占用更少的空间    |
|          | int     | 32位   | 0              | 通常使用的整数类型，适合大多数情况     |
|          | long    | 64位   | 0L             | 需要非常大的数字时使用，后缀L或l       |
| 浮点型   | float   | 32位   | 0.0f           | 单精度浮点数，后缀F或f                 |
|          | double  | 64位   | 0.0d           | 双精度浮点数，是默认的浮点数类型       |
| 字符型   | char    | 16位   | '\u0000' (即0) | 单个Unicode字符，用单引号括起来        |
| 布尔型   | boolean | 不固定 | false          | 表示真或假，只有true或false两个值      |

### 引用数据类型

| 类型              | 描述                                             |
| ----------------- | ------------------------------------------------ |
| 类 (Class)        | 用户自定义的数据结构，可以包含属性和方法         |
| 接口 (Interface)  | 定义了一组行为规范，类通过实现接口来获得特定功能 |
| 数组 (Array)      | 一种可以存储多个相同类型元素的容器，元素数量固定 |
| 枚举 (Enum)       | 特殊的数据类型，它有固定的几个常量值             |
| 注解 (Annotation) | 提供了元数据信息，可以应用于包、类、方法等       |

引用数据类型没有固定的大小，因为它们的大小取决于对象的实际内容。在Java中，所有引用数据类型都是从`Object`类派生的，这意味着每个引用数据类型都可以调用`Object`类的方法







## 标识符

标识符：就是给类、方法、变量等起的名字

> 大概就是名字

在Java中，标识符（Identifier）是用来命名变量、方法、类、接口、包等的符号。Java对标识符有一些具体的要求和规则，以确保代码的可读性和一致性。以下是Java标识符的主要要求：

1. **字母数字组合**：
   - 标识符可以由字母、数字、下划线（_）和美元符号（$）组成。
   - 标识符的第一个字符不能是数字。

2. **区分大小写**：
   - Java是区分大小写的语言。例如，`myVariable` 和 `myvariable` 是两个不同的标识符。

3. **关键字限制**：
   - 标识符不能是Java的关键字或保留字。例如，`class`、`for`、`if` 等都不能作为标识符。

4. **长度限制**：
   - 标识符的长度没有明确的限制，但过长的标识符可能会影响代码的可读性。

5. **特殊字符**：
   - 除了字母、数字、下划线（_）和美元符号（$），其他特殊字符（如 `@`, `#`, `!` 等）不能用于标识符。

6. **建议的命名约定**：
   - **驼峰命名法**（Camel Case）：对于变量和方法名，首字母小写，后续单词首字母大写。例如，`myVariableName`。
   - **帕斯卡命名法**（Pascal Case）：对于类名和接口名，每个单词的首字母都大写。例如，`MyClassName`。
   - **全大写**：对于常量，通常使用全大写字母，单词之间用下划线分隔。例如，`MAX_VALUE`。

### 示例

```java
// 合法的标识符
int myVariable;
double myVariable2;
String myString;
final int MAX_VALUE;

// 不合法的标识符
int 1variable;          // 以数字开头
int for;                // 关键字
int my-variable;        // 包含非法字符 '-'
int my variable;        // 包含空格
int @myVariable;        // 包含非法字符 '@'

// 命名约定示例
class MyClass {
    private int myVariable;
    private final int MAX_VALUE = 100;

    public void myMethod() {
        // 方法体
    }
}
```

### 总结

遵循这些规则和建议可以帮助你编写更清晰、更易维护的Java代码。如果你有任何其他问题或需要进一步的解释，请随时提问！





## 键盘输入

在Java中，我们通常使用`Scanner`类，作为获取用户输入的如何使用`Scanner`类来读取不同类型的用户输入。

####  导入 `Scanner` 类

在Java中，处理键盘输入最常用的方式是使用`Scanner`类。`Scanner`类位于`java.util`包中，因此在使用之前需要导入该类。

```java
import java.util.Scanner;
```

#### 创建 `Scanner` 对象

创建一个`Scanner`对象，通常使用`System.in`作为输入源，表示标准输入（即键盘输入）。

```java
Scanner scanner = new Scanner(System.in);
```

#### 读取不同类型的输入

`Scanner`类提供了多种方法来读取不同类型的输入。以下是一些常用的读取方法：

- **读取字符串**：
  - `nextLine()`：读取一行文本，包括空格。
  - `next()`：读取下一个单词，遇到空格停止。

  ```java
  String line = scanner.nextLine();  // 读取一行文本
  String word = scanner.next();      // 读取下一个单词
  ```

- **读取整数**：
  - `nextInt()`：读取一个整数。

  ```java
  int number = scanner.nextInt();
  ```

- **读取浮点数**：
  - `nextDouble()`：读取一个双精度浮点数。
  - `nextFloat()`：读取一个单精度浮点数。

  ```java
  double doubleNumber = scanner.nextDouble();
  float floatNumber = scanner.nextFloat();
  ```

- **读取布尔值**：
  - `nextBoolean()`：读取一个布尔值。

  ```java
  boolean value = scanner.nextBoolean();
  ```

### 示例代码

以下是一个完整的示例，展示了如何使用`Scanner`类从键盘读取不同类型的输入：

```java
import java.util.Scanner;

public class KeyboardInputExample {
    public static void main(String[] args) {
        // 创建Scanner对象
        Scanner scanner = new Scanner(System.in);

        // 读取字符串
        System.out.print("请输入您的名字: ");
        String name = scanner.nextLine();

        // 读取整数
        System.out.print("请输入您的年龄: ");
        int age = scanner.nextInt();

        // 读取浮点数
        System.out.print("请输入您的身高 (米): ");
        double height = scanner.nextDouble();

        // 读取布尔值
        System.out.print("您是否已婚? (true/false): ");
        boolean isMarried = scanner.nextBoolean();

        // 输出结果
        System.out.println("姓名: " + name);
        System.out.println("年龄: " + age);
        System.out.println("身高: " + height + " 米");
        System.out.println("已婚: " + isMarried);

        // 关闭Scanner对象
        scanner.close();
    }
}
```

#### 5. 注意事项

1. **关闭 `Scanner` 对象**：
   - 在完成输入操作后，最好关闭`Scanner`对象以释放资源。可以通过调用`scanner.close()`方法来关闭。

2. **异常处理**：
   - 如果用户输入的格式不正确，`Scanner`类会抛出`InputMismatchException`。你可以使用`try-catch`块来捕获并处理这种异常。

   ```java
   import java.util.InputMismatchException;
   import java.util.Scanner;
   
   public class ExceptionHandlingExample {
       public static void main(String[] args) {
           Scanner scanner = new Scanner(System.in);
   
           try {
               System.out.print("请输入一个整数: ");
               int number = scanner.nextInt();
               System.out.println("您输入的整数是: " + number);
           } catch (InputMismatchException e) {
               System.out.println("输入错误！请输入一个有效的整数。");
           } finally {
               scanner.close();
           }
       }
   }
   ```

3. **读取多行输入**：
   - 如果需要读取多行输入，可以多次调用`nextLine()`方法，或者在一个循环中读取。

   ```java
   import java.util.Scanner;
   
   public class MultiLineInputExample {
       public static void main(String[] args) {
           Scanner scanner = new Scanner(System.in);
   
           System.out.println("请输入多行文本，输入 'end' 结束：");
           StringBuilder input = new StringBuilder();
           while (true) {
               String line = scanner.nextLine();
               if (line.equalsIgnoreCase("end")) {
                   break;
               }
               input.append(line).append("\n");
           }
   
           System.out.println("您输入的文本是：\n" + input.toString());
   
           scanner.close();
       }
   }
   ```

### 总结

- **导入**：使用 `import java.util.Scanner;` 导入 `Scanner` 类。
- **创建对象**：使用 `Scanner scanner = new Scanner(System.in);` 创建 `Scanner` 对象。
- **读取输入**：使用 `nextLine()`, `next()`, `nextInt()`, `nextDouble()`, `nextBoolean()` 等方法读取不同类型的输入。
- **关闭对象**：使用 `scanner.close();` 关闭 `Scanner` 对象。
- **异常处理**：使用 `try-catch` 块处理 `InputMismatchException`。

希望这份笔记对你有帮助！如果你有任何其他问题或需要进一步的解释，请随时提问。
