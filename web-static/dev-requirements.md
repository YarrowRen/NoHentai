# 前端开发说明

接下来使用当前api作为后端，在/Users/boyuren/Documents/self_coding/NoHentai/web文件夹下使用vue创建前端 

## 开发技术选型

前端整体采用 Nuxt 3 作为核心框架，基于 Vue 3 Composition API 与 Vite 构建体系，使用文件系统路由与内置布局机制统一页面结构，通过 SSR / SSG 提升首屏渲染性能与 SEO 表现；状态管理采用 Pinia，数据请求使用内置 $fetch / useFetch 实现同构数据获取；样式层采用 SCSS 或 Tailwind CSS 构建响应式布局，适配 Web 与移动端；UI 组件层根据场景按需引入 Element Plus（PC）与 Vant（移动端）；工程化方面通过 TypeScript 提升类型安全，ESLint + Prettier 规范代码风格，配合 Nuxt Nitro 实现服务端渲染与 API 能力统一，最终形成一套可维护、可扩展、SEO 友好的一体化前端解决方案。

## 后端API接口

后端API接口及说明：/Users/boyuren/Documents/self_coding/NoHentai/API文档.md

## 页面开发需求

所有页面都需要考虑响应式布局保证在PC WEB环境下展示和手机端环境下展示的效果良好

### 主页 

包含高级搜索栏其中有category筛选（"Doujinshi","Manga","Artist CG","Game CG","Western","Non-H","Image Set","Cosplay","Asian Porn","Misc",）和输入栏，以及search button clear button,下方有pagination和筛选结果，每页20个。会展示gallery的基本信息（title,jp_title,tag list,date,...）和cover。

### 详情页

设计合理样式，要在页面上分别展示以下信息
- cover图
- 元数据
- 画廊所有图片缩略图

### 数据统计页

展示所有统计数据


### 配置页

配置EX配置信息以及分页数量等。同时有同步按钮，点击后进行同步