# CSS 选择器优先级

## 题目
**CSS 选择器优先级如何计算？**

---

## 💡参考答案

### 1. 选择器优先级规则
CSS 选择器优先级从**高到低**的顺序为：

1. **`!important`** - 强制最高优先级
2. **内联样式** - 在元素标签内的 `style=""` 属性
3. **ID 选择器** - `#id`
4. **类选择器** - `.class`
5. **属性选择器** - `[type="text"]`
6. **伪类选择器** - `:hover`、`:nth-child`
7. **元素选择器** - `div`、`span`、`p`
8. **伪元素选择器** - `::before`、`::after`
9. **通配符选择器** - `*`
10. **继承的样式** - 最低优先级

---

### 2. 权重计算系统
CSS 使用**权重值累计**的方式来比较优先级：

| 选择器类型 | 权重值（a,b,c,d） | 说明 |
|------------|------------------|------|
| !important | 最高，忽略权重计算 | 应谨慎使用 |
| 内联样式 | (1,0,0,0) | 在元素标签内部 |
| ID 选择器 | (0,1,0,0) | 累计：每多一个 ID 加 (0,1,0,0) |
| 类/属性/伪类 | (0,0,1,0) | 累计：每多一个加 (0,0,1,0) |
| 元素/伪元素 | (0,0,0,1) | 累计：每多一个加 (0,0,0,1) |
| 通配符/关系符 | (0,0,0,0) | 不影响优先级计算 |

---

### 3. 计算示例

```css
/* 示例1：比较计算 */
#nav .menu.item:hover    /* (0,1,3,0) → 130 */
div#main .content span   /* (0,1,1,2) → 112 */
body .logo               /* (0,0,1,1) → 011 */

/* 比较：130 > 112 > 011 */
/* 最终优先级：第1行 > 第2行 > 第3行 */
```

**详细计算过程：**
```css
/* 权重：(0,1,2,2) = ID:1 类:2 元素:2 */
#header .nav li.active a::before {
  color: red; /* 优先级：(0,1,2,2) → 122 */
}

/* 权重：(0,1,1,3) = ID:1 类:1 元素:3 */
#sidebar .list li:hover {
  color: blue; /* 优先级：(0,1,1,3) → 113 */
}

/* 122 > 113，所以显示红色 */
```

---

### 4. !important 的特殊处理

```css
/* !important 打破所有优先级规则 */
.text {
  color: black !important; /* 最高优先级 */
}

#special.text {
  color: red; /* 即使优先级更高，也不会覆盖 !important */
}
```

**最佳实践：**
- 尽量避免使用 `!important`
- 如果必须使用，添加注释说明原因
- 考虑使用 CSS-in-JS 或 CSS Modules 避免冲突

---

### 5. 优先级相等时的处理
当两个选择器优先级完全相同时：

1. **后定义覆盖先定义**（层叠性）
2. **来源顺序**：用户代理 < 用户 < 作者 < !important
3. **文件位置**：导入文件 < 主文件

```css
/* 文件1.css */
.button {
  color: blue; /* 先定义，相同优先级被覆盖 */
}

/* 文件2.css */
.button {
  color: red;  /* 相同优先级，后定义生效 */
}
```

---

## 🎯 考察点分析

### 考察方向
1. **优先级规则记忆**：选择器类型的优先级顺序
2. **权重计算能力**：实际场景中的优先级比较
3. **问题解决能力**：如何优雅地提高优先级而不滥用 `!important`
4. **代码质量意识**：编写可维护的 CSS

### 常考追问
1. **"为什么不要使用 !important？"**
   - **答案**：破坏层叠性，导致维护困难，难以覆盖，违反 CSS 设计原则

2. **"如何计算 `#a .b .c div:hover` 的优先级？"**
   - **答案**：(0,1,3,1) = 1个ID + 3个类/伪类 + 1个元素

3. **"`[data-test]` 和 `.class` 优先级相同吗？"**
   - **答案**：相同，都是 (0,0,1,0) 权重

---

## 📝 实际应用场景

### 场景1：组件库样式覆盖
```css
/* 组件库基础样式 */
.ui-button {
  background: #007bff;
  color: white;
}

/* 业务代码覆盖组件库样式 */
.card .ui-button {
  background: #28a745; /* 优先级：(0,0,2,0) > (0,0,1,0) */
}
```

### 场景2：主题定制系统
```css
/* 基础主题 */
.btn-primary {
  background: blue;
}

/* 深色主题覆盖 */
html[data-theme="dark"] .btn-primary {
  background: darkblue; /* 巧妙提高优先级 */
  /* (0,0,2,1) > (0,0,1,0) */
}
```

### 场景3：用户自定义样式支持
```css
/* 提供易覆盖的接口 */
.widget {
  --widget-bg: #ffffff; /* CSS变量实现可配置性 */
  background: var(--widget-bg);
}

/* 用户可以在自己的样式表中覆盖 */
.widget {
  --widget-bg: #f0f0f0;
}
```

---

## ⚠️ 常见错误

### 错误1：错误计算优先级
```css
/* 错误理解 */
#id div { /* 误认为比下面的高 */
  color: red;
}

div#id { /* 实际优先级相同，后定义生效 */
  color: blue;
}
/* 最终显示蓝色 */
```

### 错误2：过度嵌套
```css
/* 不好：过度嵌套导致优先级过高 */
body #main .container .row .col .content .text span {
  font-size: 14px; /* 过度嵌套，很难覆盖 */
}

/* 更好：保持低优先级 */
.text-emphasis {
  font-size: 14px; /* 更易于维护和覆盖 */
}
```

### 错误3：!important 滥用
```css
/* 糟糕的代码 */
.link { color: blue; }
.link:hover { color: darkblue; }
.active.link { color: green !important; /* 滥用 !important */ }

/* 更好的方案 */
.link.active {
  color: green;
  /* 通过提高自然优先级来覆盖 */
}
```

---

## 🔧 调试技巧

### 1. Chrome DevTools 查看
在 Chrome 开发者工具中：
1. 右键元素 → 检查
2. 查看 Styles 面板
3. 查看带删除线的样式（被覆盖的）
4. 查看计算后的优先级标识

### 2. 优先级计算工具
```javascript
// 简单优先级计算函数
function getSelectorPriority(selector) {
  const priority = [0, 0, 0, 0]; // [ID, Class, Element]
  // ID 选择器
  priority[0] = (selector.match(/#/g) || []).length;
  // 类/属性/伪类选择器
  priority[1] = (selector.match(/\.|\[|:/g) || []).length;
  // 元素选择器
  priority[2] = (selector.match(/^[a-z]|(?<= |\+|~|>)[a-z]/gi) || []).length;
  
  return priority;
}
```

### 3. 优先级冲突检测
```css
/* 添加调试信息 */
.debug-priority::before {
  content: "优先级：" attr(data-priority);
  color: #999;
  font-size: 12px;
}
```

---

## 🏢 大厂面试特点

### 腾讯面试侧重
- **原理深度**：浏览器渲染引擎如何解析选择器优先级
- **性能影响**：选择器复杂度对渲染性能的影响
- **工程实践**：大规模项目中如何管理样式优先级

### 阿里字节面试侧重
- **现代方案**：CSS-in-JS（styled-components）如何解决优先级问题
- **系统设计**：设计一个不易产生冲突的 CSS 架构
- **算法思想**：实现一个 CSS 优先级计算器

### 快手美团面试侧重
- **实际案例**：解决真实业务中的样式冲突问题
- **代码规范**：团队中如何约定优先级的编写规范
- **工具链**：如何用工具检测和避免优先级问题

---

## ✨ 进阶扩展

### 1. 伪类与伪元素的优先级差异
```css
/* 伪类：:hover、:nth-child(n)、:first-child */
a:hover { /* 权重：(0,0,1,1) 元素:1 伪类:1 */
  color: red;
}

/* 伪元素：::before、::after、::first-line */
a::before { /* 权重：(0,0,0,2) 元素:1 伪元素:1 */
  content: "★";
}
```

### 2. CSS 变量与优先级
```css
:root {
  --primary-color: #007bff;
}

.component {
  --primary-color: #28a745; /* 组件内覆盖 */
  color: var(--primary-color);
}

.component.special {
  --primary-color: #dc3545 !important; /* !important 对 CSS 变量也有效 */
}
```

### 3. Shadow DOM 中的优先级
```css
/* Shadow DOM 中的样式作用域 */
:host {
  color: black; /* 主机元素 */
}

:host(.active) {
  color: blue; /* 条件主机 */
}

::slotted(span) {
  color: red; /* 插槽内容 */
}
```

---

**本题来源**：掘金用户"阿芯爱编程"《2026前端面试题及答案》
**难度评级**：⭐⭐⭐（常见高频题）
**公司标签**：阿里、腾讯、字节、美团、京东