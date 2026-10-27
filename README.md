# wzc 的成长档案

一个后端工程师的个人站，记录技术笔记、读书思考、旅行相册和常用资源。

- 站点：<https://w2ch3n.github.io/>
- 技术栈：Hugo 0.168（extended）+ [PaperMod](https://github.com/adityatelange/hugo-PaperMod)
- 源文件在 `main` 分支；GitHub Actions 构建后把静态站点推送到 `master`，GitHub Pages 从 `master` 发布。

## 本地预览

需要 Hugo extended 0.158+（本项目用 0.168）：

```bash
git submodule update --init --recursive
hugo server -D
```

如果主题目录是空的，先执行上面的 submodule 命令。更新主题：

```bash
git -C themes/hugo-PaperMod fetch --tags
git -C themes/hugo-PaperMod checkout 1181196
```

## 写文章

文章放在 `content/post/`。推荐用 page bundle，这样封面和配图可以被 Hugo 处理成响应式尺寸：

```text
content/post/my-note/
├── index.md
└── cover.jpg
```

```bash
hugo new content/post/my-note/index.md
```

封面写在 front matter 里：

```yaml
cover:
  image: cover.jpg
  alt: 封面说明
```

旧文章仍然保留 `/p/<slug>/` 的地址，不需要迁移。

## 加相册

一本相册一个目录：

```text
content/albums/2026-05-dali/
├── index.md
├── cover.jpg
├── 01.jpg
├── 02.jpg
└── 03.jpg
```

`index.md` 示例：

```yaml
---
title: 大理
description: 云南 · 2026 年 5 月
date: 2026-05-01
location: 云南
cover:
  image: cover.jpg
  alt: 大理
ShowToc: false
comments: false
captions:
  01.jpg: 苍山下的清晨
  02.jpg: 洱海边的下午
---

写一小段这次旅行。

{{< album-gallery >}}
```

照片按文件名排序，`cover.jpg` 作为封面，不会出现在照片墙里。照片墙用本地自带的 PhotoSwipe，点击可以看大图。建议长边压到 1600px、JPEG 质量 80 左右，原图不要提交到仓库。

## 加资源

编辑 `data/resources.yaml`，按「阅读 / 工具 / 课程」分组。每条资源可以给外链，也可以把文件放到 `static/files/` 后用 `file:` 指向它。

原则：只放公版、CC、开源、免费公开的内容，或者自己写的东西；有版权的书只给官方购买/借阅链接。GitHub Pages 不适合当文件分发站。

## 部署

推送到 `main` 后，`.github/workflows/deploy.yml` 会自动：

1. 拉取主题 submodule；
2. 用 Hugo 0.168 extended 构建；
3. 把 `public/` 强制推送到 `master`。

GitHub Pages 从 `master` 根目录发布，所以站点地址是 <https://w2ch3n.github.io/>。

> 仓库的默认分支目前是 `master`（发布产物），源码在 `main`。改代码/写文章时记得先切到 `main`。
