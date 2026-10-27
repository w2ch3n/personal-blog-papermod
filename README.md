# ch3n.log

一个后端工程师的个人站，记录技术、读书和旅行，也放一些常用资源。

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

文章放在 `content/post/`，一篇一个 page bundle：

```text
content/post/my-note/
├── index.md
└── images/
    ├── 01.png
    └── 02.png
```

```bash
hugo new content/post/my-note/index.md
```

正文里用相对路径引用：

```md
![说明](images/01.png)
```

所有旧文章都已经迁移成 page bundle，图片放在各自的 `images/` 下，不再依赖 `static/img` 和 `../../`；`/p/<slug>/` 的文章地址保持不变。

## 加旅行

一次旅行一个目录：

```text
content/travel/2026-05-dali/
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

{{< travel-gallery >}}
```

照片按文件名排序，`cover.jpg` 作为封面，不会出现在照片墙里。照片墙用本地自带的 PhotoSwipe，点击可以看大图。建议长边压到 1600px、JPEG 质量 80 左右，原图不要提交到仓库。

旅行列表按年份分组，每条显示日期、标题、地点和封面；点进去是这次旅行写下的日记，下面是照片墙。旅行的 `date` 决定它在时间线上的位置。

## 照片怎么处理

原图不要提交到仓库。把原图留在手机相册、移动硬盘或网盘里，仓库只放导出后的 Web 尺寸照片。

每张照片建议：

- 长边 1600px 左右，JPEG 质量 80～85；
- 单张控制在 200～400KB；
- 一次旅行选 9～20 张，不要全传；
- 导出时去掉 GPS 等 EXIF 信息，但保留正确的旋转方向。

macOS 可以用自带的 `sips`：

```bash
sips -Z 1600 -s format jpeg -s formatOptions 82 原图.jpg --out 01.jpg
```

Linux / Windows 装了 ImageMagick 的话：

```bash
magick 原图.jpg -auto-orient -resize '1600x1600>' -strip -quality 82 01.jpg
```

HEIC 照片先转成 JPEG；在浏览器里也可以用 [Squoosh](https://squoosh.app/) 本地压缩，不会上传图片。

照片提交进 Git 后，即使以后删掉，历史里仍然会保留。所以第一次提交前就压好，不要先传原图再后悔。

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
