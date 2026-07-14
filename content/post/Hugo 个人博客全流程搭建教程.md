---
title: 'Hugo 个人博客全流程搭建教程'

date: 2026-07-13T11:00:00-07:00
lastmod: 2026-07-13T11:00:00-07:00
cover: "images/my-cover.png"
---

# Hugo 个人博客全流程搭建教程

> 基于 **Hugo** + **GitHub Pages** + **hugo-theme-reimu** 主题  
> 部署方式：GitHub Actions 自动构建（推荐）

---

## 1. 环境搭建

### 1.1 Git 下载

1. 前往 [Git 官网](https://git-scm.com/) 下载安装程序
2. 一路点击 **下一步**，默认安装即可
3. 若不懂其含义，尽量不要修改选项

### 1.2 Hugo 下载

1. 前往 [Hugo GitHub Releases](https://github.com/gohugoio/hugo/releases) 页面
2. 选择最新版本的 **Windows Extended** 版本（文件名含 `hugo_extended_withdeploy_xxx_windows-amd64`）
3. 下载后解压到本地目录，例如：`D:\software\hugo\`

> ⚠️ **注意**：必须下载 **Extended** 版本，否则主题 SCSS 编译会报错。

### 1.3 下载主题

1. 前往 [hugo-theme-reimu](https://github.com/D-Sketon/hugo-theme-reimu) 官方页面
2. 选择最新版本，下载 `Source code` 后解压
3. 删除文件名后的版本号，仅保留 `hugo-theme-reimu`

---

## 2. 部署博客

### 2.1 创建博客

进入 Hugo 解压目录，在文件夹地址栏输入 `cmd` 呼出命令行：

```bash
# 创建 Hugo 博客主文件夹
hugo new site blog

# 进入博客目录
cd blog
```

将上一级文件夹中的 `hugo.exe` 复制进 `blog` 中，此时目录结构应为：

```
blog/
├── archetypes/
├── assets/
├── content/
├── data/
├── i18n/
├── layouts/
├── static/
├── themes/
├── hugo.toml
└── hugo.exe          # 手动复制进来
```

启动本地预览：

```bash
hugo server --buildDrafts --disableFastRender
```

出现 `http://localhost:1313` 后，按住 `Ctrl` 点击地址进入浏览器。若显示 **"Page Not Found"**，表示博客服务启动成功。

> 💡 按 `Ctrl + C` 退出预览，否则后续保存文件时浏览器会自动刷新弹窗。

---

### 2.2 初始配置

在 `blog` 内创建配置目录：

```bash
blog/
└── config/
    └── _default/
```

将主题文件复制到对应位置：

| 来源 | 目标 |
|------|------|
| `theme/hugo-theme-reimu/config/_default/params.yml` | `blog/config/_default/params.yml` |
| `theme/hugo-theme-reimu/data/` 内所有文件 | `blog/data/` |
| `theme/hugo-theme-reimu/_example/` 内所有文件 | `blog/content/` |
| `theme/hugo-theme-reimu/assets/` 内所有文件 | `blog/assets/` |
| `theme/hugo-theme-reimu/layouts/` 内所有文件 | `blog/layouts/` |
| `theme/hugo-theme-reimu/static/` 内所有文件 | `blog/static/` |

编辑 `blog/hugo.toml`，添加基础配置：

```toml
baseURL = 'https://你的用户名.github.io/你的仓库名/'
languageCode = 'zh-CN'
title = '你的博客标题'
theme = 'hugo-theme-reimu'

[params]
  author = '你的名字'
  description = '博客描述'
```

再次启动预览：

```bash
hugo server --buildDrafts --disableFastRender
```

此时浏览器应能正常显示主题示例界面，博客本地搭建完成。

---

## 3. 主题设置

### 3.1 多语言 i18n 配置

打开 `blog/config/_default/params.yml`，自定义 `menu` 的 `name` 和 `url`：

```yaml
menu:
  - name: home
    url: ""
    icon: "f015"      # FontAwesome 图标代码
  - name: archives
    url: "archives"
    icon: "f187"
  - name: about
    url: "about"
    icon: "f007"
  - name: friend
    url: "friend"
    icon: "f0c0"
```

打开 `blog/i18n/` 下对应语言的 `.yml` 文件，修改菜单翻译：

```yaml
# zh-cn.yml 示例
home: 首页
about: 关于
archives: 归档
friend: 友链
```

> ⚠️ **关键**：若使用 FontAwesome 图标，必须在 `params.yml` 中设置 `icon_font: false`，否则菜单图标无法显示。

---

### 3.2 核心配置（params.yml）

#### 3.2.1 基础信息

```yaml
author: Dut.
email: 1585641459@qq.com
description: "如果我们不曾相遇..."
banner: "images/wmls.png"        # 首页背景图，放在 static/images/
avatar: "avatar.png"             # 头像，放在 static/avatar/ 下
```

> ⚠️ **头像路径坑**：`avatar` 配置只写文件名，不要加路径。文件必须放在 `static/avatar/avatar.png`，而不是 `static/images/`。

#### 3.2.2 颜色配置（去红变蓝示例）

```yaml
internal_theme:
  light:
    --red-0: '#1890ff'
    --red-1: '#40a9ff'
    --red-2: '#69c0ff'
    --red-3: '#91d5ff'
    --red-4: '#bae7ff'
    --red-5: '#e6f7ff'
    --red-5-5: '#f0fbff'
    --red-6: '#f5fcff'
    --color-red-6-shadow: 'rgba(24, 144, 255, 0.6)'
    --color-red-3-shadow: 'rgba(24, 144, 255, 0.3)'
```

> ⚠️ **YAML 语法坑**：所有 `#` 开头的颜色值**必须加单引号或双引号**，否则 `#` 会被当成注释，导致颜色为空值，页面显示默认红色或 SCSS 编译报错。

#### 3.2.3 文章封面图

在文章 Front-matter 中设置：

```markdown
---
title: '我的第一条博客'
date: 2026-07-13T11:00:00-07:00
cover: "/images/my-cover.png"    # 绝对路径，带 /
---

正文内容...
```

> ⚠️ **路径坑**：GitHub Pages 项目站点（`用户名.github.io/仓库名/`）中，封面图必须使用**绝对路径**（带前导 `/`），Hugo 构建时会根据 `baseURL` 自动拼接完整地址。若用相对路径，在文章子页面中会解析错误导致 404。

---

## 4. 博客部署（GitHub Actions 自动构建）

### 4.1 创建 GitHub 仓库

1. 登录 GitHub，新建仓库
2. 仓库名：**`你的用户名.github.io`**（如 `ghh6323.github.io`）
3. 用户名必须全小写，否则 Pages 无法使用

### 4.2 配置 GitHub Actions

在本地 `blog` 文件夹中创建工作流文件：

```bash
mkdir .github\workflows
```

创建 `.github/workflows/hugo.yml`，粘贴以下内容：

```yaml
name: Deploy Hugo site to Pages

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.164.0
    steps:
      - name: Install Hugo CLI
        run: wget -O /tmp/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb && sudo dpkg -i /tmp/hugo.deb
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5
      - name: Build with Hugo
        env:
          HUGO_ENVIRONMENT: production
          HUGO_ENV: production
        run: hugo --gc --minify --baseURL "${{ steps.pages.outputs.base_url }}/"
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

> ⚠️ **注意**：`HUGO_VERSION` 必须与你本地使用的版本一致。

### 4.3 推送代码

```bash
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/你的用户名/你的用户名.github.io.git
git push -u origin main
```

> 💡 若国内网络不稳定导致推送失败，可尝试：
> - 开启代理后设置 Git 代理：`git config --global http.proxy http://127.0.0.1:7890`
> - 或使用 GitHub Desktop 图形化工具推送

### 4.4 开启 GitHub Pages

1. 进入仓库 → **Settings** → **Pages**
2. **Source** 选择 **GitHub Actions**
3. 保存后，Actions 会自动触发构建
4. 进入 **Actions** 标签页，等待 workflow 变绿（✅）
5. 访问 `https://你的用户名.github.io/你的用户名.github.io/`，部署完成

---

## 5. 常见问题排查

### 5.1 网站显示 404 / README 内容

- 检查 **Settings → Pages → Source** 是否为 **GitHub Actions**
- 检查 `.github/workflows/hugo.yml` 是否存在且语法正确
- 检查 Actions 日志是否有报错（SCSS 编译错误通常是颜色值没加引号）

### 5.2 头像 / 封面图 404

- 头像必须放在 `static/avatar/avatar.png`，配置写 `avatar: "avatar.png"`
- 封面图放在 `static/images/xxx.png`，文章里写 `cover: "/images/xxx.png"`（带 `/`）
- 检查 GitHub 仓库中 `static` 文件夹是否已上传

### 5.3 菜单图标不显示

- 确认 `params.yml` 中 `icon_font: false`
- 确认 `menu.icon` 使用的是 FontAwesome Unicode（如 `"f015"`）

### 5.4 颜色修改不生效

- 删除 `resources/_gen` 缓存文件夹
- 重启 `hugo server --buildDrafts --disableFastRender`
- 检查 YAML 中颜色值是否加了引号

### 5.5 本地能预览，部署后样式错乱

- 检查 `hugo.toml` 中 `baseURL` 是否与 GitHub Pages 地址完全一致（含仓库名后缀）
- 检查 Actions 中 `hugo --minify` 是否成功执行

---

## 6. 后续更新博客

### 6.1 写文章

在 `content/post/` 下新建 `.md` 文件：

```markdown
---
title: "文章标题"
date: 2026-07-14T10:00:00+08:00
draft: false
categories: ["随笔"]
tags: ["hugo", "博客"]
cover: "/images/cover.jpg"
---

正文内容，支持 Markdown 语法...
```

### 6.2 推送更新

```bash
git add .
git commit -m "add new post"
git push origin main
```

推送后 Actions 会自动构建，1~3 分钟后刷新网站即可看到更新。

---

> 🎉 至此，你的 Hugo 个人博客已搭建完成。后续只需专注写作，GitHub Actions 会自动帮你完成部署！
