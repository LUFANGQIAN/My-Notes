### 总结笔记：JavaScript操作CSS样式

---

#### **10.1 使用 `style` 访问样式**

- **规则**：
  - 每个HTML元素都有一个 `style` 属性，返回 `CSSStyleDeclaration` 对象。
  - 样式属性写法与CSS不同，例如：`font-size` → `fontSize`。

- **常用属性表**：

| 类别 | 属性            | 描述         |
| ---- | --------------- | ------------ |
| 背景 | backgroundColor | 设置背景颜色 |
|      | backgroundImage | 设置背景图片 |
| 文本 | fontSize        | 设置字体大小 |
|      | color           | 设置文本颜色 |
| 边框 | border          | 设置边框属性 |

- **示例**：
  ```javascript
  const div = document.getElementById("myDiv");
  div.style.backgroundColor = "yellow"; // 将背景色设置为黄色
  div.style.fontSize = "20px";          // 将字体大小设置为20像素
  ```
  **解释**：通过 `style` 属性直接修改元素的样式，适合简单的样式调整。上述代码将一个 `div` 的背景色改为黄色，并将字体大小设置为 20 像素。

---

#### CSSStyleDeclaration属性方法

##### 概述
`CSSStyleDeclaration` 对象是通过 HTML 元素的 `style` 属性返回的对象，它包含了一系列用于操作元素样式的属性和方法。这些属性和方法允许我们在 JavaScript 中动态地修改 HTML 元素的样式。

##### 常用属性和方法

| 属性或方法                 | 说明                                                         |
| -------------------------- | ------------------------------------------------------------ |
| `cssText`                  | 访问或设置整个 CSS 样式代码。例如：`divEle.style.cssText = "width: 170px; height: 100px; background: red; font-size: 20px;"` |
| `length`                   | 获取当前样式属性的数量。例如：`divEle.style.length` 返回样式属性的数量 |
| `getPropertyValue(name)`   | 返回指定属性名对应的值。例如：`var heightVal = divEle.style.getPropertyValue("height")` |
| `item(index)`              | 返回指定位置的 CSS 属性名称。例如：`document.write(divEle.style.item(i) + "<br>")` |
| `removeProperty(name)`     | 删除指定的样式属性。例如：`divEle.style.removeProperty("font-size")` |
| `setProperty(name, value)` | 设置指定属性的值。例如：`divEle.style.setProperty("background", "yellow")` |

##### 示例代码及解释

```javascript
<div style="font-size: 30px;" id="div1"></div>
<script>
    var divEle = document.getElementById("div1");
    
    // 遍历并输出所有样式属性名称
    for (var i = 0; i < divEle.style.length; i++) {
        document.write(divEle.style.item(i) + "<br>");
    }
    
    // 获取特定属性值
    var heightVal = divEle.style.getPropertyValue("height");
    alert(heightVal);
    
    // 删除特定属性
    divEle.style.removeProperty("font-size");
    
    // 设置新属性
    divEle.style.setProperty("background", "yellow");
</script>
```

**解释**：

- **遍历样式属性**：使用 `for` 循环和 `item` 方法遍历并输出 `div` 元素的所有样式属性名称。
- **获取属性值**：使用 `getPropertyValue` 方法获取 `height` 属性的值，并通过 `alert` 显示出来。
- **删除属性**：使用 `removeProperty` 方法删除 `font-size` 属性。
- **设置新属性**：使用 `setProperty` 方法将背景色设置为黄色。



---

#### **10.3 使用 `className` 修改样式**

- **适用场景**：当需要切换多个样式时，推荐使用 `className`。

- **示例**：
  ```html
  <style>
    .highlight { background-color: yellow; color: red; } /* 高亮样式 */
    .normal { background-color: white; color: black; }   /* 默认样式 */
  </style>
  <div id="text" class="normal">Hello World</div>
  <button onclick="toggleHighlight()">切换样式</button>
  <script>
    function toggleHighlight() {
      const text = document.getElementById("text");
      text.className = text.className === "normal" ? "highlight" : "normal";
    }
  </script>
  ```
  **解释**：通过切换 `className`，可以快速应用一组预定义的样式。上述代码中，点击按钮会在 `.normal` 和 `.highlight` 两个样式之间切换，分别表示默认样式和高亮样式。

---

#### **10.4 操作内部或外部CSS样式**

- **获取样式表**：
  - 外部样式表：`document.styleSheets`
  - 内联样式表：`document.getElementsByTagName('style')[0]`

- **常用方法**：
  - `insertRule(rule, index)`：插入新规则。
  - `deleteRule(index)`：删除指定规则。

- **示例**：
  ```javascript
  const sheet = document.styleSheets[0]; // 获取第一个样式表
  sheet.insertRule("body { background-color: lightblue; }", 0); // 在索引 0 插入新规则
  sheet.deleteRule(0); // 删除索引 0 的规则
  ```
  **解释**：通过操作 `CSSStyleSheet` 对象，可以直接修改整个页面的样式规则。上述代码先在样式表中插入一条规则（将背景色设为浅蓝色），然后删除该规则。

---

#### **10.5 获取计算样式**

- **方法**：
  使用 `window.getComputedStyle(element)` 获取最终应用的样式。
  ```javascript
  const div = document.getElementById("myDiv");
  const computedStyle = window.getComputedStyle(div);
  console.log(computedStyle.width); // 输出实际宽度
  ```
  **解释**：`getComputedStyle` 返回的是元素最终渲染后的样式值，包括浏览器默认样式和继承样式。上述代码获取了 `div` 的实际宽度并打印到控制台。

---

#### **学习总结**

- **核心知识点**：
  1. 使用 `style` 直接操作单个样式：适合简单修改。
  2. 使用 `className` 快速切换多个样式：适合复杂样式管理。
  3. 操作样式表（`CSSStyleSheet`）实现动态规则管理：适合全局样式调整。
  4. 使用 `getComputedStyle` 获取最终渲染样式：适合读取计算后的样式值。

- **应用场景**：
  - 动态修改样式（如按钮点击效果、动画）。
  - 实现复杂的交互效果（如菜单切换、动态主题）。

---

以上为简洁版笔记，每个示例都附有详细解释，方便理解和记忆！
