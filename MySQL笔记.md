# MySQL笔记



## 增删查改（CRUD）

MySQL是一种广泛使用的开源关系型数据库管理系统，支持SQL（Structured Query Language）作为与数据库交互的语言。以下是关于MySQL的基本操作和概念的简要总结，包括增删查改（CRUD）操作的具体实现。

#### 1. 插入数据 (Create)
使用`INSERT INTO`语句向数据库表中添加新记录。可以指定列名或省略列名（如果插入所有列的数据）。

```sql
-- 向表中特定列插入数据
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);

-- 插入所有列的数据
INSERT INTO table_name
VALUES (value1, value2, value3, ...);
```

**示例**：向`students`表中插入一条学生信息。
```sql
INSERT INTO students (id, name, age, grade)
VALUES (1, '张三', 18, 'A');
```

#### 2. 查询数据 (Retrieve)
通过`SELECT`语句从数据库中检索数据。可以查询特定列的数据或使用条件筛选需要的数据。

```sql
-- 查询表中的所有列
SELECT * FROM table_name;

-- 查询特定列的数据
SELECT column1, column2 FROM table_name;

-- 使用条件查询
SELECT * FROM table_name WHERE condition;
```

**示例**：查询年龄为18岁的学生信息。
```sql
SELECT * FROM students WHERE age = 18;
```

#### 3. 更新数据 (Update)
利用`UPDATE`语句修改表中的现有记录。务必小心使用`WHERE`子句以确保只更新满足条件的记录。

```sql
UPDATE table_name 
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

**示例**：将学号为1的学生的成绩等级修改为'B'。
```sql
UPDATE students
SET grade = 'B'
WHERE id = 1;
```

#### 4. 删除数据 (Delete)
使用`DELETE`语句从表中移除记录。同样地，使用`WHERE`子句来限制删除哪些记录非常重要。

```sql
DELETE FROM table_name WHERE condition;
```

**示例**：删除学号为1的学生记录。
```sql
DELETE FROM students WHERE id = 1;
```





## `ORDER BY` 排序

- **作用**：对查询结果按指定列排序，提升数据可读性与分析效率。  
- **语法**：  
  ```sql
  SELECT 列名 FROM 表名 ORDER BY 列1 [ASC/DESC], 列2 [ASC/DESC];
  ```
- **默认行为**：  
  - 未指定排序方式时，默认为升序（`ASC`）。  
- **多列排序**：  
  - 可按多列排序，优先按左列排序，左列值相同时再按右列排序。  
- **升序/降序**：  
  - `ASC`：升序（默认）。  
  - `DESC`：降序。  
- **适用场景**：  
  - 按数值、日期、字母等排序结果（如成绩排名、时间线展示）。  

**示例**：  
```sql
-- 按分数降序，年龄升序排序  
SELECT * FROM students ORDER BY score DESC, age ASC;  
```