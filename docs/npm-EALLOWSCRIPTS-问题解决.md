# npm EALLOWSCRIPTS 错误解决方案

## 问题描述

通过 `npx` 创建 React Native 项目时，出现以下错误：

```
npm error code EALLOWSCRIPTS
npm error --allow-scripts is not allowed in project-scoped installs.
Add the entries to the "allowScripts" field in package.json, or to .npmrc, instead.
npm error A complete log of this run can be found in: /Users/mac/.npm/_logs/2026-10-24T01_51_59_225Z-debug-0.log
Something went wrong installing JavaScript dependencies. Check your npm logs.
Continuing to create the app.
Error: npm install exited with non-zero code: 1
```

---

## 根本原因

全局 `~/.npmrc` 中配置了 `allow-scripts` 白名单限制：

```ini
; ~/.npmrc
allow-scripts=@volcengine/ark-cli
```

### 为什么会报错

1. npm 从 v11 开始支持 `allow-scripts` 配置，用于限制安装包时可以执行哪些脚本（如 `postinstall`、`preinstall` 等），提升安全性。
2. 当 `allow-scripts` 被设置为**有限的白名单**（例如只允许 `@volcengine/ark-cli`）时，其他所有包的安装脚本都会被阻止执行。
3. React Native 项目依赖大量需要运行 `postinstall` 脚本的包（如 `@react-native/cli`、`pod-install`、`hermes-engine` 等），脚本被阻止后安装失败。
4. `npx react-native init` 内部以「项目作用域」调用 `npm install`，并尝试使用 `--allow-scripts` 标志，而 npm 的安全策略不允许在 project-scoped 安装中使用该标志，因此抛出 `EALLOWSCRIPTS`。

简单说：**全局 `.npmrc` 的 `allow-scripts` 白名单太严格，React Native 依赖的包无法执行安装脚本。**

---

## 解决方案

### 思路

临时解除全局限制 → 创建项目 → 在项目内配置 `allowScripts` → 恢复全局限制

这样既保留了全局的安全默认，又让 React Native 项目能正常安装依赖。

---

### 步骤 1：备份并临时禁用全局 allow-scripts

```bash
# 备份
cp ~/.npmrc ~/.npmrc.backup

# 注释掉 allow-scripts 行（或直接删除）
# 编辑 ~/.npmrc，将 allow-scripts 那一行注释掉
```

修改后的 `~/.npmrc`：

```ini
registry=https://registry.npmmirror.com
prefix=/Users/mac/.npm-global
# allow-scripts temporarily disabled for React Native project creation
# allow-scripts=@volcengine/ark-cli
```

---

### 步骤 2：创建 React Native 项目

```bash
npx react-native@latest init MyApp
```

等待项目创建和依赖安装完成。

---

### 步骤 3：在项目中配置 allowScripts

进入项目目录，编辑项目根目录下的 `.npmrc` 或 `package.json`，明确允许 React Native 相关包执行脚本。

#### 方案 A：项目级 `.npmrc`（推荐）

在项目根目录创建/编辑 `.npmrc`：

```ini
; MyApp/.npmrc
; 允许 React Native 及相关依赖运行安装脚本
allow-scripts[]=*
```

> `allow-scripts[]=*` 表示该项目内所有包都可以执行脚本。
> 如果你希望更精细地控制，可以逐个添加需要的包名。

#### 方案 B：在 `package.json` 中配置

```json
{
  "name": "MyApp",
  "version": "0.0.1",
  "private": true,
  "allowScripts": {
    "@react-native/cli": true,
    "hermes-engine": true,
    "pod-install": true,
    "react-native": true,
    "fsevents": true
  }
}
```

> 注意：React Native 生态中执行脚本的包较多，完整白名单需要自行梳理。对于大多数项目，方案 A（项目级允许全部）更省心。

---

### 步骤 4：恢复全局 .npmrc 限制

```bash
# 恢复备份
cp ~/.npmrc.backup ~/.npmrc

# 或者手动取消注释
```

恢复后全局仍保持只允许 `@volcengine/ark-cli` 运行脚本的安全策略，
而 React Native 项目因为有自己的 `.npmrc` 配置，不会受到全局限制的影响。

---

## 验证

项目创建完成后，进入项目目录验证：

```bash
cd MyApp
npm install   # 应该能正常完成，不再报 EALLOWSCRIPTS
npm run ios   # 或 npm run android，确保项目能正常运行
```

---

## 补充说明

### 为什么 npx 不能用 --allow-scripts？

npm 的 `--allow-scripts` CLI 标志**只允许在全局安装**（`-g`）时使用，
在项目级安装（project-scoped install）中使用会直接被拒绝，这是 npm 的安全设计。
因此 `npx react-native init` 无法通过加参数的方式绕过。

### 相关版本信息

| 工具 | 版本 |
|------|------|
| npm  | 11.19.0 |
| Node | v26.7.0 |
| 系统 | macOS Darwin 27.0.0 (Apple Silicon) |

### 参考链接

- [npm allow-scripts 官方文档](https://docs.npmjs.com/cli/v11/using-npm/config#allow-scripts)
- [npm security: allow-scripts](https://github.com/npm/rfcs/blob/main/accepted/0000-allow-scripts.md)
