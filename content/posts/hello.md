+++
date = "2025-07-11T15:25:51+08:00"
draft = false
title = "Hugo 网站搭建入门"
+++

本文详细讲解如何在 macOS 环境下，从零开始搭建 Hugo 静态网站，不依赖 Homebrew，包含 Git 主题添加、配置修改、内容创建等关键操作，帮助你快速搭建属于自己的高性能博客。

---

## 一、准备工作：下载安装 Hugo 与 Git

### 1.1 手动下载安装 Hugo

1. 打开 [Hugo 官方 Releases 页面](https://github.com/gohugoio/hugo/releases)。
2. 下载对应 macOS 的扩展版（Extended），如：
   `hugo_extended_0.148.0_darwin-universal.tar.gz`。
3. 在终端执行以下命令：
   
   ```bash
   cd ~/Downloads
   tar -zxvf hugo_extended_0.148.0_darwin-universal.tar.gz
   sudo mv hugo /usr/local/bin/
   chmod +x /usr/local/bin/hugo
   ```

4. 验证安装：
   ```bash
   hugo version
   ```

确认输出包含 `extended` 字样。

---

### 1.2 安装并确认 Git

macOS 默认自带 Git，如果没有，可按提示安装 Xcode Command Line Tools：

```bash
git --version
```

确保能正常显示 Git 版本号。

---

## 二、创建 Hugo 项目

### 2.1 创建新网站

```bash
hugo new site www-blog
```

> 其中 `www-blog` 是新建的文件夹名称，可以随意取名，不影响博客标题或内容，只是项目根目录名称。

### 2.2 进入项目目录

```bash
cd www-blog
```

---

## 三、添加主题

### 3.1 通过 Git 添加官方主题（以 Ananke 为例）

```bash
git init
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
```

> **提示**：如果遇到代理或网络问题，建议手动下载 ZIP 并解压到 `themes/ananke` 文件夹。

### 3.2 修改配置文件启用主题

编辑项目根目录下的 `hugo.toml`，添加或修改如下内容：

```toml
baseURL = "http://example.org/"
languageCode = "zh-CN"
title = "我的 Hugo 博客"
theme = "ananke"
```

---

## 四、创建内容文章

### 4.1 新建一篇 Markdown 文章

```bash
hugo new posts/hello.md
```

### 4.2 编辑文章内容

打开 `content/posts/hello.md`，将 `draft: true` 改为 `draft: false`，示例内容如下：

```yaml
---
title: "Hello Hugo"
date: 2025-07-11T14:00:00+08:00
draft: false
---

这是我使用 Hugo 创建的第一篇博客文章。  
欢迎阅读并分享！
```

---

## 五、本地预览网站

启动 Hugo 本地服务器：

```bash
hugo server -D
```

然后打开浏览器访问：

```
http://localhost:1313
```

你会看到主题渲染的博客首页和你的文章。

---

## 六、生成静态站点文件

退出服务器后，执行：

```bash
hugo
```

静态页面生成在项目根目录的 `public/` 文件夹。

---

## 七、部署到 GitHub Pages

### 7.1 新建 GitHub 仓库（假设名为 `www-blog`）

### 7.2 推送 `public` 文件夹内容到 `gh-pages` 分支

```bash
cd public
git init
git remote add origin https://github.com/你的用户名/www-blog.git
git checkout -b gh-pages
git add .
git commit -m "首次部署 Hugo 站点"
git push -f origin gh-pages
```

### 7.3 在 GitHub 仓库设置页面开启 GitHub Pages

* 进入 `Settings` → `Pages`
* 选择 `gh-pages` 分支作为发布源
* 保存后等待几分钟即可访问：

```
https://你的用户名.github.io/www-blog/
```

---

## 八、总结说明

* **`hugo new site <文件夹名>` 中的文件夹名只是项目目录名**，不影响博客标题或 URL，只是你电脑里的文件夹名称。
* **博客标题和网站配置在 `hugo.toml` 中设置**，你可以随时修改。
* **主题通过 Git 子模块添加**，也可用手动下载主题 ZIP 包的方式。
* **写文章时注意将 `draft` 设置为 `false`，否则文章不会发布**。
* **本地用 `hugo server -D` 预览，生成静态站点用 `hugo` 命令。**

