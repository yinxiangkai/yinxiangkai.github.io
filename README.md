# 豈風

个人技术站点源码，使用 [Astro](https://astro.build/) + Tailwind CSS 构建，
页面结构参考 [AIInfraGuide](https://github.com/caomaolufei/AIInfraGuide)。
在线访问：[www.qifeng.xyz](https://www.qifeng.xyz/)。

## 站点结构（六部）

导航与内容按六个部分组织，目录名即对应的英文：

| 导航 | 英文目录 | 定位 |
|---|---|---|
| 丛编 | `series` | 按主题组织的系列文章 |
| 杂记 | `essays` | 零散随笔 |
| 集萃 | `collection` | 收藏的书、影、图，每条一页 |
| 门径 | `paths` | 学习路线，一条路线一页 |
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
> `docs/essays/`、`docs/collection/`、`docs/paths/` 为空时，`npm run dev` 会提示
> `The collection "xxx" does not exist or is empty.` —— 写入第一篇内容后该提示自动消失，不影响构建。

## 目录结构

```
docs/                内容（Markdown）
  series/            丛编
  essays/            杂记
  collection/        集萃（books/ films/ gallery/ 三个子库）
  paths/             门径（学习路线）
public/              静态资源（图片、图标、favicon）
src/
  components/        Astro 组件（导航、侧栏、TOC、搜索、评论等）
  content/config.ts  内容集合的 frontmatter 校验规则
  data/              站点常量与链接数据
  layouts/           页面骨架
  pages/             路由（旧地址 /blog /columns /friends /about 在此留 301 跳转）
  utils/             丛编/杂记/集萃/门径的读取与排序逻辑
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
| `docs/collection/<子库>/<条目>.md` | `/collection/<子库>/<条目>/` |
| `docs/paths/<路线>.md` | `/paths/<路线>/` |
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
icon: bot                   # 图标名，见 SeriesIcon.astro 注册表，缺省 book
date: 2026-08-26           # 首页信息流排序用
order: 0                   # 丛编之间的排序
pinned: false              # 置顶到首页信息流
draft: false               # 为 true 时整篇不构建
---
```

文章 frontmatter 另支持 `tags: [CUDA, GPU]`。

丛编、集萃、门径通用的图标名（取自 [lucide](https://lucide.dev/)，
新增图标就在 `src/components/SeriesIcon.astro` 里登记一行）：

`bot` `brain` `book` `code` `compass` `cpu` `film` `gpu` `hexagon` `image` `layers` `route` `server` `workflow`

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

### 集萃

`docs/collection/<子库>/<条目>.md`，子库固定三个：`books` 书库 / `films` 影库 / `gallery` 图库
（新增子库 = 建目录 + 在 `src/utils/collection.ts` 的 `LIBRARIES` 里登记一行）。
一个条目一个文件，文件名即 URL 片段。

三个子库共用一套字段，按需取用；封面用外链或站内路径都行，
不填封面会渲染成带书名与子库图标的占位块，网格不会破相。

```yaml
---
title: Designing Data-Intensive Applications  # 必填，书名 / 片名
creator: Martin Kleppmann    # 作者 / 导演
cover: https://image.example.com/xxx.webp  # 封面 / 海报，站内路径或完整外链
category: 专业                # 类型 / 题材：卡片角标，点击可筛出同类
rating: 4                    # 评分 0–5，省略即未评分，页面显示为星级
date: 2020-02-04             # 出版日期 / 首播
link: https://book.douban.com/subject/xx/  # 外链（微信读书 / 豆瓣等）
description: 内容简介         # 缺省时自动截取正文首段
status: 待读                  # 阅读 / 观看状态（保留在数据里，页面暂不展示）
# 书籍专有
edition: 2
isbn: 978-3031006364
goal: 2026
deadline: 2026-04-30
# 影视专有
language: 日语
# 通用
tags: []
pinned: false                 # 置顶；列表默认按标题排序
draft: false
---
```

正文可选，写长评时会在详情页按文章排版渲染。

### 门径

`docs/paths/<路线>.md`，一条路线一个文件、一个独立页面，正文用自由 Markdown 写。
`docs/paths/index.md` 只提供总览页的标题与描述，正文不渲染。

```yaml
---
title: 机器学习入门路线     # 必填
description: 一句话说明      # 可选，缺省时自动截取正文首段
icon: bot                 # 可选，图标名（SeriesIcon.astro 注册表，缺省 compass）
order: 1                  # 可选，路线之间的排序
date: 2026-08-26          # 可选
tags: [入门, 约 6 个月]     # 可选，显示为角标
pinned: false
draft: false
---
```

### 写作语法

- 数学公式：`$...$` 与 `$$...$$`（KaTeX 渲染）
- 流程图：```` ```mermaid ```` 代码块
- 代码块：Shiki 高亮，带 Mac 风格标题栏与复制按钮
- 站外链接自动加 `target="_blank"` 与 `rel="nofollow noopener"`

