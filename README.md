## Welcome!

这个是 25 级计算机应用 1 班的班级博客\~ 使用 Hugo 静态站点生成器和 Stack 主题搭建，部署于 Vercel，记录班级学习与生活的点滴 📚

## 🌐 在线访问

- 博客地址：<https://blog.hjx-25pc1.xyz/>

- 班级主站：<https://hjx-25pc1.xyz>

## ✨ 特性

- 🎨 **Stack 主题**：简洁美观的卡片式布局，支持暗色模式

- 🔍 **全站搜索**：内置搜索功能，快速定位文章

- 🗂️ **归档与分类**：按时间归档、分类和标签云组织内容

- 💬 **Waline 评论**：集成 Waline 评论系统，支持表情互动与访问统计

- 📱 **响应式设计**：完美适配桌面端和移动端

- 🚀 **Vercel 部署**：推送自动构建，极速全球访问

- 📄 **短链接风格**：文章永久链接使用 `/p/:slug/` 简洁格式

- 🔎 **SEO 优化**：自动生成 `sitemap.xml`、`robots.txt`，输出 Open Graph / Twitter Card / JSON-LD 结构化数据（WebSite + BlogPosting），可逐页覆写标题、描述、关键词、封面图等

## 🛠️ 技术栈

| 类别      | 技术                                                               |
| ------- | ---------------------------------------------------------------- |
| 静态站点生成器 | Hugo 0.165.0 (Extended)                                          |
| 主题      | [hugo-theme-stack](https://github.com/CaiJimmy/hugo-theme-stack) |
| 评论系统    | Waline                                                           |
| 部署平台    | Vercel                                                           |
| CI/CD   | GitHub Actions                                                   |
| 许可证     | MIT                                                              |

## 📁 项目结构

```
class-blog/
├── .github/workflows/    # GitHub Actions CI 配置
├── archetypes/           # 文章原型模板（含 SEO 字段）
├── assets/               # 静态资源（favicon 等）
├── content/              # 内容目录
│   ├── page/             # 独立页面（归档、搜索）
│   └── post/             # 博客文章
├── layouts/              # 主题覆盖（含 SEO partial）
│   ├── 404.html          # 自定义 404 页
│   └── partials/head/    # 覆盖主题的 head meta/JSON-LD
├── static/               # 直接复制到站点的静态文件
│   └── robots.txt        # 搜索引擎抓取规则
├── themes/               # Hugo 主题（Stack 主题作为 submodule）
├── hugo.toml             # Hugo 站点配置（含 SEO 默认值）
├── vercel.json           # Vercel 部署配置
├── .gitmodules           # Git 子模块配置
└── LICENSE               # MIT 许可证
```

## 🚀 本地开发

### 环境要求

- **Hugo Extended** >= 0.165.0（需支持 SCSS 编译）

- **Git**（用于初始化子模块）

### 安装步骤

1. **克隆仓库并初始化子模块**

```bash
git clone <repo-url>
cd class-blog
git submodule update --init --recursive
```

2. **本地构建 & 启动本地开发服务器（验证）**

```bash
hugo --gc --minify # 构建
hugo server # 启动本地服务器
```

构建产物输出至 `public/` 目录。

## 📝 写作指南

### 创建新文章

使用 Hugo 提供的 archetype 快速创建文章：

```bash
hugo new post/你的文章标题.md
```

### Front Matter 示例

```toml
+++
title = '文章标题'
date = '2026-09-01T10:00:00+08:00'
draft = false
categories = ['分类名称']
tags = ['标签1', '标签2']
+++

这里写文章正文...
```

### 支持的内容格式

- 标准 Markdown 语法

- 代码高亮（支持行号）

- 数学公式（LaTeX）

- Mermaid 图表

- 图片画廊

- 自定义 Shortcodes（Bilibili、YouTube 视频等）

## 🔎 SEO 支持

站点已内置 SEO 能力，开箱即用：

| 自动输出 | 说明 |
| --- | --- |
| `sitemap.xml` | Hugo 内置生成，配置位于 `hugo.toml` 的 `[sitemap]` 块 |
| `robots.txt` | 位于 `static/robots.txt`，已指向 `sitemap.xml` |
| `index.xml` (RSS) | 主页 + 各 section 启用 RSS 输出（`[outputs]` 配置） |
| 自定义 404 页 | `layouts/404.html`，返回首页引导，避免硬 404 |
| Meta 标签 | description / keywords / author / robots / canonical |
| Open Graph | og:type / title / description / url / image / locale；文章页额外输出 article:\* |
| Twitter Card | summary / summary_large_image 自动按封面图选择 |
| JSON-LD | WebSite（所有页）+ BlogPosting（文章页）结构化数据 |

### 在文章中覆写 SEO 字段

在 `content/post/*.md` 的 front matter 中可逐页覆写：

```toml
+++
title = '我的文章'
description = '这篇文章讲了什么……'  # 不填则自动取摘要
keywords = ['关键词1', '关键词2']
author = '你的名字'
image = '/images/cover.png'          # 相对站点根的路径
robots = 'index, follow'             # 设为 'noindex' 可阻止收录
+++
```

`tags` 和 `categories` 会自动作为 `keywords` 与 `article:tag` / `article:section` 输出。

### 在站点级别修改默认值

编辑 `hugo.toml` 中 `[params]` 块下的 `description` / `keywords` / `author` / `image` / `robots` 即可，所有页面会以此为默认值。

### 自定义主题 meta 时请注意

`layouts/partials/head/meta.html` 覆盖了 Stack 主题的同名 partial，**不要**在主题目录（`themes/hugo-theme-stack/`）里修改它——直接修改项目内的 `layouts/` 即可。

## ☁️ 部署说明

### Vercel 自动部署

项目已配置 `vercel.json`，连接 Vercel 后推送至仓库即可自动部署。关键配置：

- **构建命令**：`hugo --gc --minify`

- **输出目录**：`public`

- **Hugo 版本**：0.165.0 (Extended)

- **安装命令**：`git submodule update --init --recursive`（拉取主题）

### GitHub Actions CI

推送到仓库或提交 PR 时，会自动触发构建检查，确保站点能正常编译。

## ⚙️ 配置说明

### 站点基础配置（hugo.toml）

- `baseURL`：站点根 URL

- `title`：站点标题

- `copyright`：页脚版权文字

- `params.sidebar.subtitle`：侧边栏副标题

- `params.footer.since`：页脚起始年份

### 评论系统（Waline）

在 `hugo.toml` 中配置：

```toml
[params.comments]
    enabled = true
    provider = "waline"
    [params.comments.waline]
        serverURL = "https://waline.mantoujun-lab.com"
```

## 🤝 贡献

欢迎班级同学投稿或完善项目！提交 PR 前请确保本地构建通过 (`hugo --gc --minify`)。

你可以扫描下方的微信赞赏码，来支持项目的开发与维护 ☕

<img src="./assets/funding_wechat.png" alt="微信赞赏码" width="240" />

## 📄 许可证

本项目基于 [MIT License](./LICENSE) 开源。
