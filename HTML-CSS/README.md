# HTML/CSS 面试题

本目录包含 HTML 和 CSS 相关的常见面试题，涵盖布局、动画、响应式设计、性能优化等方面。

## 目录
- [基础概念](#基础概念)
- [布局相关](#布局相关)
- [动画与效果](#动画与效果)
- [性能优化](#性能优化)
- [实战项目](#实战项目)

---

## 🌟 精选题目

### 1. HTML5 新特性有哪些？
**考察点**: HTML5 语义化、新API、多媒体支持
**难度**: ⭐⭐
**公司标签**: 腾讯、阿里、字节

#### 参考答案
1. **语义化标签**：`<header>`, `<nav>`, `<section>`, `<article>`, `<footer>`
2. **多媒体支持**：`<audio>`, `<video>`
3. **图形绘制**：`<canvas>`, SVG
4. **新API**：地理定位、本地存储、Web Workers、WebSocket
5. **表单增强**：新的 input 类型、placeholder、autocomplete

---

### 2. CSS 盒模型是什么？标准盒模型与 IE 盒模型有什么区别？
**考察点**: 盒模型原理、样式计算
**难度**: ⭐⭐
**公司标签**: 百度、美团

#### 参考答案
CSS 盒模型由四部分组成：**内容(content) → 内边距(padding) → 边框(border) → 外边距(margin)**

**区别**：
- **标准盒模型**：`width = content + padding + border`
- **IE盒模型**：`width = content`

使用 `box-sizing` 属性控制：
```css
/* 标准盒模型 */
box-sizing: content-box; /* 默认值 */

/* IE盒模型 */
box-sizing: border-box;
```

---

### 3. 如何实现垂直居中？至少列出3种方式
**考察点**: CSS 布局能力、多种实现方案
**难度**: ⭐⭐⭐
**公司标签**: 字节、快手

#### 参考答案
**方法1：Flexbox**
```css
.parent {
  display: flex;
  align-items: center;
  justify-content: center;
}
```

**方法2：Grid**
```css
.parent {
  display: grid;
  place-items: center;
}
```

**方法3：Transform**
```css
.child {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

**方法4：Table-cell**
```css
.parent {
  display: table-cell;
  vertical-align: middle;
  text-align: center;
}
```

---

### 4. 什么是 BFC？如何触发？有什么应用场景？
**考察点**: 布局理解、渲染机制
**难度**: ⭐⭐⭐⭐
**公司标签**: 腾讯、华为

#### 参考答案
**BFC (Block Formatting Context)** 是块级格式化上下文，独立的渲染区域。

**触发方式**：
```css
body {
  position: absolute | relative | fixed | sticky;
  display: inline-block | flex | grid | table | table-cell;
  float: left | right;
  overflow: hidden | scroll | auto;
}
```

**应用场景**：
- **清除浮动**：BFC 元素会自动包含子元素
- **防止外边距重叠**：BFC 内外元素外边距不合并
- **自适应两栏布局**：利用 BFC 元素避开浮动元素

---

### 5. CSS 选择器的优先级规则是什么？
**考察点**: CSS 优先级计算、样式冲突解决
**难度**: ⭐⭐
**公司标签**: 阿里、拼多多

#### 参考答案
优先级从高到低：
1. **!important**
2. **内联样式**（style=""）
3. **ID选择器**（#id）
4. **类选择器**（.class）、属性选择器（[type="text"]）、伪类（:hover）
5. **元素选择器**（div）、伪元素（::before）

**计算规则**：
- ID选择器：100
- 类选择器：10
- 元素选择器：1
- 内联样式：1000

例：`.nav a.active` = 10 + 1 + 10 = 21

---

### 6. 如何实现圣杯布局/双飞翼布局？
**考察点**: CSS 布局实现、语义化
**难度**: ⭐⭐⭐⭐
**公司标签**: 字节、百度

#### 圣杯布局实现
```html
<div class="container">
  <div class="main">主内容</div>
  <div class="left">左侧栏</div>
  <div class="right">右侧栏</div>
</div>
```

```css
.container {
  padding: 0 200px; /* 左右固定宽度 */
}
.main {
  float: left;
  width: 100%;
  background: #f5f5f5;
}
.left {
  float: left;
  width: 200px;
  margin-left: -100%;
  position: relative;
  left: -200px;
}
.right {
  float: left;
  width: 200px;
  margin-left: -200px;
  position: relative;
  left: 200px;
}
```

---

### 7. 什么是响应式设计？如何实现？
**考察点**: 移动端适配、设备兼容性
**难度**: ⭐⭐⭐
**公司标签**: 大疆、小红书

#### 参考答案
**响应式设计**指网页能根据不同设备（手机、平板、桌面）自动调整布局。

**实现方式**：
1. **媒体查询**
   ```css
   @media (max-width: 768px) {
     .container { width: 100%; }
   }
   ```

2. **相对单位**
   ```css
   .container {
     width: 100%;
     font-size: 1rem;
     padding: 5%;
   }
   ```

3. **Flexbox/Grid**
   ```css
   .container {
     display: grid;
     grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
   }
   ```

4. **viewport**
   ```html
   <meta name="viewport" content="width=device-width, initial-scale=1">
   ```

---

### 8. CSS 中 position 属性有哪些值？有什么区别？
**考察点**: 定位机制、布局理解
**难度**: ⭐⭐
**公司标签**: 腾讯、网易

#### 参考答案
**static**：默认值，正常文档流
**relative**：相对自身正常位置定位
**absolute**：相对最近的非static祖先元素定位
**fixed**：相对浏览器窗口定位，不随滚动移动
**sticky**：在阈值内为relative，超过为fixed

---

### 9. 请解释 CSS 层叠上下文和 z-index 生效条件
**考察点**: 布局深度、渲染层级
**难度**: ⭐⭐⭐⭐
**公司标签**: 阿里、腾讯

#### 参考答案
**层叠上下文**是元素的层级分组，同一层叠上下文内的元素按 z-index 排序。

**z-index 生效条件**：
1. 元素必须是**定位元素**（position非static）
2. 元素位于同一层叠上下文中
3. z-index值影响同一层叠上下文内的层级关系

---

### 10. CSS 性能优化有哪些方法？
**考察点**: 性能意识、优化技巧
**难度**: ⭐⭐⭐⭐
**公司标签**: 快手、字节

#### 参考答案
1. **选择器优化**：避免嵌套过深，避免通配符
2. **减少重绘回流**：使用 transform 和 opacity
3. **使用CSS硬件加速**
4. **压缩CSS文件**
5. **减少@import**
6. **使用CSS Sprite**
7. **预加载关键CSS**

---

## 📚 扩展学习资源

1. MDN Web Docs
2. CSS-Tricks
3. Codepen 实战案例
4. 各大厂技术博客

---

> 持续更新中... 欢迎贡献更多高质量题目！