# 豈風

个人技术站点源码，使用 [Astro](https://astro.build/) + Tailwind CSS 构建，
页面结构参考 [AIInfraGuide](https://github.com/caomaolufei/AIInfraGuide)。
在线访问：[www.qifeng.xyz](https://www.qifeng.xyz/)。

## 站点结构（四部）

导航与内容按四个部分组织，目录名即对应的英文：

| 导航 | 英文目录 | 定位 |
|---|---|---|
| 丛编 | `series` | 按主题组织的系列文章 |
| 杂记 | `essays` | 零散随笔 |
| 外编 | `links` | 收集的文章与网站 |
| 自序 | `preface` | 网站介绍与写作缘起 |

## 本地开发

```bash
npm install
npm run dev              # 本地预览 http://localhost:4321
npm run build            # 构建到 dist/（含 astro check、Pagefind 索引）
npm run preview          # 预览构建产物
```

> 站内搜索由 [Pagefind](https://pagefind.app/) 提供，索引在 `npm run build` 时生成。
> `npm run dev` 下搜索框会读取上一次构建留下的 `public/pagefind/`，首次使用请先构建一次。
>
> `docs/essays/` 为空时，`npm run dev` 会提示 `The collection "essays" does not exist or is empty.`
> —— 写入第一篇杂记后该提示自动消失，不影响构建。

## 目录结构

```
docs/                内容（Markdown）
  series/            丛编
  essays/            杂记
public/              静态资源（图片、图标、favicon）
src/
  components/        Astro 组件（导航、侧栏、TOC、搜索、评论等）
  content/config.ts  内容集合的 frontmatter 校验规则
  data/              站点常量与链接数据
  layouts/           页面骨架
  pages/             路由（旧地址 /blog /columns /friends /about 在此留 301 跳转）
  utils/             丛编/杂记的读取与排序逻辑
```

内容目录与路由的对应关系：

| 文件 | 页面 |
|---|---|
| `src/pages/index.astro` | `/` 首页（hero 与信息流均为自动生成） |
| `docs/series/index.md` | `/series/` 总览（提供标题与描述） |
| `docs/series/<丛编>/index.md` | `/series/<丛编>/` 丛编首页 |
| `docs/series/<丛编>/<章>/index.md` | `/series/<丛编>/<章>/` 章首页（正文会渲染） |
| `docs/series/<丛编>/<章>/*.md` | `/series/<丛编>/<章>/<文章>/` |
| `docs/essays/<类别>/<文章>.md` | `/essays/<类别>/<文章>/` |
| `src/data/links.ts` | `/links/` 外编 |
| `src/pages/preface/index.astro` | `/preface/` 自序 |
| `src/pages/guestbook/index.astro` | `/guestbook/` 留笺（访客留言板） |

## 内容约定

### 丛编

按「丛编 → 章 → 文章」的目录层级组织，层级即结构，无需手改任何配置文件：

- 目录名用英文；文章文件名用英文
- `第X章` 形式的目录按中文数字排序（如 `第1章-基础`、`第2章-进阶`），
  其余目录排在其后，按目录名排序
- 章内文章按 frontmatter 的 `order` 排序，相同则按文件名排序
- 丛编首页与丛编总览页的正文不渲染，只取 frontmatter 的
  `title` / `description` / `icon` / `date` / `order` / `pinned`

丛编 frontmatter 字段：

```yaml
---
title: AI 丛编              # 必填
description: 深度学习与大模型学习笔记…   # 丛编首页与首页卡片
icon: bot                   # 图标名（SeriesIcon.astro 注册表：bot/cpu/book/code/layers），缺省 book
date: 2026-08-26           # 首页信息流排序用
order: 0                   # 丛编之间的排序
pinned: false              # 置顶到首页信息流
draft: false               # 为 true 时整篇不构建
---
```

文章 frontmatter 另支持 `tags: [CUDA, GPU]`。

### 杂记

`docs/essays/<类别>/<文章>.md`，`<类别>` 会成为卡片角标。
`tags` 里的标签可点击，每个标签有独立的 `/essays/tags/<标签>/` 列表页。
文章页左栏「杂记总目」按时间分组（年份 → 月份，可折叠）；
左侧目录与右侧文章内目录都可折叠。

杂记文章页布局：左栏总目录 + 正文 + 右侧本页目录（仅 xl 屏宽显示）。

```yaml
---
title: 文章标题             # 必填
date: 2026-08-26           # 必填，决定排序与上下篇
categories: [技术]         # 可选，缺省取目录名
tags: [CUDA, GPU]          # 可选
description: 摘要          # 可选，缺省时自动截取正文首段
heroImage: /images/x.png   # 可选，文章页头图
pinned: false
draft: false
---
```

### 写作语法

- 数学公式：`$...$` 与 `$$...$$`（KaTeX 渲染）
- 流程图：```` ```mermaid ```` 代码块
- 代码块：Shiki 高亮，带 Mac 风格标题栏与复制按钮
- 站外链接自动加 `target="_blank"` 与 `rel="nofollow noopener"`

