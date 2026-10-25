# React Native 小白学习路线（基于 Expo）

> 适用人群：有一定 React / 前端基础，想从零开始学 React Native 的初学者
> 技术栈：Expo SDK 57 + Expo Router + TypeScript（与本项目一致）

---

## 🗺️ 学习路线总览

```
第 1 阶段：环境与基础（1 周）
    ↓
第 2 阶段：核心组件与 API（1-2 周）
    ↓
第 3 阶段：导航与路由（1 周）
    ↓
第 4 阶段：状态管理（1 周）
    ↓
第 5 阶段：网络请求与数据处理（1 周）
    ↓
第 6 阶段：原生模块与设备能力（1-2 周）
    ↓
第 7 阶段：性能优化（1 周）
    ↓
第 8 阶段：测试与发布（1 周）
    ↓
第 9 阶段：进阶主题（持续学习）
```

预计总耗时：约 8-12 周（每天学习 1-2 小时）

---

## 第一阶段：环境搭建与基础概念（1 周）

### 学习目标

- 搭好开发环境，能跑起第一个 React Native 项目
- 理解 React Native 的基本原理

### 学习内容

| 知识点 | 说明 | 推荐资源 |
|--------|------|---------|
| **Expo 是什么** | 理解 Expo 和 React Native 的关系 | [Expo 官方文档](https://docs.expo.dev/) |
| **环境搭建** | 安装 Node.js、Expo CLI、Expo Go App | [Expo 安装指南](https://docs.expo.dev/get-started/installation/) |
| **创建第一个项目** | `npx create-expo-app` 跑起来 | 跟着官方 Tutorial 做一遍 |
| **基础组件** | View、Text、Image、StyleSheet | 官方 Tutorial |
| **Flexbox 布局** | React Native 的布局方式 | [React Native Layout 文档](https://reactnative.dev/docs/flexbox) |
| **Fast Refresh** | 热重载机制 | 本项目文档：ReactNative热重载机制详解.md |

### 动手练习

- [ ] 用 `create-expo-app` 创建一个空白项目并跑起来
- [ ] 在 Expo Go 真机上运行
- [ ] 试着修改文案和颜色，观察热重载效果
- [ ] 用 View + Text + StyleSheet 写一个简单的个人介绍页面

### 注意事项

- **不要上来就配原生环境**：先用 Expo Go，等需要时再考虑开发构建
- **重点理解 Flexbox**：React Native 没有 CSS，布局全靠 Flexbox
- **React 基础要扎实**：如果你不熟悉 React Hooks，先回去补 React 基础

---

## 第二阶段：核心组件与 API（1-2 周）

### 学习目标

- 掌握常用的 React Native 核心组件
- 能够独立完成简单的页面开发

### 学习内容

#### 基础组件（必须掌握）

| 组件 | 用途 | 重要程度 |
|------|------|---------|
| `View` | 最基础的容器组件，相当于 `div` | ⭐⭐⭐⭐⭐ |
| `Text` | 文本组件，所有文字都要用它包 | ⭐⭐⭐⭐⭐ |
| `Image` / `expo-image` | 图片展示 | ⭐⭐⭐⭐⭐ |
| `ScrollView` | 滚动容器 | ⭐⭐⭐⭐⭐ |
| `TextInput` | 输入框 | ⭐⭐⭐⭐⭐ |
| `Button` | 按钮 | ⭐⭐⭐⭐ |
| `TouchableOpacity` | 可点击容器（自定义按钮） | ⭐⭐⭐⭐⭐ |
| `TouchableHighlight` | 点击高亮 | ⭐⭐⭐ |
| `FlatList` | 长列表（高性能） | ⭐⭐⭐⭐⭐ |
| `SectionList` | 分组列表 | ⭐⭐⭐ |
| `Modal` | 模态框 | ⭐⭐⭐⭐ |
| `Alert` | 系统弹窗 | ⭐⭐⭐⭐ |
| `ActivityIndicator` | 加载转圈 | ⭐⭐⭐⭐ |

#### 样式与布局

| 知识点 | 说明 |
|--------|------|
| **StyleSheet** | 样式写法，和 CSS 类似但有区别 |
| **Flexbox** | 主轴、交叉轴、justifyContent、alignItems |
| **响应式布局** | 不同屏幕尺寸适配，useWindowDimensions |
| **像素密度** | DPI、独立像素（dp）的概念 |

#### 手势与交互

| 组件 | 用途 |
|------|------|
| `Pressable` | 更灵活的可按压组件 |
| `PanResponder` | 手势识别系统 |
| `react-native-gesture-handler` | 更强大的手势库（Expo 已集成） |

### 动手练习

- [ ] 写一个 Todo List（增加、删除、标记完成）
- [ ] 写一个联系人列表（用 FlatList，带头像和姓名）
- [ ] 写一个登录页面（用户名、密码输入框、登录按钮）
- [ ] 写一个九宫格布局（6 个图标，3 列 2 行）

---

## 第三阶段：导航与路由（1 周）

### 学习目标

- 掌握 Expo Router 的使用
- 能搭建有多个页面的 App 结构

### 学习内容

| 知识点 | 说明 | 重要程度 |
|--------|------|---------|
| **Expo Router 基础** | 文件系统路由，类似 Next.js | ⭐⭐⭐⭐⭐ |
| **堆栈导航（Stack）** | 页面进进出出，带转场动画 | ⭐⭐⭐⭐⭐ |
| **底部标签导航（Tabs）** | 底部 Tab 切换 | ⭐⭐⭐⭐⭐ |
| **抽屉导航（Drawer）** | 侧边栏抽屉 | ⭐⭐⭐ |
| **页面传参** | `useLocalSearchParams`、`router.push` | ⭐⭐⭐⭐⭐ |
| **返回上一页** | `router.back()` | ⭐⭐⭐⭐⭐ |
| **嵌套导航** | Tabs 里面套 Stack | ⭐⭐⭐⭐ |
| **路由守卫** | 登录拦截、权限控制 | ⭐⭐⭐ |

### 推荐资源

- [Expo Router 官方文档](https://docs.expo.dev/router/introduction/)
- 本项目 `src/app/` 目录可以直接参考

### 动手练习

- [ ] 搭建一个带底部 Tab 的 App 结构（首页、列表、我的）
- [ ] 实现列表页 → 详情页的跳转和传参
- [ ] 实现登录页 → 首页的跳转（登录后不能返回登录页）
- [ ] 加一个左侧抽屉导航

---

## 第四阶段：状态管理（1 周）

### 学习目标

- 理解 React 状态管理的不同方案
- 选择并掌握一个适合自己的方案

### 学习内容

#### 方案一：React 内置方案（先掌握这个）

| 方案 | 适用场景 |
|------|---------|
| `useState` | 组件内局部状态 |
| `useReducer` | 复杂的组件内状态逻辑 |
| `useContext` + `useReducer` | 跨组件共享状态（中小型项目够用） |

#### 方案二：第三方库（选一个深入）

| 库 | 特点 | 推荐指数 |
|----|------|---------|
| **Zustand** | 极简、轻量、上手最快 | ⭐⭐⭐⭐⭐（新手推荐） |
| **Redux Toolkit** | 官方推荐、生态完善、较复杂 | ⭐⭐⭐⭐ |
| **MobX** | 响应式、代码少、有学习曲线 | ⭐⭐⭐ |
| **Jotai** | 原子化状态管理，类似 Recoil | ⭐⭐⭐ |

#### 数据持久化

| 方案 | 说明 |
|------|------|
| `AsyncStorage` | 简单的键值存储（已从核心移除，需单独安装） |
| `expo-secure-store` | 安全存储（存 token、密码） |
| `MMKV` | 高性能键值存储（微信出品） |
| `SQLite` / `expo-sqlite` | 关系型数据库，复杂本地数据 |

### 学习建议

1. **先用好 `useState` 和 `useContext`**，理解状态提升
2. 再选一个第三方库，**推荐 Zustand**（最简单直接）
3. 数据持久化先用 `AsyncStorage` 或 `expo-secure-store`

### 动手练习

- [ ] 用 Context + useReducer 重构之前的 Todo List
- [ ] 接入 Zustand（或你选的库），实现全局状态
- [ ] 实现"登录状态"的全局管理和持久化
- [ ] 实现主题切换（亮色/暗色模式）

---

## 第五阶段：网络请求与数据处理（1 周）

### 学习目标

- 掌握网络请求的方式
- 能处理列表加载、分页、下拉刷新等常见场景

### 学习内容

#### 网络请求

| 方案 | 说明 | 推荐 |
|------|------|------|
| **Fetch API** | React Native 内置，基础够用 | 入门用 |
| **Axios** | 功能更全，拦截器、取消请求等 | ⭐⭐⭐⭐⭐ 推荐 |

#### 数据处理与缓存

| 库 | 用途 | 推荐 |
|----|------|------|
| **TanStack Query (React Query)** | 服务端状态管理、缓存、自动刷新 | ⭐⭐⭐⭐⭐ 强烈推荐 |
| **SWR** | 类似 React Query，更轻量 | ⭐⭐⭐⭐ |
| **Zod / Yup** | 数据验证（TypeScript 友好） | ⭐⭐⭐⭐ |

#### 常见场景

| 场景 | 实现方式 |
|------|---------|
| **下拉刷新** | `FlatList` 的 `refreshing` + `onRefresh` |
| **上拉加载更多** | `FlatList` 的 `onEndReached` |
| **加载状态** | 骨架屏（`react-native-skeleton`）或 ActivityIndicator |
| **错误处理** | try/catch、错误提示、重试按钮 |
| **取消请求** | AbortController 或 Axios CancelToken |

### 动手练习

- [ ] 接入一个公开 API（如 [JSONPlaceholder](https://jsonplaceholder.typicode.com/) 或 [Punk API](https://punkapi.com/)）
- [ ] 实现一个带下拉刷新和上拉加载的列表
- [ ] 用 React Query 管理数据缓存
- [ ] 添加加载状态、错误重试、空状态

---

## 第六阶段：原生模块与设备能力（1-2 周）

### 学习目标

- 了解 Expo 提供的常用原生能力
- 能调用相机、定位、通知等设备功能

### 必学模块（按使用频率排序）

| 模块 | 用途 | 重要程度 |
|------|------|---------|
| `expo-image-picker` | 拍照、选图 | ⭐⭐⭐⭐⭐ |
| `expo-location` | 获取地理位置 | ⭐⭐⭐⭐⭐ |
| `expo-notifications` | 推送通知 | ⭐⭐⭐⭐⭐ |
| `expo-camera` | 相机（扫码、拍照） | ⭐⭐⭐⭐ |
| `expo-av` | 音视频播放 | ⭐⭐⭐⭐ |
| `expo-file-system` | 文件系统操作 | ⭐⭐⭐ |
| `expo-sensors` | 加速度计、陀螺仪等传感器 | ⭐⭐⭐ |
| `expo-haptics` | 震动反馈 | ⭐⭐⭐ |
| `expo-sharing` | 系统分享面板 | ⭐⭐⭐ |
| `expo-contacts` | 读取通讯录 | ⭐⭐ |

### 动画

| 库 | 说明 | 推荐 |
|----|------|------|
| **Animated（内置）** | React Native 自带动画库 | 基础入门 |
| **react-native-reanimated** | 更强大的手势驱动动画，60fps | ⭐⭐⭐⭐⭐ 推荐 |
| **Lottie** | 设计师导出的 JSON 动画 | ⭐⭐⭐⭐ |
| **Moti** | 基于 Reanimated 的声明式动画库 | ⭐⭐⭐ |

### 动手练习

- [ ] 实现头像选择（拍照或从相册选）
- [ ] 实现获取当前位置并显示地图（`react-native-maps`）
- [ ] 做一个简单的音乐播放器（播放/暂停/进度条）
- [ ] 用 Reanimated 做一个拖拽动画效果

---

## 第七阶段：性能优化（1 周）

### 学习目标

- 了解 React Native 常见性能问题
- 能进行基本的性能优化

### 学习内容

#### 列表性能

| 优化点 | 说明 |
|--------|------|
| **FlatList 正确使用** | keyExtractor、getItemLayout、initialNumToRender |
| **避免不必要的重渲染** | React.memo、useMemo、useCallback |
| **图片优化** | 合适的尺寸、缓存（`expo-image` 已内置） |
| **大列表虚拟化** | FlatList 本身就是虚拟化列表 |

#### 渲染性能

| 优化点 | 说明 |
|--------|------|
| **React DevTools Profiler** | 分析渲染性能 |
| **Hermes 引擎** | 更快的启动速度、更小的内存占用（Expo 默认开启） |
| **避免内联函数** | 子组件频繁渲染时注意 |
| **懒加载** | 路由懒加载、组件懒加载 |

#### 启动优化

| 优化点 | 说明 |
|--------|------|
| **首屏加载** | 首屏内容尽量少，延迟加载非必要内容 |
| **字体预加载** | `expo-font` |
| **资源优化** | 压缩图片、减小包体积 |

### 调试工具

- **React DevTools**：组件树、props、state 检查
- **Performance Monitor**：Expo 开发菜单中的性能监视器
- **Flipper**：Facebook 出品的调试工具（原生性能分析）
- **Console / React Native Debugger**：日志调试

### 动手练习

- [ ] 用 React DevTools 分析自己项目的渲染性能
- [ ] 优化一个长列表，确保滚动流畅
- [ ] 检查并优化 App 启动速度

---

## 第八阶段：测试与发布（1 周）

### 学习目标

- 了解 React Native 的测试方法
- 能打包发布到应用商店

### 测试

| 测试类型 | 工具 | 重要程度 |
|---------|------|---------|
| **单元测试** | Jest + React Native Testing Library | ⭐⭐⭐⭐ |
| **组件测试** | React Native Testing Library | ⭐⭐⭐⭐ |
| **端到端测试** | Maestro（Expo 推荐）/ Detox | ⭐⭐⭐ |
| **快照测试** | Jest Snapshot | ⭐⭐ |

### 构建与发布（EAS）

| 阶段 | 命令 / 工具 | 说明 |
|------|------------|------|
| **本地开发构建** | `npx expo run:ios/android` | 需要原生代码时用 |
| **EAS 云构建** | `eas build` | 云端编译，不需要本地环境 |
| **Android 发布** | Google Play Console | 打包 aab 上传 |
| **iOS 发布** | App Store Connect | 需要 Apple 开发者账号 |
| **OTA 热更新** | `eas update` | 不发版也能更新 JS 代码 |

### 动手练习

- [ ] 给 Todo List 写几个单元测试
- [ ] 打一个 Android 的 APK 安装到手机
- [ ] （可选）尝试 EAS 云构建

---

## 第九阶段：进阶主题（持续学习）

### 架构与工程化

- [ ] **Monorepo**：Turborepo / Nx，多包管理
- [ ] **代码规范**：ESLint、Prettier、Husky、lint-staged
- [ ] **CI/CD**：GitHub Actions、EAS Build 自动化
- [ ] **类型安全**：TypeScript 严格模式、Zod 运行时验证

### 高级功能

- [ ] **离线支持**：数据缓存、离线队列
- [ ] **推送通知**：FCM（Android）+ APNs（iOS）
- [ ] **深度链接**：App Links、Universal Links
- [ ] **Web 支持**：React Native Web，一套代码三端运行
- [ ] **深色模式**：系统主题适配
- [ ] **国际化（i18n）**：多语言支持

### 原生开发进阶

- [ ] **Config Plugins**：用脚本修改原生配置
- [ ] **原生模块开发**：自己写 Swift/Kotlin 原生模块
- [ ] **新架构（New Architecture）**：Fabric + TurboModules

---

## 🎯 推荐学习资源

### 官方文档

- [Expo 官方文档](https://docs.expo.dev/) — 最权威，优先看
- [React Native 官方文档](https://reactnative.dev/docs/getting-started) — 核心组件和 API
- [Expo Router 文档](https://docs.expo.dev/router/introduction/)

### 视频教程

- [Expo 官方 YouTube 频道](https://www.youtube.com/@Expo)
- [React Native 官方教程](https://reactnative.dev/docs/tutorial)

### 社区资源

- [React Native Directory](https://reactnative.directory/) — 第三方库搜索
- [Expo 博客](https://blog.expo.dev/) — 最新动态
- [Stack Overflow](https://stackoverflow.com/questions/tagged/react-native) — 遇到问题搜这里

### 推荐项目参考

- [本项目的源码](..) — 边学边改
- [Expo 官方示例](https://github.com/expo/examples) — 各种功能示例

---

## 💡 给小白的建议

1. **动手最重要**：不要光看视频，一定要跟着敲代码
2. **从小项目开始**：Todo List → 天气 App → 社区 App，逐步增加复杂度
3. **善用 Expo Go**：开发阶段先用 Expo Go，别着急搞原生环境
4. **遇到报错先搜**：99% 的问题别人都遇到过
5. **React 基础打牢**：Hooks 不熟练会很痛苦
6. **不要追求完美**：先跑起来，再优化
7. **多看看本项目代码**：项目里的写法都是最佳实践，可以直接参考

---

## 📅 学习计划模板（每日 1.5 小时）

| 周次 | 阶段 | 每日任务 |
|------|------|---------|
| 第 1 周 | 基础 | 每天学 2-3 个组件，做小练习 |
| 第 2-3 周 | 核心组件 | 每天写一个小页面/小功能 |
| 第 4 周 | 导航 | 用 Expo Router 搭一个多页面 App |
| 第 5 周 | 状态管理 | 重构之前的项目，引入状态管理 |
| 第 6 周 | 网络请求 | 接入真实 API，做列表页 |
| 第 7-8 周 | 设备能力 | 每周学 2-3 个原生模块 |
| 第 9 周 | 性能优化 | 分析并优化自己的项目 |
| 第 10 周 | 测试发布 | 写测试，尝试打包发布 |

> 不必严格按时间来，根据自己的节奏调整。重点是持续学习+动手实践。
