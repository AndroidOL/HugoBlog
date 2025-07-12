
+++
date = "2025-07-11T15:25:51+08:00"
draft = false
title = "Getting Started with Hugo Website Setup"
+++

This article provides a detailed guide on how to build a Hugo static website from scratch on macOS without relying on Homebrew. It includes key operations such as adding a Git-based theme, modifying configurations, and creating content, helping you quickly set up your own high-performance blog.

---

## 1. Preparation: Install Hugo and Git

### 1.1 Manually Download and Install Hugo

1. Open the [Hugo official Releases page](https://github.com/gohugoio/hugo/releases).
2. Download the Extended version for macOS, such as:  
   `hugo_extended_0.148.0_darwin-universal.tar.gz`
3. Run the following commands in the terminal:

   ```bash
   cd ~/Downloads
   tar -zxvf hugo_extended_0.148.0_darwin-universal.tar.gz
   sudo mv hugo /usr/local/bin/
   chmod +x /usr/local/bin/hugo
```

4. Verify the installation:
    ```bash
    hugo version
    ```

Make sure the output includes the word `extended`.

---

### 1.2 Install and Verify Git

macOS comes with Git preinstalled. If not, follow the prompt to install Xcode Command Line Tools:

```bash
git --version
```

Ensure the Git version is displayed correctly.

---

## 2. Create a Hugo Project

### 2.1 Create a New Site

```bash
hugo new site www-blog
```

> `www-blog` is the name of the new folder. You can name it anything — it does not affect your blog's title or content, only the root directory name.

### 2.2 Enter the Project Directory

```bash
cd www-blog
```

---

## 3. Add a Theme

### 3.1 Add an Official Theme via Git (Example: Ananke)

```bash
git init
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
```

> **Tip**: If you encounter network or proxy issues, manually download the ZIP file and extract it into the `themes/ananke` folder.

### 3.2 Modify Config File to Enable the Theme

Edit `hugo.toml` in the project root directory and add or modify the following:

```toml
baseURL = "http://example.org/"
languageCode = "zh-CN"
title = "My Hugo Blog"
theme = "ananke"
```

---

## 4. Create Content

### 4.1 Create a New Markdown Post

```bash
hugo new posts/hello.md
```

### 4.2 Edit the Post Content

Open `content/posts/hello.md`, change `draft: true` to `draft: false`. Example content:

```yaml
---
title: "Hello Hugo"
date: 2025-07-11T14:00:00+08:00
draft: false
---

This is my first blog post created with Hugo.  
Welcome to read and share!
```

---

## 5. Preview the Website Locally

Start the local Hugo server:

```bash
hugo server -D
```

Then open your browser and visit:

```
http://localhost:1313
```

You’ll see the homepage rendered with your theme and content.

---

## 6. Generate Static Site Files

After stopping the server, run:

```bash
hugo
```

The static files will be generated in the `public/` folder under the project root.

---

## 7. Deploy to GitHub Pages

### 7.1 Create a New GitHub Repository (e.g., `www-blog`)

### 7.2 Push the `public` Folder Content to `gh-pages` Branch

```bash
cd public
git init
git remote add origin https://github.com/your-username/www-blog.git
git checkout -b gh-pages
git add .
git commit -m "Initial Hugo site deployment"
git push -f origin gh-pages
```

### 7.3 Enable GitHub Pages in Repository Settings

- Go to `Settings` → `Pages`
- Select the `gh-pages` branch as the publishing source
- After saving, wait a few minutes and visit:

```
https://your-username.github.io/www-blog/
```

---

## 8. Summary Notes

- **The folder name in `hugo new site <folder-name>` is only the project directory name**, it does not affect the blog title or URL.
- **Blog title and site configuration are set in `hugo.toml`**, and can be changed anytime.
- **Themes are added via Git submodules**, but you can also download and extract the theme manually.
- **Make sure to set `draft: false` when writing posts**, or they won’t be published.
- **Use `hugo server -D` for local preview, and `hugo` command to generate static site files.**