# Komari Aurora

一款基于 Apple HIG（Human Interface Guidelines）设计语言的 [Komari](https://github.com/komari-monitor/komari) 探针主题。

## 预览

| 服务器总览 | 服务器详情 |
|:---:|:---:|
| ![Overview](preview.png) | ![Detail](preview.png) |

> 截图为示例展示，实际效果请以上传后为准。

## 功能特性

### 三页面 SPA 架构

- **服务器总览** — 6 个聚合指标卡片 + 地区筛选 + 搜索框 + 响应式服务器卡片网格
- **服务器卡片快览** — 居中弹窗（max-width 680px），2×2 指标网格 + 三网延迟 ISP 卡片
- **服务器详情页** — 260px 侧边栏（定价 / 硬件 / 系统信息）+ 主内容区（SVG 网络质量折线图 + 资源监控卡片）

### 技术实现

| 特性 | 说明 |
|------|------|
| 零外部依赖 | 单 HTML 文件，所有 CSS/JS 内联 |
| 纯 SVG 图表 | Catmull-Rom 贝塞尔曲线 + linearGradient 渐变填充，不依赖 Chart.js / ECharts |
| WebSocket 实时数据 | 连接 `/api/clients`，指数退避重连（上限 20s） |
| SPA 路由 | `history.pushState` + `popstate`，支持 `/` 和 `/node/:uuid` |
| API 优雅降级 | 3 秒超时 + AbortController，失败返回 null |
| 明暗主题 | 通过 `localStorage appearance` 切换，支持跟随系统 |
| 中英文 i18n | 通过 `localStorage i18nextLng` 切换 |
| SVG 国旗 | 258 个国旗 SVG + 文字回退 |
| 响应式适配 | 桌面 / 平板 / 手机三档断点 |

### 设计规范

严格遵循 Apple HIG 暗色设计语言：

- **色彩系统**：CPU `#007AFF`、RAM `#34C759`、Disk `#FF9500`、Traffic `#AF52DE`、在线 `#34C759`、离线 `#FF3B30`
- **进度条渐变**：CPU 蓝→青、RAM 绿→浅绿、Disk 橙→黄、Traffic 紫→粉紫
- **组件样式**：圆角 12-16px、药丸徽章、毛玻璃顶栏、柔和阴影
- **字体**：SF Pro Display / SF Pro Text / SF Mono 系统字体栈

## 安装

1. 前往 [Releases](../../releases) 下载最新 `komari-theme-aurora-v1.0.0.zip`
2. 进入 Komari 后台管理 → 主题管理
3. 上传 zip 文件并激活

> **注意**：Komari 部署在 Linux 服务器上，请确保 zip 包内路径使用正斜杠 `/`（本项目已正确处理）。

## 主题配置

在 Komari 后台主题设置中可配置以下选项：

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `dataUpdateInterval` | number | 3 | 实时数据刷新间隔（秒） |
| `defaultHistoryHours` | number | 24 | 详情页默认历史时间范围（1/6/12/24） |
| `showOfflineNodes` | switch | true | 是否显示离线节点 |
| `enablePing` | switch | true | 是否启用三网延迟 |
| `pingHours` | number | 24 | Ping 数据时间范围（小时） |
| `cardsPerRow` | number | 3 | 每行卡片数 |
| `footerName` | string | Aurora | 页脚显示名称 |

## 项目结构

```
.
├── komari-theme.json      # 主题配置文件
├── preview.png            # 主题预览图
├── dist/
│   ├── index.html         # 主题主文件（HTML + CSS + JS 单文件）
│   ├── favicon.ico
│   └── images/
│       ├── flags/         # 258 个国家国旗 SVG
│       └── logo/          # 33 个 OS logo SVG
└── .gitignore
```

## 技术栈

- **HTML5** — 语义化结构
- **CSS3** — CSS 变量、Flexbox、Grid、backdrop-filter
- **Vanilla JavaScript** — 无框架依赖，IIFE 模块化
- **SVG** — 纯手绘图表（贝塞尔曲线、渐变填充、sparkline）

## API 接口

本主题使用以下 Komari 公开 API（无需认证）：

| 接口 | 方法 | 说明 |
|------|------|------|
| `/api/nodes` | GET | 获取节点静态信息列表 |
| `/api/public` | GET | 获取站点设置与主题配置 |
| `/api/clients` | WebSocket | 获取实时监控数据 |
| `/api/records/load` | GET | 获取历史负载记录 |
| `/api/records/ping` | GET | 获取三网延迟记录 |
| `/api/task/ping` | GET | 获取 Ping 任务列表 |

> 所有 JSON API 响应均为 `{status, message, data}` 包装格式，主题内部已自动解包。

## 致谢

- [Komari](https://github.com/komari-monitor/komari) — 探针项目
- [komari-theme-emerald](https://github.com/Tokinx/komari-theme-emerald) — 参考主题
- Apple Human Interface Guidelines — 设计灵感

## License

MIT
