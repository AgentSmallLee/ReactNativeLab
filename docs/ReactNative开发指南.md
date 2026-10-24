# React Native 移动端 App 开发指南

## 目录

1. [环境搭建](#1-环境搭建)
2. [创建第一个项目](#2-创建第一个项目)
3. [项目结构](#3-项目结构)
4. [核心组件与 API](#4-核心组件与-api)
5. [样式与布局](#5-样式与布局)
6. [导航](#6-导航)
7. [状态管理](#7-状态管理)
8. [网络请求](#8-网络请求)
9. [调试与测试](#9-调试与测试)
10. [打包与发布](#10-打包与发布)
11. [常见问题](#11-常见问题)

---

## 1. 环境搭建

### 1.1 必备工具

- **Node.js**: 建议 18.x 或更高版本
- **Java Development Kit (JDK)**: Android 开发需要，建议 JDK 17
- **Android Studio**: 用于 Android 开发和模拟器
- **Xcode**: 用于 iOS 开发（仅 macOS）
- **Watchman**: Facebook 开源的文件监听工具，建议安装

### 1.2 安装 React Native CLI

```bash
# 使用官方推荐的 Expo 方式（快速上手）
npx create-expo-app@latest

# 或使用原生 React Native CLI（需要完整原生环境）
npm install -g react-native-cli
```

### 1.3 环境变量配置（macOS）

```bash
# 在 ~/.zshrc 或 ~/.bash_profile 中添加
export ANDROID_HOME=$HOME/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/platform-tools
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin

export JAVA_HOME=/Library/Java/JavaVirtualMachines/zulu-17.jdk/Contents/Home
```

### 1.4 验证环境

```bash
# 检查 React Native 环境
npx react-native doctor

# 或 Expo 环境
npx expo --version
```

---

## 2. 创建第一个项目

### 2.1 使用 Expo（推荐新手）

```bash
# 创建项目
npx create-expo-app MyFirstApp --template blank

# 进入目录
cd MyFirstApp

# 启动开发服务器
npx expo start
```

然后用手机扫描二维码，或按 `i` 启动 iOS 模拟器，按 `a` 启动 Android 模拟器。

### 2.2 使用 React Native CLI

```bash
# 创建项目
npx react-native@latest init MyFirstApp

# 进入目录
cd MyFirstApp

# 运行 Android
npx react-native run-android

# 运行 iOS（仅 macOS）
npx react-native run-ios
```

---

## 3. 项目结构

```
MyFirstApp/
├── android/              # Android 原生代码
├── ios/                  # iOS 原生代码
├── src/                  # 源代码（建议自建）
│   ├── components/       # 通用组件
│   ├── screens/          # 页面组件
│   ├── navigation/       # 导航配置
│   ├── hooks/            # 自定义 Hooks
│   ├── utils/            # 工具函数
│   ├── services/         # API 服务
│   ├── store/            # 状态管理
│   └── assets/           # 静态资源
├── App.js                # 入口组件
├── app.json              # 应用配置
├── package.json          # 依赖管理
└── babel.config.js       # Babel 配置
```

---

## 4. 核心组件与 API

### 4.1 基础组件

```jsx
import React from 'react';
import { View, Text, Image, TextInput, Button, ScrollView, FlatList } from 'react-native';

const BasicComponents = () => {
  return (
    <ScrollView>
      {/* 视图容器 */}
      <View style={{ padding: 20 }}>
        {/* 文本 */}
        <Text style={{ fontSize: 20, color: '#333' }}>Hello React Native!</Text>

        {/* 图片 */}
        <Image
          source={{ uri: 'https://example.com/image.png' }}
          style={{ width: 100, height: 100 }}
        />

        {/* 本地图片 */}
        <Image source={require('./assets/logo.png')} />

        {/* 输入框 */}
        <TextInput
          style={{ height: 40, borderColor: 'gray', borderWidth: 1 }}
          placeholder="请输入内容"
          onChangeText={(text) => console.log(text)}
        />

        {/* 按钮 */}
        <Button
          title="点击我"
          onPress={() => alert('按钮被点击了！')}
          color="#841584"
        />
      </View>
    </ScrollView>
  );
};
```

### 4.2 列表组件

```jsx
import React from 'react';
import { FlatList, Text, View, StyleSheet } from 'react-native';

const data = [
  { id: '1', title: '第一项' },
  { id: '2', title: '第二项' },
  { id: '3', title: '第三项' },
];

const ListExample = () => {
  const renderItem = ({ item }) => (
    <View style={styles.item}>
      <Text style={styles.title}>{item.title}</Text>
    </View>
  );

  return (
    <FlatList
      data={data}
      renderItem={renderItem}
      keyExtractor={(item) => item.id}
    />
  );
};

const styles = StyleSheet.create({
  item: {
    backgroundColor: '#f9c2ff',
    padding: 20,
    marginVertical: 8,
    marginHorizontal: 16,
  },
  title: {
    fontSize: 20,
  },
});
```

### 4.3 常用 API

```jsx
import { Alert, ToastAndroid, Platform, Dimensions } from 'react-native';

// 弹窗
Alert.alert(
  '提示',
  '确定要删除吗？',
  [
    { text: '取消', style: 'cancel' },
    { text: '确定', onPress: () => console.log('确认删除') },
  ]
);

// Android Toast
if (Platform.OS === 'android') {
  ToastAndroid.show('消息提示', ToastAndroid.SHORT);
}

// 获取屏幕尺寸
const { width, height } = Dimensions.get('window');

// 判断平台
const isIOS = Platform.OS === 'ios';
```

---

## 5. 样式与布局

### 5.1 Flexbox 布局

React Native 使用 Flexbox 进行布局，和 Web 端类似，但有一些默认值不同：

| 属性 | Web 默认 | RN 默认 |
|------|---------|---------|
| flexDirection | row | column |
| alignItems | stretch | stretch |
| justifyContent | flex-start | flex-start |
| position | static | relative |

```jsx
import { View, StyleSheet } from 'react-native';

const LayoutExample = () => (
  <View style={styles.container}>
    <View style={[styles.box, { backgroundColor: 'red' }]} />
    <View style={[styles.box, { backgroundColor: 'blue' }]} />
    <View style={[styles.box, { backgroundColor: 'green' }]} />
  </View>
);

const styles = StyleSheet.create({
  container: {
    flex: 1,
    flexDirection: 'row',        // 主轴方向
    justifyContent: 'center',    // 主轴对齐
    alignItems: 'center',        // 交叉轴对齐
    backgroundColor: '#fff',
  },
  box: {
    width: 50,
    height: 50,
    margin: 5,
  },
});
```

### 5.2 响应式设计

```jsx
import { Dimensions, StyleSheet, useWindowDimensions } from 'react-native';

// 方式一：Dimensions API
const { width, height } = Dimensions.get('window');

// 方式二：useWindowDimensions Hook（推荐，响应式更新）
const ResponsiveComponent = () => {
  const { width, height } = useWindowDimensions();
  const isTablet = width >= 768;

  return (
    <View style={{
      width: isTablet ? '50%' : '100%',
      padding: width * 0.05,
    }}>
      <Text>响应式布局</Text>
    </View>
  );
};
```

### 5.3 样式注意事项

- 没有 CSS 继承（`Text` 组件内的 `Text` 除外）
- 使用 `StyleSheet.create()` 优化性能
- 颜色支持 hex、rgb、rgba、命名颜色
- 尺寸单位默认是 dp（设备独立像素）

---

## 6. 导航

### 6.1 React Navigation 安装

```bash
# 安装核心库
npm install @react-navigation/native

# 安装依赖
npx expo install react-native-screens react-native-safe-area-context

# 安装堆栈导航
npm install @react-navigation/native-stack

# 安装底部标签导航
npm install @react-navigation/bottom-tabs
```

### 6.2 堆栈导航示例

```jsx
import React from 'react';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import HomeScreen from './screens/HomeScreen';
import DetailScreen from './screens/DetailScreen';

const Stack = createNativeStackNavigator();

const App = () => {
  return (
    <NavigationContainer>
      <Stack.Navigator
        initialRouteName="Home"
        screenOptions={{
          headerStyle: { backgroundColor: '#f4511e' },
          headerTintColor: '#fff',
          headerTitleStyle: { fontWeight: 'bold' },
        }}
      >
        <Stack.Screen
          name="Home"
          component={HomeScreen}
          options={{ title: '首页' }}
        />
        <Stack.Screen
          name="Detail"
          component={DetailScreen}
          options={{ title: '详情' }}
        />
      </Stack.Navigator>
    </NavigationContainer>
  );
};
```

### 6.3 页面跳转与传参

```jsx
// HomeScreen.js
import { Button } from 'react-native';

const HomeScreen = ({ navigation }) => {
  return (
    <Button
      title="前往详情页"
      onPress={() =>
        navigation.navigate('Detail', {
          itemId: 86,
          otherParam: '传递的参数',
        })
      }
    />
  );
};

// DetailScreen.js
import { Text, View } from 'react-native';

const DetailScreen = ({ route, navigation }) => {
  const { itemId, otherParam } = route.params;

  return (
    <View>
      <Text>itemId: {itemId}</Text>
      <Text>otherParam: {otherParam}</Text>
      <Button
        title="返回"
        onPress={() => navigation.goBack()}
      />
    </View>
  );
};
```

---

## 7. 状态管理

### 7.1 React 内置 Hooks

```jsx
import React, { useState, useEffect, useReducer } from 'react';
import { View, Text, Button } from 'react-native';

const Counter = () => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log(`count 变化了: ${count}`);
  }, [count]);

  return (
    <View>
      <Text>计数: {count}</Text>
      <Button title="增加" onPress={() => setCount(count + 1)} />
      <Button title="减少" onPress={() => setCount(count - 1)} />
    </View>
  );
};
```

### 7.2 Context API（跨组件共享状态）

```jsx
// ThemeContext.js
import React, { createContext, useContext, useState } from 'react';

const ThemeContext = createContext();

export const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState('light');

  const toggleTheme = () => {
    setTheme(theme === 'light' ? 'dark' : 'light');
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

export const useTheme = () => useContext(ThemeContext);

// 使用
const MyComponent = () => {
  const { theme, toggleTheme } = useTheme();
  return (
    <View style={{ backgroundColor: theme === 'dark' ? '#000' : '#fff' }}>
      <Button title="切换主题" onPress={toggleTheme} />
    </View>
  );
};
```

### 7.3 Redux Toolkit（大型项目推荐）

```bash
npm install @reduxjs/toolkit react-redux
```

```jsx
// store.js
import { configureStore, createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => { state.value += 1; },
    decrement: (state) => { state.value -= 1; },
  },
});

export const { increment, decrement } = counterSlice.actions;

export const store = configureStore({
  reducer: {
    counter: counterSlice.reducer,
  },
});

// App.js
import { Provider } from 'react-redux';
import { store } from './store';

export default function App() {
  return (
    <Provider store={store}>
      <MyApp />
    </Provider>
  );
}
```

---

## 8. 网络请求

### 8.1 使用 Fetch API

```jsx
const fetchData = async () => {
  try {
    const response = await fetch('https://api.example.com/data');
    const json = await response.json();
    return json;
  } catch (error) {
    console.error('请求失败:', error);
  }
};
```

### 8.2 使用 Axios

```bash
npm install axios
```

```jsx
import axios from 'axios';

// 创建实例
const api = axios.create({
  baseURL: 'https://api.example.com',
  timeout: 10000,
  headers: { 'Content-Type': 'application/json' },
});

// 请求拦截器
api.interceptors.request.use(
  (config) => {
    // 在请求发送前添加 token
    config.headers.Authorization = `Bearer ${token}`;
    return config;
  },
  (error) => Promise.reject(error)
);

// GET 请求
const getUsers = async () => {
  const { data } = await api.get('/users');
  return data;
};

// POST 请求
const createUser = async (userData) => {
  const { data } = await api.post('/users', userData);
  return data;
};
```

### 8.3 数据持久化

```bash
# AsyncStorage
npx expo install @react-native-async-storage/async-storage
```

```jsx
import AsyncStorage from '@react-native-async-storage/async-storage';

// 存储
const storeData = async (key, value) => {
  try {
    await AsyncStorage.setItem(key, JSON.stringify(value));
  } catch (e) {
    console.error(e);
  }
};

// 读取
const getData = async (key) => {
  try {
    const value = await AsyncStorage.getItem(key);
    return value != null ? JSON.parse(value) : null;
  } catch (e) {
    console.error(e);
    return null;
  }
};

// 删除
const removeData = async (key) => {
  try {
    await AsyncStorage.removeItem(key);
  } catch (e) {
    console.error(e);
  }
};
```

---

## 9. 调试与测试

### 9.1 开发调试

- **打开开发者菜单**: 摇一摇手机或按 `Cmd+D` (iOS) / `Ctrl+M` (Android)
- **热重载**: 保存文件后自动刷新（默认开启）
- **React DevTools**: 浏览器扩展或独立应用
- **Console 日志**: 使用 `console.log`，在 Metro 终端查看

### 9.2 React Native Debugger

```bash
# macOS
brew install --cask react-native-debugger

# 或使用 Chrome DevTools
# 在开发者菜单中选择 "Debug JS Remotely"
```

### 9.3 单元测试（Jest）

```bash
npm install --save-dev jest @testing-library/react-native
```

```jsx
// __tests__/MyComponent.test.js
import React from 'react';
import { render, fireEvent } from '@testing-library/react-native';
import MyComponent from '../MyComponent';

test('点击按钮增加计数', () => {
  const { getByText } = render(<MyComponent />);
  const button = getByText('增加');
  fireEvent.press(button);
  expect(getByText('计数: 1')).toBeTruthy();
});
```

---

## 10. 打包与发布

### 10.1 Android 打包

```bash
# 生成签名密钥
keytool -genkeypair -v -storetype PKCS12 -keystore my-upload-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000

# 编辑 android/gradle.properties 添加签名配置
MYAPP_UPLOAD_STORE_FILE=my-upload-key.keystore
MYAPP_UPLOAD_KEY_ALIAS=my-key-alias
MYAPP_UPLOAD_STORE_PASSWORD=*****
MYAPP_UPLOAD_KEY_PASSWORD=*****

# 生成发布包
cd android
./gradlew assembleRelease   # APK
./gradlew bundleRelease     # AAB（推荐上架 Google Play）
```

### 10.2 iOS 打包（需要 macOS 和 Apple 开发者账号）

1. 在 Xcode 中打开 `ios/MyApp.xcworkspace`
2. 配置签名和 Team
3. 选择 `Product` → `Archive`
4. 使用 Xcode Organizer 上传到 App Store Connect

### 10.3 Expo OTA 更新

```bash
# 安装 EAS CLI
npm install -g eas-cli

# 登录
eas login

# 构建
eas build --platform all
```

---

## 11. 常见问题

### Q1: 启动报错 "SDK location not found"

**解决方法**: 在 `android/local.properties` 中添加：
```
sdk.dir = /Users/USERNAME/Library/Android/sdk
```

### Q2: iOS 打包报错 "Signing for requires a development team"

**解决方法**: 在 Xcode → Signing & Capabilities 中选择你的 Team。

### Q3: Android 模拟器启动很慢

**建议**: 启用硬件加速 (HAXM)，或使用真机调试。

### Q4: FlatList 渲染大量数据卡顿

**优化方法**:
- 使用 `getItemLayout` 优化计算
- 设置 `initialNumToRender`
- 使用 `removeClippedSubviews={true}`
- 避免在 `renderItem` 中创建新函数

### Q5: 样式在不同平台表现不一致

**建议**:
- 使用 `Platform.OS` 做平台判断
- 使用 `Platform.select()` 编写平台特定样式
- 使用 `SafeAreaView` 处理刘海屏

---

## 推荐资源

- [React Native 官方文档](https://reactnative.dev/docs/getting-started)
- [React Navigation 文档](https://reactnavigation.org/)
- [Expo 文档](https://docs.expo.dev/)
- [React Native 社区 Awesome 列表](https://github.com/jondot/awesome-react-native)

---

## 学习路线建议

1. **基础阶段**: 掌握 React 基础 → 学习 RN 核心组件 → 理解 Flexbox 布局
2. **进阶阶段**: 导航库 → 状态管理 → 网络请求 → 原生模块
3. **实战阶段**: 完整项目开发 → 性能优化 → 打包发布
4. **高级阶段**: 原生模块开发 → 架构设计 → 自动化测试
