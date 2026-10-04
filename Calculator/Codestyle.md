# 前端代码规范（Front-end Code Style）

本项目的代码规范来源如下（Sources）：

1. **Google JavaScript Style Guide**
   https://google.github.io/styleguide/jsguide.html

2. **MDN Web Docs - Writing style guide**
   https://developer.mozilla.org/en-US/docs/MDN/Writing_guidelines/Writing_style_guide

3. **Airbnb JavaScript Style Guide**（辅助参考）
   https://github.com/airbnb/javascript

以下规范是在上述官方标准基础上，结合本项目实际情况提炼出来的具体约定。


## 1. 缩进与空格（Indentation & Whitespace）

- 统一使用 **4 个空格** 缩进，禁止使用 Tab。
- 一行代码不超过 **100 个字符**。
- 运算符两侧加空格：`a + b`、`x === y`。
- 函数参数逗号后加一个空格：`foo(a, b, c)`。
- 对象字面量冒号后加空格：`{ key: value }`。
- 函数声明与方法之间空一行。


## 2. 命名规范（Naming Conventions）

- **变量、函数**：`lowerCamelCase`
  - 示例：`loadHistory`、`sendToBackend`、`resultLine`
- **常量**：`UPPER_SNAKE_CASE`
  - 示例：`API_BASE`、`MAX_LENGTH`
- **DOM 元素引用**：`lowerCamelCase`，与 id 保持一致
  - 示例：`const historyList = document.getElementById('historyList');`
- **布尔值**：以 `is` / `has` / `can` / `should` 开头
  - 示例：`isLoading`、`hasError`
- **CSS 类名**：kebab-case
  - 示例：`history-panel`、`result-line`、`btn-clear`


## 3. 字符串（Strings）

- 优先使用**单引号**：
  ```javascript
  const API_BASE = 'http://localhost:8080';
  ```
- 需要插值时使用**模板字符串**（反引号）：
  ```javascript
  const url = `${API_BASE}/api/calculate`;
  ```
- 避免拼接字符串：`'a' + b + 'c'` ✗ → `` `a${b}c` `` ✅

## 4. 变量声明（Variable Declarations）

- 优先使用 `const`，需要重新赋值时用 `let`。
- **禁止使用 `var`**。
- 声明后必须赋值，避免声明未使用变量。
- 变量声明尽量靠近使用位置。

## 5. 函数（Functions）

- 使用 **具名函数** 或 **箭头函数**，避免过深嵌套。
- 单个函数不超过 **60 行**。
- 每个函数只做一件事（单一职责）。
- 异步操作统一使用 `async / await`，不写 `.then()` 链：
  ```javascript
  async function loadHistory() {
      const resp = await fetch(`${API_BASE}/api/history`);
      const data = await resp.json();
      renderHistory(data);
  }
  ```
- 请求失败必须用 `try / catch` 包裹并给出用户提示。
---
## 6. 注释（Comments）

- 复杂逻辑（如历史记录渲染、错误处理）必须写清注释。
- 文件开头用块注释说明本文件用途。
- 行内注释解释"为什么"，而不是"是什么"。
- **禁止保留注释掉的废弃代码**。
- 中文注释与英文注释混用时，保持同一段落语言一致。

示例：
```javascript
// 把表达式发送到后端计算；成功后刷新历史列表
async function sendToBackend() {
    // ...
}
```
## 7. HTML 规范

- 标签名、属性名一律 **小写**。
- 属性值使用**双引号**：
  ```html
  <input type="button" class="button" value="=" onclick="sendToBackend()">
  ```
- 自闭合标签（如 `<meta>`、`<input>`）不加 `/`。
- 缩进与 JS 保持一致，同为 **4 空格**。
- id 用于 JS 引用，class 用于样式，不混用。

## 8. CSS 规范

- 每条样式声明末尾必须加分号 `;`。
- 属性名全小写，kebab-case。
- 选择器按"块→元素→修饰"顺序书写。
- 数字为 0 时省略单位：`margin: 0;`（不写 `0px`）。
- 颜色使用十六进制小写：`#1e293b`。
- 同类样式集中书写，避免重复。

## 9. 与后端通信规范
- 所有 API 地址统一由顶部 **`API_BASE` 常量**管理，不散落各处。
- 与后端的交互必须处理三类情况：
  1. HTTP 请求成功且业务成功 → 显示结果
  2. HTTP 请求成功但业务失败（`success: false`）→ 显示 `message`
  3. HTTP 请求失败（网络错误 / 后端未启动）→ 显示"无法连接后端"
- 不把计算逻辑放到前端（保证前后端分离）。


## 10. 安全（Security）
- **禁止使用 `eval`** 或任何形式的动态代码执行。
- 渲染动态内容（如历史表达式）必须经过 `escapeHtml()` 转义，防止 XSS。
- 不通过 URL 参数拼接敏感数据。
---

## 11. 格式一致性检查
提交前自查：
- [ ] 所有缩进是 4 空格（不是 Tab）
- [ ] 字符串使用单引号
- [ ] 变量名 lowerCamelCase
- [ ] 常量 UPPER_SNAKE_CASE
- [ ] 无 `var`
- [ ] 无 `eval`
- [ ] 无废弃代码块
- [ ] 所有函数带必要注释