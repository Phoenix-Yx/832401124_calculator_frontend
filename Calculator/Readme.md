# Calculator 前端
前后端分离计算器系统——前端部分
## 1. 项目介绍（Project Introduction）
本项目是"前后端分离计算器系统"的**前端**，与 Spring Boot 后端配合使用。
前端负责：
- 提供计算器界面（数字、运算符、括号、小数点、清空、退格）
- 收集用户输入的表达式
- 通过 HTTP 请求把表达式发送给后端
- 显示后端返回的计算结果
- 从后端数据库读取并展示计算历史
- 删除单条历史 / 清空全部历史
- 显示后端返回的错误信息（如除零、非法表达式）
前端不参与任何核心计算，所有计算由后端完成。

## 2. 技术栈（Technology Stack）
- HTML5
- CSS3（Flexbox 布局、深色主题、毛玻璃效果）
- 原生 JavaScript（ES6+）
- Fetch API + async/await 与后端通信

## 3. 运行环境（Runtime Environment）

- 现代浏览器：Chrome / Edge / Firefox
- VSCode
- VSCode 插件：**Live Server**
- 后端需运行在 `http://localhost:8080`


## 4. 安装方法（Installation Method）
### 4.1 安装 VSCode
官网下载：https://code.visualstudio.com/

### 4.2 安装 Live Server 插件
1. 打开 VSCode
2. 点左侧扩展图标（四个方块）
3. 搜索 `Live Server`
4. 点 **Install**

### 4.3 获取本项目

```bash
git clone https://github.com/你的用户名/832401124_calculator_frontend.git
```

或用 VSCode 直接 **File → Open Folder** 打开本项目文件夹。

## 5. 启动方法（Startup Method）

1. 用 VSCode 打开本项目文件夹
2. 在左侧文件树中找到 `src/calculator.html`
3. **右键** → 选择 **Open with Live Server**
4. 浏览器会自动打开：
   ```
   http://127.0.0.1:5501/src/calculator.html
   ```

### 关于端口 5501

本项目通过 `.vscode/settings.json` 把 Live Server 的端口固定为 **5501**：

```json
{
  "liveServer.settings.port": 5501
}
```

这样前端地址固定为 `http://127.0.0.1:5501`，与后端 `8080` 分开，符合前后端分离架构。

---

## 6. 配置说明（Configuration Instructions）

前端的**唯一配置项**是后端 API 地址，位于 `src/calculator.html` 的 `<script>` 顶部：

```javascript
const API_BASE = 'http://localhost:8080';
```

- 如果你的后端端口不是 8080，请改这里。
- 如果是部署到服务器，改成对应的域名，例如 `https://api.example.com`。
- **注意**：必须带 `http://` 或 `https://` 前缀，末尾不要加 `/`。

## 7. 数据库初始化方法（Database Initialization）

前端**不涉及数据库**。
数据库由后端负责初始化（见后端 README）。前端启动后第一次调用 `/api/history` 时，会自动从后端拉取历史列表。

## 8. 前后端连接方式（Front-end / Back-end Connection Method）

前端通过 **HTTP + JSON** 与后端通信，调用以下接口：
| 功能 | 方法 | 地址 |
|------|------|------|
| 计算表达式 | POST | `http://localhost:8080/api/calculate` |
| 查询历史 | GET | `http://localhost:8080/api/history` |
| 删除指定历史 | DELETE | `http://localhost:8080/api/history/{id}` |
| 清空全部历史 | DELETE | `http://localhost:8080/api/history` |

### 请求体示例（计算）

```json
{ "expression": "(1+2)*3" }
```

### 响应示例（成功）

```json
{
  "success": true,
  "id": 1,
  "expression": "(1+2)*3",
  "result": 9.0,
  "createdAt": "2026-10-01T10:20:00"
}
```

### 响应示例（失败）

```json
{
  "success": false,
  "message": "除数不能为零"
}
```

### 跨域说明

后端已配置 `@CrossOrigin(origins = "*")`，允许前端从 5501 端口跨域访问 8080，无需额外配置。

## 9. 其他必要信息（Other Information）

### 9.1 目录结构

```
832401124_calculator_前端/
├── src/
│   └── calculator.html     前端页面（HTML + 内联 CSS + 内联 JS）
├── .vscode/
│   └── settings.json       Live Server 端口配置（5501）
├── README.md               本文件
└── codestyle.md            前端代码规范
```

### 9.2 功能演示要点

- 输入 `12+8` → 点 `=` → 显示 `= 20.0`
- 输入 `(1+2)*3` → 点 `=` → 显示 `= 9.0`
- 输入 `1/0` → 点 `=` → 显示红色错误 `⚠ 除数不能为零`
- 计算成功后，右侧历史列表自动刷新
- 刷新浏览器（F5），历史记录仍存在（来自后端数据库）
- 点击历史记录右侧 `✕` → 该条被删除
- 点击右上角"清空" → 全部历史被清空

### 9.3 前后端分离验证

**停掉后端服务后**：
- 前端页面仍可正常打开
- 按钮仍可点击、可输入数字
- 但点击 `=` 会提示"无法连接后端"，无法得到新的计算结果

这证明计算逻辑在后端，符合前后端分离要求。

### 9.4 常见问题

**Q：点击 `=` 提示"无法连接后端"？**
A：后端未启动。请先启动后端 Spring Boot 项目（默认 8080 端口）。

**Q：浏览器控制台报 CORS 错误？**
A：确认后端 `CalculatorController` 上有 `@CrossOrigin(origins = "*")`，并重启后端。

**Q：Live Server 端口不是 5501？**
A：检查 `.vscode/settings.json`，或直接忽略端口差异，只要 `API_BASE` 指向正确的后端地址即可。