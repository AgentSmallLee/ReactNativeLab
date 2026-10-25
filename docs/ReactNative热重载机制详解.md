# React Native 热重载机制详解

## 快速回答

**有，而且 React Native 的热重载体验非常流畅。** 你感受到的"焦点离开编辑器就生效"是正常的 —— 这是 **Fast Refresh（快速刷新）** 机制在工作。

---

## 什么是 Fast Refresh？

Fast Refresh 是 React Native 从 0.61 版本开始引入的官方热重载机制，替代了之前的 Hot Reloading 和 Live Reload。

### 核心特性

| 特性 | 说明 |
|------|------|
| **保存即刷新** | 保存文件后自动更新，不需要手动刷新 |
| **保留组件状态** | 修改组件样式、逻辑时，组件的 state 不会丢失 |
| **语法错误容错** | 出错时显示红屏错误，修复后自动恢复 |
| **函数组件全面支持** | 对 React Hooks 有完整支持 |

### 工作原理

```
编辑代码 → 保存文件 → Metro 检测变化 → 重新编译 → 通过 WebSocket 推送到设备
                                                          ↓
                                                    替换模块代码
                                                          ↓
                                                    组件重新渲染
```

1. **Metro bundler** 监听文件变化（Metro 是 React Native 的打包工具）
2. 文件保存后，Metro 增量编译变更的模块
3. 通过 WebSocket 将新代码推送到运行中的 App
4. React 运行时热替换模块，重新渲染受影响的组件

---

## 为什么"焦点离开编辑器就生效"？

这是因为你的编辑器设置了 **失焦自动保存（auto-save on focus change）**，而不是因为焦点变化本身触发了刷新。

### 完整流程

```
你在 VS Code 里改代码
    ↓
焦点离开编辑器（切到模拟器/浏览器）
    ↓
编辑器自动保存文件（autoSave: onFocusChange）
    ↓
Metro 检测到文件变化
    ↓
Fast Refresh 推送新代码到 App
    ↓
界面更新
```

### VS Code 的自动保存设置

在 VS Code 的 `settings.json` 中：

```json
{
  // 焦点变化时自动保存（默认值）
  "files.autoSave": "onFocusChange",

  // 其他可选值：
  // "off"        - 关闭自动保存，需手动 Ctrl+S
  // "afterDelay" - 输入停止一段时间后自动保存
  // "onWindowChange" - 窗口失去焦点时保存
}
```

如果你想手动控制刷新时机，可以把 `autoSave` 改成 `"off"`，然后用 `Cmd+S` / `Ctrl+S` 手动保存触发刷新。

---

## Fast Refresh 的边界

Fast Refresh 很强大，但也有它的局限性：

### ✅ 会保留状态的情况

- 修改函数组件的样式、布局
- 修改组件内部逻辑（事件处理、计算逻辑）
- 新增/修改 Hooks
- 修改常量、工具函数

### ❌ 会重置状态的情况

- 修改了组件的**类名**或**函数名**
- 修改了组件的**默认导出**名称
- 修改了 **`constructor`** 或 **`useEffect` 的依赖**（有时）
- 修改了 **全局变量** 或 **模块顶层代码**
- 修改了 **`state` 的初始值**
- 修改了 **类型定义**（TypeScript 不影响运行时，但可能需要全量刷新）

### 🚨 需要完全重启的情况

- 修改了 **原生代码**（`ios/`、`android/` 目录）
- 修改了 **`app.json`** 配置
- 新增了 **带原生代码的依赖库**
- 修改了 **Metro 配置**（`metro.config.js`）
- 修改了 **babel 配置**（`babel.config.js`）
- 修改了 **环境变量**

---

## 开发调试快捷键

### iOS 模拟器

| 快捷键 | 功能 |
|--------|------|
| `Cmd + R` | 手动刷新（Reload） |
| `Cmd + D` | 打开开发菜单 |
| `Cmd + Shift + Z` | 撤销（触发 shake 手势） |

### Android 模拟器

| 快捷键 | 功能 |
|--------|------|
| `R + R`（双击 R） | 手动刷新 |
| `Cmd + M` / `Ctrl + M` | 打开开发菜单 |

### 开发菜单中的选项

- **Reload**：全量重新加载（相当于重启 JS）
- **Enable Fast Refresh**：开启/关闭快速刷新
- **Toggle Inspector**：元素检查器
- **Show Perf Monitor**：性能监视器
- **Debug Remote JS**：远程 JS 调试

---

## Expo 环境下的额外能力

本项目使用 Expo，在 Fast Refresh 基础上还有更多能力：

### Expo Go

- 直接扫码即可在真机上运行，享受同样的热重载体验
- 保存后真机上的 App 也会立即更新

### EAS Update（生产环境热更新）

Fast Refresh 是**开发时**的热重载。在生产环境中，Expo 还提供了 **EAS Update**：

```bash
# 发布 OTA 更新
eas update --branch production
```

用户不需要重新下载 App，下次打开时自动更新。

> 注意：EAS Update 是生产级的 OTA 更新服务，和开发时的 Fast Refresh 是两回事。

---

## 常见问题

### Q: Fast Refresh 没反应怎么办？

1. 确认 Metro bundler 正在运行（终端里有 Metro 日志）
2. 确认设备和电脑在同一 WiFi 网络（真机调试时）
3. 尝试手动刷新：`Cmd + R`（iOS）或双击 R（Android）
4. 如果还是不行，清缓存重启：

```bash
npx expo start -c
# 或
npx react-native start --reset-cache
```

### Q: 修改了 state 初始值为什么没生效？

Fast Refresh 会保留组件 state。如果想看到初始值变化，可以：
- 手动 Reload（`Cmd + R`）
- 或者在开发菜单中关闭再重新开启 Fast Refresh

### Q: 原生代码修改后为什么不刷新？

原生代码（Swift / Kotlin / Java / Objective-C）需要重新编译，Fast Refresh 只针对 JavaScript / TypeScript 代码。原生代码修改后需要重新运行：

```bash
npx expo run:ios
npx expo run:android
```

### Q: Web 端和移动端的热重载一样吗？

React Native Web（`expo start --web`）使用的是 **HMR（Hot Module Replacement）**，由 Webpack 或 Vite 提供，原理类似但实现不同。体验上都是保存即刷新。

---

## 小结

- React Native 通过 **Fast Refresh** 实现热重载，体验流畅
- "焦点离开就生效"本质是**编辑器自动保存 + Fast Refresh** 的组合效果
- Fast Refresh 能**保留组件状态**，比传统的 Live Reload 更高效
- 原生代码修改需要重新编译，不参与热重载
- Expo 额外提供了生产环境的 **EAS Update** 热更新能力
