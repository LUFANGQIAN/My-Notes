## 常问的小题

### 显示模式-块级-行内-行内块

Q：什么是块级元素 行内元素 行内块元素？

A：块级元素、行内元素和行内块元素是HTML和CSS中用于控制页面布局的基础概念，其核心区别在于元素的显示方式和对页面空间的占据规则。

**块级元素**（如`div`、`p`、`h1`-`h6`）默认独占一行，宽度自动填满父容器，可自由设置宽高、外边距和内边距，能够包含行内元素和其他块级元素（需符合嵌套规范）。

**行内元素**（如`span`、`a`、`strong`）不独占一行，多个行内元素在同一行排列，宽度由内容决定，无法直接设置宽高，且只能包含文本或其他行内元素。其垂直方向的外边距对布局影响有限。

**行内块元素**（如`img`、`input`，或通过`display: inline-block`显式定义的元素）结合了块级元素和行内元素的特性：既可与其他元素同行显示，又能设置宽高和边距。但需注意HTML源码中的空白符可能导致元素间产生微小间隙。

三者可通过CSS的`display`属性相互转换（如`block`、`inline`、`inline-block`），实际开发中需根据布局需求选择元素类型，并优先遵循HTML语义化原则。随着Flexbox和Grid等现代布局技术的普及，传统块级/行内布局的限制已大幅弱化，但理解其底层机制仍是掌握CSS的关键基础。



### 网站网页基本单位

Q：网站的基本组成单位是什么？网页的基本组成单位是什么？

A：网站的基本组成单位是**网页**，就像一本书由一页页纸组成。而网页的基本组成单位是**HTML元素**，比如标题（`<h1>`）、段落（`<p>`）、图片（`<img>`）、链接（`<a>`）等，这些元素通过代码定义了网页的结构和内容。简单说，网站靠网页搭建，网页靠HTML元素“搭积木”。



### 按钮相关

Q：按钮怎么写？按钮控件有几个？

A：按钮可以用input的button类型，也可以直接使用button标签，button标签有三种，普通，重置reset，提交submit



### H5新增内容

Q：H5新增标签有哪些？

A：以布局标签举例：
头部header 页脚footer 导航nav 侧标栏aside 独立文章article 文章中的章节section，article通常由多个section组成，section通常都包含标题

脑子里有个内容网站有三部分，header头部，main主要内容，footer页脚，header中有导航栏nav，main中有aside侧边栏悬浮，还有article独立内容，article由多个section组成

![H5语义化结构标签](D:/桌面文件/笔记/My-Notes/img/H5语义化结构标签.png)



Q：新增的type属性(常用的)？

A：通过场景记忆，
首先是数据校验类型：发邮件的Email类型，打电话的tel类型，网址校验的url类型，输入数字可规定步长的number类型
其次是时间类型： 日期date 月month 周week 时间time 日期与时间datetime-local 
最后是单独记忆的几个选择：选择颜色用color 拖动条选择用range 搜索东西用search



Q：H5新增的列表相关的标签

A：details与summary搭配可折叠列表标签，details包含summary，summary内存储标题与内容

​		datalist是联动input:text进行内容提示的，通过name包括



### 简单介绍下状态标签与框架标签

Q：简单介绍下状态标签与框架标签

A：状态标签有两个，一个是meter 一个是progress，meter表示已知范围测量值，progress通常表示工作进度



### 表格

Q：表格合并的标签是什么？都有什么标签？

A：表格的行合并(单元格竖向合并)使用**rowspan**，表单列合并(单元格横向合并)使用**colspan**

​	首先由表格标签开始 **table** 包裹所有表格内的元素，表格标题是**caption**，表头行为 **thead**, 表主体为 **tbody** ,表页脚为 **tfoot** ,所有表格行由 **tr**，表头单元格由 **th** 组成，表主体单元格由 **td** 组成，表页脚单元格也是 **td** 组成



### 表单

Q：表单由什么构成的(表单控件)？

A：经典的账号 密码 性别 爱好 提交 重置，账号用input:text ，密码用input:password ,性别用单选框 input:radio，

​	爱好用多选框 input:checkbox , 提交 重置有两种写法 

​		第一种用input搭配类型使用input:submit input:reset

​		第二种直接使用button标签



Q：场景题，男女单选，怎么写？

A：name设置成一样的，分成一组



Q：复选框 单选框 默认选中怎么处理？文本框禁用怎么处理？必填项？占位提示？

A：默认选中在标签中使用checked，文本框禁用使用disabled，必填项用required，占位用placeholder

```txt
title悬浮提示

placeholder占位提示

required必填项

autofocus自动获取焦点

autocomplete=“on/off”自动填充历史输入记录(前提是浏览器保存并填写地址功能开启)

pattern正则表达式
```



### 列表

Q：列表有哪些类型？

A：**有序**列表**ol li**      **无序**列表**ul li**      **定义**列表**dl dt子级标题 dd子级内容**







### 多媒体资源引入

Q：如何引入图片，音频 ，视频？

A：img标签，audio标签，video标签

