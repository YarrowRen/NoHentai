# NoHentai 静态版本

这是 NoHentai 的纯静态版本，所有数据都从本地 JSON 文件读取，不依赖后端 API。

## 功能特性

### ✅ 已实现功能
- **画廊列表展示** - 分页浏览、搜索、类型过滤
- **数据统计分析** - 图表展示、季度统计、热门标签分析  
- **标签翻译** - 中英文标签对照显示
- **主题切换** - 深色/浅色模式
- **妈妈模式** - 隐私保护模式
- **响应式设计** - 支持移动端浏览

### ❌ 暂不支持功能
- ~~画廊详情页~~ (需要后端 API 支持)
- ~~漫画阅读器~~ (需要图片代理服务)  
- ~~设置页面~~ (需要后端配置管理)
- ~~数据同步~~ (需要 ExHentai 认证)

## 部署方式

### 本地开发
```bash
npm install
npm run dev
```

### 构建静态文件
```bash
npm run build
```
构建后的文件在 `dist/` 目录，可直接部署到任何静态托管服务。

### 数据更新
1. 使用后端同步脚本更新数据：
   ```bash
   python sync_script.py
   ```

2. 复制更新后的数据文件：
   ```bash
   cp app/data/exhentai_favs_metadata.json web-static/public/data/galleries.json
   cp app/data/db.text.json web-static/public/data/translations.json
   ```

3. 重新构建静态文件：
   ```bash
   npm run build
   ```

## 数据文件结构

```
public/data/
├── galleries.json     # 画廊数据 (从 exhentai_favs_metadata.json 复制)
└── translations.json  # 标签翻译 (从 db.text.json 复制)
```

## 技术架构

- **前端框架**: Vue 3 + TypeScript
- **UI 组件**: PrimeVue
- **图表库**: Chart.js  
- **路由**: Vue Router
- **构建工具**: Vite
- **数据来源**: 静态 JSON 文件

## 部署建议

### GitHub Pages
```bash
npm run build
# 将 dist/ 目录内容推送到 gh-pages 分支
```

### Netlify
```bash
# Build command: npm run build  
# Publish directory: dist
```

### Vercel
```bash
# Framework Preset: Vue.js
# Build Command: npm run build
# Output Directory: dist  
```

## 限制说明

1. **数据更新**: 需要手动运行后端同步脚本并更新 JSON 文件
2. **图片显示**: 只能显示 JSON 中的 `thumb` 字段，受 CORS 限制可能无法正常显示
3. **交互功能**: 无法进行画廊详情查看、阅读等需要后端支持的操作

## 优势

- ✅ **零服务器成本** - 纯静态部署  
- ✅ **极快访问速度** - 无 API 请求延迟
- ✅ **离线可用** - 支持 PWA 离线访问
- ✅ **CDN 友好** - 所有资源可缓存
- ✅ **简单维护** - 定期更新 JSON 文件即可