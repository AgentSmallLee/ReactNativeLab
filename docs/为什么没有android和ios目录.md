# 为什么 React Native 项目没有 `android/` 和 `ios/` 目录

## 结论

没有 `android/` 和 `ios/` 目录是**正常且预期的**。这是 Expo 现代化工作流的设计，不是项目缺失。

本项目使用的是 **Expo SDK 57**，属于 Expo 托管工作流（managed workflow）。

## 原因：Continuous Native Generation（持续原生生成）

Expo 采用了 **CNG（Continuous Native Generation）** 机制：原生项目文件不再手动维护，而是由 Expo 根据 `app.json` 配置**按需自动生成**。

> 项目规范：如果 `ios/` 和 `android/` 目录不存在，说明它们是自动生成的（Continuous Native Generation）。**永远不要手动创建或编辑它们** —— 在 `app.json` 和 config plugins 中配置原生行为。

## 两种运行方式，两种生成时机

| 方式 | 原生目录何时生成 | 说明 |
|------|------------------|------|
| **Expo Go** (`expo start`) | 永远不会生成 | 直接运行在 Expo Go 客户端里，使用其内置的原生模块 |
| **开发构建** (`expo run:ios` / `expo run:android`) | 运行命令时自动生成 | 会在项目根目录下生成 `ios/` 和 `android/`，然后编译安装 |
| **EAS 云构建** (`eas build`) | 在云端生成 | 原生目录在 EAS 服务器上生成，本地不会出现 |

## 原生配置写在哪里？

所有原生相关的配置都通过 `app.json` 和 **Config Plugins** 来声明式配置，而不是直接改 Xcode / Gradle 文件：

```json
// app.json - 配置的单一真相来源
{
  "expo": {
    "name": "ReactNativeLab",
    "ios": { "bundleIdentifier": "..." },
    "android": { "package": "..." },
    "plugins": [/* config plugins */]
  }
}
```

## 如何手动生成原生目录？

运行以下命令会自动生成 `ios/` 和 `android/` 目录：

```bash
# 仅生成，不运行
npx expo prebuild

# 生成并运行 iOS（需要 macOS + Xcode）
npx expo run:ios

# 生成并运行 Android
npx expo run:android
```

> **注意**：不建议手动编辑这些生成的文件，因为下次 `prebuild` 时会被覆盖。所有原生配置应通过 `app.json` 和 config plugins 声明式管理。

## 总结

没有 `android/` / `ios/` 不是缺失，而是 Expo 现代化工作流的设计：用声明式配置替代手动维护原生项目，减少平台差异带来的维护负担。需要时它们会自动生成。
