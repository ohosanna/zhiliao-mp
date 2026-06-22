# 知了糖眼管家 — 开发文档

## 一、项目概述

### 1.1 项目简介

「知了糖眼管家」是一个基于 uni-app 3.x 开发的微信小程序，为糖尿病患者提供智能化的血糖管理与并发症风险管理服务。

### 1.2 技术栈

| 类别 | 技术 | 说明 |
|------|------|------|
| 框架 | uni-app 3.x (Vue 3 + Vite 5) | 跨端开发框架，一套代码可编译到多端 |
| 语言 | TypeScript | 全量 TypeScript，类型安全 |
| UI 组件库 | Wot UI v2 | 轻量美观的 uni-app 组件库 |
| 状态管理 | Pinia | Vue 3 官方推荐状态管理 |
| HTTP 请求 | Alova 3.x | 极致高效的请求工具集，支持 Mock |
| 样式 | UnoCSS + SCSS | 原子化 CSS 引擎 + SCSS 预处理器 |
| 路由 | @wot-ui/router | 基于 vue3 的轻量级 uni-app 路由库 |
| 图表 | uni-echarts + Apache ECharts 6 | 适用于 uni-app 的图表组件 |
| 构建 | Vite 5 + pnpm | 高性能构建工具链 |
| 代码质量 | ESLint + vue-tsc | 代码规范 + 类型检查 |

### 1.3 目录结构

```
zhiliao-mp/
├── src/
│   ├── pages/                  # 页面目录（基于文件的路由）
│   │   ├── welcome/index.vue   # 欢迎页（首页/启动页）
│   │   ├── home/index.vue      # 首页（Tab 1）
│   │   ├── data/               # 数据模块
│   │   │   ├── index.vue       # 数据页（Tab 2）
│   │   │   ├── areas.vue       # 区域数据
│   │   │   └── services.vue    # 服务数据
│   │   ├── remind/             # 提醒模块
│   │   │   ├── index.vue       # 提醒页（Tab 3）
│   │   │   └── detail.vue      # 提醒详情
│   │   ├── user/               # 用户模块
│   │   │   ├── index.vue       # 个人中心（Tab 4）
│   │   │   ├── auth.vue        # 授权登录
│   │   │   └── register.vue    # 注册
│   │   └── about/index.vue     # 关于页面
│   ├── layouts/                # 布局系统
│   │   ├── default.vue         # 默认布局
│   │   └── tabbar.vue          # TabBar 布局（带底部导航）
│   ├── components/             # 全局公共组件（自动注册）
│   │   ├── GlobalToast.vue     # 轻提示组件
│   │   ├── GlobalDialog.vue    # 对话框组件
│   │   ├── GlobalLoading.vue   # 加载指示器组件
│   │   ├── PrivacyPopup.vue    # 隐私协议弹窗
│   │   └── DemoBlock.vue       # 演示区块组件
│   ├── business/               # 业务组件（自动注册）
│   ├── api/                    # 数据请求层
│   │   ├── core/               # Alova 核心配置
│   │   │   ├── instance.ts     # Alova 实例
│   │   │   ├── middleware.ts   # 请求中间件
│   │   │   └── handlers.ts     # 响应处理器
│   │   ├── mock/               # Mock 数据
│   │   │   ├── mockAdapter.ts  # Mock 适配器
│   │   │   ├── modules/        # 各模块 Mock 数据
│   │   │   └── demo.ts         # Mock 演示
│   │   ├── index.ts            # API 导出入口
│   │   ├── apiDefinitions.ts   # API 路由定义（自动生成）
│   │   └── createApis.ts       # API 代理创建（自动生成）
│   ├── store/                  # Pinia 状态管理
│   │   ├── persist.ts          # 持久化插件
│   │   ├── themeStore.ts       # 主题状态
│   │   └── manualThemeStore.ts # 手动主题控制
│   ├── router/index.ts         # 路由配置与守卫
│   ├── composables/            # 组合式函数
│   ├── utils/                  # 工具函数
│   ├── types/                  # 类型声明
│   ├── static/                 # 静态资源
│   ├── subEcharts/             # 图表分包
│   ├── subAsyncEcharts/        # 异步图表分包
│   ├── uni_modules/            # uni-app 模块（mp-html 等）
│   ├── App.vue                 # 应用入口组件
│   ├── App.ku.vue              # uni-ku 根组件
│   └── main.ts                 # 应用入口
├── remote_static               # 远程静态资源，用于同步上传到静态服务器上，避免打包体积过大
├── pages.config.ts             # 页面路由配置
├── manifest.config.ts          # 应用 manifest 配置（自动生成 src/manifest.json）
├── vite.config.ts              # Vite 构建配置
├── uno.config.ts               # UnoCSS 配置
├── tsconfig.json               # TypeScript 配置
├── package.json                # 依赖与脚本
└── README.md                   # 项目说明
```

---

## 二、前期准备

### 2.1 申请小程序账号与获取 AppID

开发微信小程序需要一个有效的 AppID，请按以下步骤操作：

1. 访问 [微信公众平台](https://mp.weixin.qq.com/)，点击「立即注册」
2. 选择账号类型为**「小程序」**
3. 填写邮箱、密码、验证码等注册信息，完成邮箱激活
4. 选择主体类型（个人或企业），填写对应的主体信息并完成验证
5. 注册成功后，登录小程序管理后台
6. 进入「管理」→「成员管理」→「项目成员」，添加小程序开发者
7. 进入「开发」→「开发管理」→「开发设置」，即可看到 **AppID（小程序 ID）**
8. 复制该 AppID，后续需要配置到项目中

> 参考链接：[微信官方文档 - 开始](https://developers.weixin.qq.com/miniprogram/dev/framework/quickstart/getstart.html#申请账号)

### 2.2 安装微信开发者工具

1. 下载 [微信开发者工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html) 并安装
2. 启动后使用微信扫码登录（需要与小程序账号关联的开发者微信号）
3. 后续开发时需要用它来预览和调试小程序

### 2.3 本地开发环境

**Node.js**

项目要求 Node.js >= 20.19.0（或 >= 22.12.0，或 >= 24.0.0）。推荐使用 [nvm](https://github.com/nvm-sh/nvm) 管理 Node.js 版本。

**pnpm**

项目使用 pnpm 作为包管理器：

```bash
npm install -g pnpm
```

**VS Code 推荐插件**

| 插件 | 说明 |
|------|------|
| [Vue - Official](https://marketplace.visualstudio.com/items?itemName=Vue.volar) | Vue 3 语言支持 |
| [uni-app-schemas](https://marketplace.visualstudio.com/items?itemName=uni-helper.uni-app-schemas) | uni-app JSON 配置提示 |
| [Wot UI IntelliSense](https://github.com/wot-ui/wot-ui-intellisense) | Wot UI 组件代码提示 |
| [UnoCSS](https://marketplace.visualstudio.com/items?itemName=antfu.unocss) | UnoCSS 原子类提示 |
| [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) | 代码规范检查 |
| [i18n Ally](https://marketplace.visualstudio.com/items?itemName=Lokalise.i18n-ally) | 国际化支持 |

---

## 三、项目初始化与启动

### 3.1 克隆代码

```bash
git clone <项目仓库地址>
cd zhiliao-mp
```

### 3.2 配置 AppID

打开 `manifest.config.ts`，找到 `mp-weixin.appid`，将之前获取的 AppID 填入：

```ts
'mp-weixin': {
  appid: 'wx你的AppID',  // ← 替换为实际 AppID
  // ...
}
```

> **注意**：`src/manifest.json` 是由 `manifest.config.ts` 自动生成的，**不要直接修改** `src/manifest.json`。任何配置变更都要改 `manifest.config.ts`。

### 3.3 安装依赖

```bash
pnpm install
```

### 3.4 启动开发服务

**微信小程序模式**

```bash
pnpm dev:mp-weixin
```

执行后会在项目根目录生成 `dist/dev/mp-weixin/` 目录，这就是编译后的小程序代码。

**H5 模式**

```bash
pnpm dev:h5
```

启动后在浏览器中预览，热更新即时生效。

### 3.5 在微信开发者工具中预览

1. 打开微信开发者工具，点击「导入项目」
2. 项目目录选择 `dist/dev/mp-weixin/`
3. AppID 填入你的小程序 AppID
4. 点击「导入」，等待编译完成即可预览

> **注意**：每次修改代码后，uniapp 会自动重新编译到 `dist/dev/mp-weixin/`，微信开发者工具会自动检测到变化并刷新。

---

## 四、项目核心架构

### 4.1 路由系统

项目采用**基于文件的路由**方案，由 `@uni-helper/vite-plugin-uni-pages` 插件自动扫描 `src/pages/` 下的 `.vue` 文件生成页面路由。

**路由配置**（`pages.config.ts`）：

```ts
export default defineUniPages({
  pages: [],  // 由插件自动生成，不需要手动填写
  globalStyle: { /* 全局样式配置 */ },
  tabBar: {
    custom: true,   // 使用自定义 TabBar
    list: [
      { pagePath: 'pages/home/index' },
      { pagePath: 'pages/data/index' },
      { pagePath: 'pages/remind/index' },
      { pagePath: 'pages/user/index' },
    ],
  },
})
```

**首页设置**：在 `vite.config.ts` 的 `UniHelperPages` 插件中配置 `homePage: 'pages/welcome/index'`。

**路由守卫**：`src/router/index.ts` 中通过 `@wot-ui/router` 的 `beforeEach` / `afterEach` 实现导航守卫，可在其中做登录校验、页面跳转日志等。

### 4.2 布局系统

布局文件放在 `src/layouts/` 下，由 `@uni-helper/vite-plugin-uni-layouts` 插件管理：

- **`default.vue`** — 默认布局，用于非 TabBar 页面
- **`tabbar.vue`** — 带自定义底部导航的布局，用于四个主页面

> 页面文件命名约定：页面文件名即为布局名。例如 `pages/home/index.vue` 会自动匹配 `tabbar` 布局（因为它在 TabBar 列表中），而 `pages/welcome/index.vue` 则使用 `default` 布局。

### 4.3 状态管理（Pinia）

使用 Pinia 管理全局状态，并自带持久化插件。

**持久化插件**（`src/store/persist.ts`）：

自动将 Pinia store 的状态持久化到 `uni.setStorageSync`，刷新/重启后自动恢复。通过排除列表 `['temp']` 可跳过不需要持久化的 store。

**使用方式**：

```ts
// src/store/xxxStore.ts
export const useXxxStore = defineStore('xxx', () => {
  const count = ref(0)
  return { count }
})
// 默认自动持久化，无需额外配置
```

**现有 Store**：

| Store | 文件 | 说明 |
|-------|------|------|
| `themeStore` | `src/store/themeStore.ts` | 主题状态 |
| `manualThemeStore` | `src/store/manualThemeStore.ts` | 手动主题控制 |

### 4.4 请求层（Alova）

项目使用 [Alova](https://alova.js.org/zh-CN/) 作为 HTTP 请求库，基于 `@alova/adapter-uniapp` 适配 uniapp。

**Alova 实例**（`src/api/core/instance.ts`）：

```ts
export const alovaInstance = createAlova({
  baseURL: import.meta.env.VITE_API_BASE_URL || 'https://petstore3.swagger.io/api/v3',
  ...AdapterUniapp({ mockRequest: mockAdapter }),
  statesHook: vueHook,
  beforeRequest: (method) => { /* 统一添加请求头、防缓存等 */ },
  responded: {
    onSuccess: handleAlovaResponse,
    onError: handleAlovaError,
  },
  timeout: 60000,
  cacheFor: null,  // 全局关闭请求缓存
})
```

**请求中间件**（`src/api/core/middleware.ts`）：

提供两种内置中间件：

- `createDelayLoadingMiddleware(delay)` — 延迟显示 loading 状态，防止快速请求闪动
- `createGlobalLoadingMiddleware(options)` — 全局加载指示器，支持自定义延迟时间和文字

**请求示例**：

```ts
import Apis from '@/api'

// GET 请求
const { data, loading, error } = useRequest(
  Apis.pet.findPetsByStatus({ params: { status: 'available' } })
)

// POST 请求
const { send } = useRequest(
  Apis.pet.addPet({ data: { name: 'Buddy', status: 'available' } })
)
```

### 4.5 组件体系

**自动注册**：`@uni-helper/vite-plugin-uni-components` 插件自动扫描并全局注册 `src/components/` 和 `src/business/` 下的组件，页面中无需手动 `import` 即可直接使用。

**Wot UI 组件库**：通过 `WotResolver()` 自动按需加载，使用示例：

```html
<wd-button type="primary" variant="plain">提交</wd-button>
<wd-input v-model="value" placeholder="请输入" />
<wd-cell title="标题" :value="value" />
<wd-popup v-model:show="show" position="bottom" custom-style="height: 40%" />
```

**公共组件**：

| 组件 | 说明 |
|------|------|
| `GlobalToast` | 轻提示，通过 `useGlobalToast()` 调用 |
| `GlobalDialog` | 对话框/确认框，通过 `useGlobalDialog()` 调用 |
| `GlobalLoading` | 全屏加载指示器，通过 `useGlobalLoading()` 调用 |
| `PrivacyPopup` | 微信小程序隐私协议弹窗 |
| `DemoBlock` | 演示区块容器组件 |

### 4.6 样式方案

**UnoCSS** — 原子化 CSS，直接在模板中写工具类：

```html
<view class="flex items-center justify-between p-4 bg-white rounded-lg">
  <text class="text-base text-gray-700">标题</text>
  <text class="text-sm text-blue-500">详情 ></text>
</view>
```

**UnoCSS 预设**：
- `@uni-helper/unocss-preset-uni` — 针对 uni-app 的 UnoCSS 预设
- `@wot-ui/unocss-preset` — Wot UI 主题变量预设
- `@unocss/preset-icons` — 图标预设（carbon、healthicons 等图标集）
- `transformerDirectives()` — 支持 `@apply` 指令
- `transformerVariantGroup()` — 支持变体分组 `flex items-center` 简写为 `flex-(items-center)`

**SCSS** — 复杂样式使用 SCSS：

```html
<template>
  <view class="order-page">
    ...
  </view>
</template>

<style lang="scss">
.order-page {
  // 样式写在根 class 下，避免泄漏到其他页面
  .card {
    background: #fff;
    border-radius: 12rpx;
  }
}
</style>
```

**Wot UI 主题变量**：通过 `theme.json` 配置自定义主题色等变量，在 `uni.scss` 中导入 Wot UI 主题。

---

## 五、页面开发指南

### 5.1 新建页面

在 `src/pages/` 下创建目录和 `.vue` 文件即可自动注册路由，无需手动配置：

```bash
mkdir -p src/pages/order
touch src/pages/order/index.vue
```

页面文件示例：

```vue
<script setup lang="ts">
// 页面逻辑
const title = ref('订单页面')

onLoad((options) => {
  console.log('页面加载，参数:', options)
})
</script>

<template>
  <view class="order-page">
    <wd-navbar :title="title" fixed safeAreaInsetTop />
    <view class="p-4">
      <!-- 页面内容 -->
    </view>
  </view>
</template>

<style lang="scss">
.order-page {
  min-height: 100vh;
  background: #f5f5f5;
}
</style>
```

### 5.2 路由注册

页面创建后，路由自动生效，无需手动修改配置。

**TabBar 页面**：如需添加新的 TabBar 页，在 `pages.config.ts` 的 `tabBar.list` 中添加对应 `pagePath`。

**分包（subPackages）**：将非首页、非 TabBar 的页面放到分包中，减小主包体积。在 `vite.config.ts` 中 `UniHelperPages` 插件的 `subPackages` 数组中添加分包路径：

```ts
UniHelperPages({
  subPackages: [
    'src/subPages',       // 普通分包
    'src/subEcharts',     // 图表分包
    'src/subAsyncEcharts', // 异步图表分包
  ],
})
```

现有分包：
- `subEcharts/echarts/` — 图表页面
- `subAsyncEcharts/asyncEcharts/` — 异步加载图表页面

### 5.3 页面生命周期

| 生命周期 | 说明 |
|---------|------|
| `onLoad(options)` | 页面加载时触发，可获取传入参数 |
| `onShow()` | 页面显示/切入前台时触发 |
| `onReady()` | 页面初次渲染完成时触发 |
| `onHide()` | 页面隐藏/切入后台时触发 |
| `onUnload()` | 页面卸载时触发 |
| `onPullDownRefresh()` | 下拉刷新时触发 |
| `onReachBottom()` | 上拉触底时触发 |
| `onShareAppMessage()` | 点击右上角转发时触发 |

> 这些生命周期函数在 uniapp 中是全局可用的，无需 import。

### 5.4 导航与传参

使用 `@wot-ui/router` 进行页面跳转：

```ts
const router = useRouter()
const route = useRoute()

// 跳转
router.push('/pages/order/index')

// 带参数跳转
router.push('/pages/order/detail', { id: '123', type: 'test' })

// 获取参数
onLoad((options) => {
  console.log(options.id, options.type)
})

// 返回上一页
router.back()
```

---

## 六、组件开发

### 6.1 组件自动注册

`@uni-helper/vite-plugin-uni-components` 插件会自动扫描并全局注册以下目录中的组件：

- `src/components/` — 全局公共组件
- `src/business/` — 业务组件

组件命名采用文件名（含目录层级），使用方式：

```vue
<!-- 文件 src/components/CustomCard.vue，使用时直接写 -->
<CustomCard title="标题" />

<!-- 目录下组件 src/components/order/StatusBadge.vue -->
<OrderStatusBadge status="success" />
```

### 6.2 Wot UI 组件使用规范

Wot UI v2 组件通过自动按需加载引入，使用前建议查阅文档确认 API：

```bash
npx wot info <组件名>
```

常见组件示例：

```vue
<!-- 按钮 -->
<wd-button type="primary" variant="plain">确认</wd-button>
<wd-button type="danger" size="small" loading>删除</wd-button>

<!-- 输入框 -->
<wd-input v-model="phone" placeholder="请输入手机号" />

<!-- 弹窗 -->
<wd-popup v-model:show="showPopup" position="bottom" custom-style="height: 50%">
  <view class="p-4">弹窗内容</view>
</wd-popup>

<!-- 单元格 -->
<wd-cell title="姓名" :value="name" is-link @click="handleClick" />

<!-- 间距 -->
<wd-gap height="16" />

<!-- 图片 -->
<wd-img :src="imageUrl" width="100%" height="200" />

<!-- Toast 提示（通过组合式函数调用） -->
const { show: showToast } = useGlobalToast()
showToast('操作成功')

<!-- 对话框 -->
const { confirm } = useGlobalDialog()
confirm({ title: '提示', msg: '确定要删除吗？' })
```

### 6.3 公共组件一览

| 组件 | 文件路径 | 说明 |
|------|---------|------|
| `GlobalToast` | `src/components/GlobalToast.vue` | 轻提示，通过 `useGlobalToast()` 调用 |
| `GlobalDialog` | `src/components/GlobalDialog.vue` | 确认/提示对话框，通过 `useGlobalDialog()` 调用 |
| `GlobalLoading` | `src/components/GlobalLoading.vue` | 全屏加载，通过 `useGlobalLoading()` 调用 |
| `PrivacyPopup` | `src/components/PrivacyPopup.vue` | 微信小程序隐私协议授权弹窗 |
|                 |                                    |                                                |

---

## 七、API 与数据请求

### 7.1 Alova 实例配置

Alova 实例由 `src/api/core/instance.ts` 创建，核心配置：

- **`baseURL`**：通过环境变量 `VITE_API_BASE_URL` 配置，开发环境可用 Mock
- **请求适配器**：使用 `@alova/adapter-uniapp`，原生支持 uniapp 请求
- **Mock 适配**：在 `AdapterUniapp` 中传入 `mockRequest: mockAdapter`
- **请求拦截**：自动为 GET 请求添加时间戳防缓存，为 POST/PUT/PATCH 设置 JSON Content-Type
- **响应拦截**：通过 `handleAlovaResponse` 和 `handleAlovaError` 统一处理返回数据和错误
- **缓存策略**：默认关闭全局缓存（`cacheFor: null`）

### 7.2 alova.config.ts — 自动代码生成

项目根目录下的 `alova.config.ts` 是 [Alova Wormhole](https://alova.js.org/tutorial/getting-started/extension-integration) 的配置文件，用于根据 Swagger/OpenAPI 文档自动生成 API 代码。

```ts
// alova.config.ts
export default <Config>{
  generator: [
    {
      input: 'https://petstore3.swagger.io/api/v3/openapi.json',  // OpenAPI 文档地址
      platform: 'swagger',                // 文档格式（目前仅支持 swagger）
      output: 'src/api',                  // 生成代码输出目录
      version: 3,                         // 生成 API 版本（2 或 3）
      type: 'typescript',                 // 生成 TypeScript 代码
      global: 'Apis',                     // 全局 API 对象名
      handleApi: (apiDescriptor) => {     // 过滤/转换生成的 API
        if (apiDescriptor.deprecated) return undefined  // 跳过废弃接口
        return apiDescriptor
      },
    },
  ],
  autoUpdate: {
    launchEditor: true,                   // 编辑器启动时自动更新
    interval: 5 * 60 * 1000,              // 每 5 分钟检查更新
  },
}
```

**使用方式**：

1. 将 `input` 地址替换为你的后端 Swagger/OpenAPI 文档地址
2. 运行 `pnpm alova-gen`（对应 `package.json` 中的 `alova gen -f` 命令），自动生成 `src/api/apiDefinitions.ts`、`src/api/createApis.ts` 以及对应的类型声明
3. 生成的 API 通过 `import Apis from '@/api'` 即可使用

### 7.3 API 定义方式

`alova-gen` 命令会生成 `src/api/apiDefinitions.ts`，格式为 `模块.接口名: [方法, 路径]`：

```ts
// src/api/apiDefinitions.ts（自动生成，不要手动修改）
export default {
  'pet.findPetsByStatus': ['GET', '/pet/findByStatus'],
  'pet.getPetById':        ['GET', '/pet/{petId}'],
  'pet.addPet':           ['POST', '/pet'],
  'user.loginUser':       ['GET', '/user/login'],
  // ...
}
```

如果后端接口没有 Swagger 文档或需要手动添加接口，也可以直接编辑此文件。

### 7.3 发起请求

```ts
import Apis from '@/api'

// 方式一：响应式（组合式 API）
const {
  data,       // 响应数据（ref）
  loading,    // 加载状态（ref）
  error,      // 错误信息（ref）
  send,       // 手动触发请求
  onSuccess,  // 成功回调
  onError,    // 错误回调
} = useRequest(Apis.pet.findPetsByStatus({
  params: { status: 'available' },
}))

// 方式二：手动调用（Promise）
async function submit() {
  try {
    const result = await Apis.pet.addPet({
      data: { name: 'Buddy', status: 'available' },
    }).send()
    console.log('创建成功:', result)
  } catch (error) {
    console.error('创建失败:', error)
  }
}

// 带路径参数
const pet = await Apis.pet.getPetById({
  pathParams: { petId: 123 },
}).send()

// 带自定义请求头
const result = await Apis.pet.deletePet({
  pathParams: { petId: 123 },
  headers: { api_key: 'special-key' },
}).send()
```

### 7.4 使用中间件

```ts
import { createGlobalLoadingMiddleware, createDelayLoadingMiddleware } from '@/api/core/middleware'

// 延迟 loading（默认 300ms 延迟）
const { send } = useRequest(method, {
  middleware: createDelayLoadingMiddleware(300),
})

// 全局 loading
const { send: submit } = useRequest(method, {
  middleware: createGlobalLoadingMiddleware({
    delay: 500,
    loadingText: '正在提交...',
  }),
})
```

### 7.5 Mock 模式

Mock 数据在 `src/api/mock/modules/` 下按模块组织。

**启用 Mock**：当前 Mock 始终启用（`enable: true`），可根据环境变量切换。Mock 请求会模拟 200–600ms 的网络延迟。

**模拟网络延迟示例**（`mockAdapter.ts`）：

```ts
const mockAdapter = createAlovaMockAdapter(allMocks, {
  enable: true,
  delay: Math.random() * 400 + 200,  // 200-600ms
  mockRequestLogger: import.meta.env.MODE === 'development',
})
```

**添加新 Mock 模块**：

```ts
// src/api/mock/modules/xxx.ts
import { createMockAdapter } from '@alova/mock'

export default createMockAdapter([
  {
    url: '/api/xxx',
    method: 'GET',
    response: () => {
      return { code: 0, data: [...] }
    },
  },
])
```

然后在 `mockAdapter.ts` 中导入并合并。

---

## 八、状态管理（Pinia）

### 8.1 Store 定义规范

所有 Store 放在 `src/store/` 目录下，使用 `defineStore` 定义：

```ts
// src/store/counterStore.ts
import { defineStore } from 'pinia'

export const useCounterStore = defineStore('counter', () => {
  const count = ref(0)

  function increment() {
    count.value++
  }

  return { count, increment }
})
```

### 8.2 持久化

Store 默认自动持久化到 `uni.setStorageSync`，页面刷新或小程序重启后自动恢复状态。

如需跳过持久化，将 store 的 id 加入 `persist.ts` 中的排除列表：

```ts
// src/store/persist.ts
persist(context, ['temp'])  // 'temp' 开头的 store 不持久化
```

### 8.3 使用 Store

```ts
// 在页面或组件中
const counterStore = useCounterStore()
console.log(counterStore.count)
counterStore.increment()
```

---

## 九、图表（uni-echarts）

### 9.1 图表组件注册

图表组件通过 `WotResolver` 和 `UniEchartsResolver` 自动按需加载，配置在 `vite.config.ts` 中：

```ts
UniHelperComponents({
  resolvers: [WotResolver(), UniEchartsResolver()],
})
```

### 9.2 支持的图表类型

图表组件放在 `src/subEcharts/echarts/components/` 下：

| 组件 | 说明 |
|------|------|
| `LineChart` | 折线图 |
| `BarChart` | 柱状图 |
| `PieChart` | 饼图 |
| `DonutChart` | 环形图 |
| `FunnelChart` | 漏斗图 |
| `RadarChart` | 雷达图 |
| `ScatterChart` | 散点图 |
| `GaugeChart` | 仪表盘 |
| `LiquidFillChart` | 水球图 |
| `StackedBarChart` | 堆叠柱状图 |
| `MiniLineChart` | 迷你折线图 |

### 9.3 图表页面开发

图表页面放在 `src/subEcharts/echarts/index.vue`（分包加载）：

```vue
<script setup lang="ts">
import { useEcharts } from 'uni-echarts'

const chartOptions = ref({
  // ECharts 配置项
  xAxis: { type: 'category', data: ['Mon', 'Tue', 'Wed'] },
  yAxis: { type: 'value' },
  series: [{ type: 'line', data: [120, 200, 150] }],
})
</script>

<template>
  <view class="echarts-page">
    <!-- 直接使用图表组件 -->
    <LineChart :options="chartOptions" height="300" />
  </view>
</template>
```

---

## 十、构建与发布

### 10.1 生产构建

```bash
# 构建微信小程序
pnpm build:mp-weixin
```

构建产物输出到 `dist/build/mp-weixin/` 目录。

### 10.2 上传代码

1. 打开微信开发者工具
2. 导入 `dist/build/mp-weixin/` 目录
3. 点击工具栏「上传」按钮
4. 填写版本号和备注信息

### 10.3 提交审核与发布

1. 登录 [微信公众平台](https://mp.weixin.qq.com/)
2. 进入「管理」→「版本管理」
3. 在「开发版本」中找到刚上传的版本，点击「提交审核」
4. 填写审核信息（功能描述等）
5. 审核通过后，点击「发布」

### 10.4 版本号管理

项目使用 `standard-version` 管理版本号：

```bash
# 大版本（breaking changes）
pnpm release-major

# 次版本（新功能）
pnpm release-minor

# 补丁（bug 修复）
pnpm release-patch
```

---

## 十一、代码质量与规范

### 11.1 ESLint

项目使用 `@uni-helper/eslint-config` 统一代码风格。在提交时自动运行：

```bash
# 手动检查
pnpm exec eslint src/

# 提交时自动修复（lint-staged）
```

### 11.2 TypeScript 类型检查

```bash
pnpm type-check
```

TSConfig 基于 `@vue/tsconfig`，路径别名 `@/` 映射到 `src/`。

**类型声明文件**：

| 文件 | 说明 |
|------|------|
| `src/types/components.d.ts` | 组件类型声明（自动生成） |
| `src/types/auto-imports.d.ts` | 自动导入类型声明（自动生成） |
| `src/types/uni-pages.d.ts` | 页面路由类型（自动生成） |
| `src/env.d.ts` | Vite 环境变量类型 |
| `src/types/shims.d.ts` | uni-app 类型补充 |

### 11.3 Git 提交规范

提交信息使用中文，简洁描述变更内容：

```bash
git commit -m "feat: 新增血糖记录页面"

# 常见前缀
# feat: 新功能
# fix: 修复 bug
# chore: 杂项（构建、工具等）
# refactor: 重构
# docs: 文档
# style: 样式调整
```

---

## 十二、常见问题与注意事项

### 12.1 manifest.config.ts 规则

`src/manifest.json` 由 `manifest.config.ts` 通过 `@uni-helper/vite-plugin-uni-manifest` 插件自动生成。**不要直接修改** `src/manifest.json`，所有配置变更都改 `manifest.config.ts`。

常见需要配置的字段：
- `mp-weixin.appid`：小程序 AppID
- `name`：小程序名称
- `versionName` / `versionCode`：版本号
- `mp-weixin.darkmode`：暗色模式开关

### 12.2 Wot UI 组件常见坑点

- **`wd-popup` 的位置属性**：使用 `position` 而不是 `placement`。样式用 `custom-style` 而不是 `:style`。
- **`wd-button` 的朴素模式**：使用 `variant="plain"` 而不是布尔 `plain` 属性。
- 使用不熟悉的组件时，先用 `npx wot info <组件名>` 查阅文档确认 props 名。
- Wot UI v2 与 v1 的 API 可能有所不同，注意版本对应。

### 12.3 分包加载

微信小程序主包不能超过 2MB，建议将非首屏页面放入分包：

- `subEcharts/echarts/` — 图表页面分包
- `subAsyncEcharts/asyncEcharts/` — 异步加载图表分包
- 后续新增的非核心页面可以新建 `src/subPages/` 目录，并在 `vite.config.ts` 中配置

### 12.4 暗色主题相关

项目在 `manifest.config.ts` 中已关闭微信小程序和 H5 的暗色模式：

```ts
'mp-weixin': { darkmode: false },
'h5': { darkmode: false },
```

如需开启，将 `darkmode` 改为 `true` 并添加 `themeLocation` 字段。
