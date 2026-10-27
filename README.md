# wzc 的成长档案

一个后端工程师的个人站，记录技术笔记、读书思考、旅行相册和常用资源。

- 站点：<https://w2ch3n.github.io/>
- 技术栈：Hugo 0.168（extended）+ [PaperMod](https://github.com/adityatelange/hugo-PaperMod)
- 源码仓库：[`w2ch3n/personal-blog-papermod`](https://github.com/w2ch3n/personal-blog-papermod)，源码在 `main`
- 部署仓库：[`w2ch3n/w2ch3n.github.io`](https://github.com/w2ch3n/w2ch3n.github.io)，只保留 `master` 构建产物
- 推送 `main` 后，Actions 构建并把 `public/` 推送到部署仓库的 `master`，GitHub Pages 从 `master` 发布。

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

推送 `main` 后，[`.github/workflows/deploy.yml`](.github/workflows/deploy.yml) 会：

1. 拉取主题 submodule；
2. 用 Hugo 0.168 extended 构建；
3. 用仓库 secret `DEPLOY_KEY`（对部署仓库有 write 权限的 deploy key）把 `public/` 强制推送到 `w2ch3n.github.io` 的 `master`；
4. GitHub Pages 从 `master` 根目录发布。

配置部署的步骤：

1. 在 `w2ch3n.github.io` 的 **Settings → Deploy keys** 添加公钥，勾选 **Allow write access**；
2. 在 `personal-blog-papermod` 的 **Settings → Secrets and variables → Actions** 添加 `DEPLOY_KEY`，值为私钥。

> 部署仓库只放构建产物，不要再往它的 `master` 手写内容；源码改动都在本仓库的 `main` 上完成。
