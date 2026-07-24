# Hugo Bookmark Directory

一个用 Hugo 构建的现代化书签目录网站，支持分类、标签、搜索等功能。

## 功能特性

- 📚 **多分类系统** - 支持父分类和子分类的层级结构
- 🏷️ **标签系统** - 每个书签可添加多个标签
- 🔍 **实时搜索** - 全站实时标题搜索
- 📱 **响应式设计** - 适配桌面和移动设备
- 🌙 **现代UI** - Bootstrap 5 + 渐变色设计
- 📄 **博客系统** - 内置博客功能
- 🔗 **Favicon 自动获取** - 自动获取网站图标
- 📊 **SEO 友好** - 清晰的 URL 结构

## 技术栈

- **Hugo Extended** - 静态网站生成器
- **Bootstrap 5** - CSS 框架
- **Markdown** - 内容格式

## 快速开始

### 1. 安装 Hugo

```bash
# macOS (Homebrew)
brew install hugo

# Linux (Debian/Ubuntu)
sudo apt install hugo

# 或从官网下载: https://gohugo.io/getting-started/installing/
```

### 2. 克隆项目

```bash
git clone https://github.com/yourusername/hugo-bookmark-blog.git
cd hugo-bookmark-blog
```

### 3. 启动开发服务器

```bash
hugo server -D
```

访问 `http://localhost:1313` 查看网站。

### 4. 构建生产版本

```bash
hugo --minify
```

生成的文件在 `public/` 目录。

## 项目结构

```
├── archetypes/          # 内容模板
├── content/             # 网站内容
│   ├── bookmark/        # 书签内容
│   ├── blog/            # 博客文章
│   ├── bookmarkCategories/ # 分类页面
│   └── blogCategories/  # 博客分类
├── themes/
│   └── hugo-bookmark/   # 主题
│       ├── layouts/     # 模板文件
│       └── static/      # 静态资源
├── config.toml          # 配置文件
└── public/              # 构建输出
```

## 配置说明

编辑 `config.toml` 自定义网站：

```toml
baseURL = "https://yourdomain.com/"
title = "Your Bookmark Directory"
```

### 添加分类

在 `config.toml` 的 `[params.bookmarkCategories]` 部分添加：

```toml
[params.bookmarkCategories.my-category]
  name = "My Category"
  icon = "bi bi-star"
  subcategories = ["Sub Category 1", "Sub Category 2"]
```

### 添加书签

在 `content/bookmark/` 创建 markdown 文件：

```markdown
---
title: "Example Website"
website: "https://example.com"
description: "Website description"
favicon: "https://example.com/favicon.ico"
tags: ["tag1", "tag2"]
bookmarkCategory: "Sub Category Name"
date: 2024-01-01T10:00:00+08:00
lastmod: 2024-01-01T10:00:00+08:00
draft: false
---

Detailed description of the website.
```

## 主题结构

### Layouts

- `index.html` - 首页
- `bookmark/list.html` - 书签列表页
- `bookmark/single.html` - 书签详情页
- `blog/list.html` - 博客列表页
- `blog/single.html` - 博客详情页
- `bookmarkcategories/term.html` - 分类详情页
- `bookmarkcategories/list.html` - 分类总览页
- `tags/term.html` - 标签详情页
- `tags/list.html` - 标签总览页

### Partials

- `header.html` - 头部导航
- `footer.html` - 页脚
- `sidebar-bookmark.html` - 书签侧边栏
- `sidebar-blog.html` - 博客侧边栏
- `breadcrumb.html` - 面包屑导航

## 自定义

### 修改颜色

主题使用 CSS 变量，在 `head.html` 中可以自定义颜色。

### 添加社交媒体链接

在 `config.toml` 的 `[params.social]` 部分配置：

```toml
[params.social]
  twitter = "https://twitter.com/username"
  linkedin = "https://linkedin.com/in/username"
  email = "mailto:your@email.com"
```

## 部署

### GitHub Pages

使用 GitHub Actions 自动部署，参考 `.github/workflows/gh-pages.yml`。

### Netlify

1. 连接 GitHub 仓库
2. 构建命令: `hugo --minify`
3. 发布目录: `public`

### Vercel

1. 连接 GitHub 仓库
2. Vercel 会自动检测 Hugo 项目

## 贡献

欢迎提交 Issue 和 Pull Request！

## License

MIT License - 详见 [LICENSE](LICENSE) 文件。

## 作者

- **Design & Development** - [shopaii.net](https://shopaii.net/)
