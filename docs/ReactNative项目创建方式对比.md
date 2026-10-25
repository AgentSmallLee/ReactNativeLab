# React Native 项目创建方式与区别

## 概览

| 创建方式 | 命令 | 原生目录 | 上手难度 | 自定义程度 | 推荐场景 |
|---------|------|---------|---------|-----------|---------|
| **Expo (create-expo-app)** | `npx create-expo-app` | 自动生成（CNG） | ⭐ 最简单 | 中高 | 大多数应用、快速原型 |
| **React Native CLI** | `npx @react-native-community/cli init` | 手动维护 | ⭐⭐⭐ 较复杂 | 最高 | 需要深度定制原生代码 |
| **Ignite** | `npx ignite-cli new` | 基于 Expo | ⭐⭐ 简单 | 高 | 生产级应用模板 |
| **Expo 模板** | `npx create-expo-app --template <name>` | 自动生成 | ⭐ 最简单 | 中高 | 特定技术栈快速启动 |

---

## 1. Expo（create-expo-app）⭐ 推荐大多数人使用

### 命令

```bash
# 基础项目
npx create-expo-app MyApp

# TypeScript 模板（默认就是 TS）
npx create-expo-app MyApp --template expo-template-blank-typescript

# Expo Router 模板（带路由）
npx create-expo-app MyApp --template expo-template-router
```

### 特点

- **托管工作流（Managed Workflow）**：原生代码由 Expo 管理，不需要手动维护 `ios/` 和 `android/` 目录
- **Continuous Native Generation (CNG)**：根据 `app.json` 配置自动生成原生项目
- **Expo Go**：可以直接在 Expo Go App 中运行，无需编译
- **Expo SDK**：提供大量经过验证的原生模块（相机、地图、推送通知等）
- **EAS Build**：云端构建，不需要本地安装 Xcode / Android Studio
- **OTA 更新**：通过 EAS Update 发布热更新
- **Expo Router**：文件系统路由，类似 Next.js

### 优点

- ✅ 上手极快，几分钟就能运行
- ✅ 不需要配置原生开发环境
- ✅ 跨平台一致性好
- ✅ 升级 SDK 相对简单
- ✅ 丰富的官方原生模块
- ✅ 云构建和热更新开箱即用

### 缺点

- ❌ 深度自定义原生代码较麻烦（需要写 config plugins）
- ❌ 第三方原生库如果没有 Expo 适配，需要自己处理
- ❌ 包体积比纯原生略大

### 适用场景

- 大多数业务 App
- 快速原型开发
- 团队没有原生开发经验
- 希望跨平台维护成本最低

---

## 2. React Native CLI（裸项目 / Bare Workflow）

### 命令

```bash
npx @react-native-community/cli init MyApp

# 指定版本
npx @react-native-community/cli init MyApp --version 0.74.0
```

### 特点

- **裸工作流（Bare Workflow）**：生成完整的 `ios/` 和 `android/` 目录，完全由你掌控
- 直接使用 React Native 官方仓库，没有 Expo 层
- 需要本地安装 Xcode 和 Android Studio
- 原生代码完全可定制

### 优点

- ✅ 原生代码完全可控，可深度定制
- ✅ 可直接使用任何第三方原生库
- ✅ 没有 Expo SDK 的额外依赖
- ✅ 最接近原生开发体验

### 缺点

- ❌ 上手门槛高，需要配置原生开发环境
- ❌ 跨平台维护成本高
- ❌ 升级 React Native 版本较麻烦
- ❌ 需要自己处理构建、签名、发布
- ❌ 没有开箱即用的云构建和热更新

### 适用场景

- 需要大量自定义原生模块
- 有专门的 iOS/Android 开发团队
- 对包体积有严格要求
- 需要集成复杂的原生 SDK

---

## 3. Ignite CLI

### 命令

```bash
npx ignite-cli new MyApp
```

### 特点

- Infinite Red 团队维护的生产级 React Native 模板
- 基于 Expo + Expo Router
- 内置了完整的技术栈和最佳实践

### 内置功能

- Expo Router（路由）
- MobX / Zustand（状态管理）
- TypeScript
- React Native Testing Library（测试）
- 主题系统
- 组件库
- 项目结构约定

### 优点

- ✅ 生产级模板，省掉大量搭建工作
- ✅ 内置最佳实践
- ✅ 基于 Expo，仍享受 Expo 生态
- ✅ 有专门的维护团队

### 缺点

- ❌ 技术栈固定（比如状态管理用 MobX），换栈需要改
- ❌ 模板较重，小项目可能用不上那么多功能

### 适用场景

- 中大型项目
- 希望有现成的项目结构和技术规范
- 团队想减少前期搭建成本

---

## 4. Expo 官方模板

`create-expo-app` 支持多种 `--template` 参数，快速搭建不同类型的项目：

| 模板名 | 说明 |
|-------|------|
| `blank` | 空白项目（JavaScript） |
| `blank-typescript` | 空白项目（TypeScript），默认 |
| `tabs` | 带底部 Tab 导航的模板 |
| `tabs-typescript` | Tab 导航 + TypeScript |
| `expo-template-router` | Expo Router 模板 |
| `expo-template-default` | 默认模板（带示例页面） |
| `expo-template-blank` | 最精简的空白模板 |

### 命令示例

```bash
# 带底部 Tab 的 TypeScript 项目
npx create-expo-app MyApp --template tabs-typescript

# 使用 Expo Router
npx create-expo-app MyApp --template expo-template-router
```

---

## 5. 其他创建方式

### 5.1 从已有 Expo 项目切换到裸项目

```bash
# 在 Expo 项目中运行，生成 ios/ 和 android/ 目录
npx expo prebuild
```

这是 Expo 项目的"弹出"（eject）操作，生成可手动编辑的原生目录。但即使生成了原生目录，仍然可以继续使用 Expo SDK 和工具链。

### 5.2 Create React Native App（已废弃）

早期的创建工具，现在已经被 `create-expo-app` 取代，不再推荐使用。

---

## 详细对比

### 功能对比

| 功能 | Expo | React Native CLI | Ignite |
|------|------|------------------|--------|
| 快速启动 | ✅ 分钟级 | ❌ 需要配置环境 | ✅ 分钟级 |
| 无需原生环境 | ✅ | ❌ | ✅（基于Expo） |
| 原生代码完全可控 | ⚠️ 需要 config plugin | ✅ | ⚠️ 需要 config plugin |
| 云构建 | ✅ EAS Build | ❌ 自己搭建 | ✅ EAS Build |
| 热更新 OTA | ✅ EAS Update | ❌ 自己集成 | ✅ EAS Update |
| Expo SDK 模块 | ✅ | ❌ 需单独安装 | ✅ |
| 路由 | Expo Router | 自己选 | Expo Router |
| 状态管理 | 自己选 | 自己选 | MobX（内置） |
| 升级难度 | 低（expo upgrade） | 高 | 中 |
| 包体积 | 稍大 | 小 | 稍大 |

### 原生代码访问能力对比

```
React Native CLI（完全可控）
    ↑
    ┊ 可通过 prebuild 获得原生目录
    ↓
Expo Managed（通过 config plugins 声明式配置）
```

- **完全可控**：直接编辑 Xcode / Android Studio 项目文件
- **声明式配置**：通过 `app.json` 的 `plugins` 数组配置，Expo 在构建时自动应用

---

## 如何选择？

### 推荐用 Expo 的情况

- 你是 React Native 新手
- 团队没有原生开发经验
- 想快速验证想法 / 做原型
- 应用主要是业务逻辑，不需要特别的原生功能
- 希望维护成本低
- 想使用云构建和热更新

### 推荐用 React Native CLI 的情况

- 应用需要大量自定义原生代码
- 需要集成没有 Expo 适配的复杂原生 SDK
- 有专门的原生开发团队
- 对包体积有极致要求
- 需要完全掌控构建流程

### 推荐用 Ignite 的情况

- 选择了 Expo，但想要完整的项目模板
- 团队想统一技术栈和代码规范
- 希望减少前期搭建工作

---

## 本项目说明

本项目使用的是 **Expo SDK 57 + Expo Router**，属于 **Expo 托管工作流**：

- 创建命令：`npx create-expo-app ReactNativeLab --template expo-template-router`
- 没有 `ios/` 和 `android/` 目录是正常的（由 CNG 自动生成）
- 原生配置通过 `app.json` 和 config plugins 管理
- 可以使用 EAS 进行云构建和 OTA 热更新
