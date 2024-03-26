# Hugo+Github 搭建个人主页


## 1 准备阶段

### 1.1 安装hugo

这里为了省去环境路径的配置采用如下的cmd命令方式，也可以选择去github下载相应文件。如文档：[Hugo中文文档 (gohugo.org)](https://www.gohugo.org/)

```cmd
winget install Hugo.Hugo.Extended
```

### 1.2 安装git

[Git - Downloads (git-scm.com)](https://git-scm.com/downloads)

选择合适的版本一直下一步即可。

在使用git前需要设置一下用户名和邮箱。

```csharp
git config --global user.name "username"
```

```typescript
git config --global user.email useremail@qq.com 
```

### 1.3 创建Github仓库

需要创建两个仓库一个公开仓库名字是username.github.io，名字必须是这个格式。另外创建一个私有仓库，是为了方便管理源代码。

## 2 创建 site

参考：[Quick start | Hugo (gohugo.io)](https://gohugo.io/getting-started/quick-start/)

创建一个site

```text
hugo new site WebSite
```

```text
cd WebSite
```

初始化git仓库

```powershell
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/CATSLAB-SDU/WebSite.git
git push -u origin main
```

模板设置，可以更换自己喜欢的模板[Complete List | Hugo Themes (gohugo.io)](https://themes.gohugo.io/)

```powershell
git clone https://github.com/dillonzq/LoveIt.git themes/LoveIt
```

修改hugo.toml文件，建议使用vscode

```toml
baseURL = "https://CATSLAB-SDU.github.io"
# 更改使用 Hugo 构建网站时使用的默认主题
theme = "LoveIt"

uglyURLs=true

# 网站语言, 仅在这里 CN 大写 ["en", "zh-CN", "fr", "pl", ...]
languageCode = "zh-CN"
# 语言名称 ["English", "简体中文", "Français", "Polski", ...]
languageName = "简体中文"
# 是否包括中日韩文字
hasCJKLanguage = true
# 默认每页列表显示的文章数目
paginate = 12
# 谷歌分析代号 [UA-XXXXXXXX-X]

# 版权描述，仅仅用于 SEO
copyright = ""

# 是否使用 robots.txt
enableRobotsTXT = true
# 是否使用 git 信息
enableGitInfo = true
# 是否使用 emoji 代码
enableEmoji = true

# 忽略一些构建错误
ignoreErrors = ["error-remote-getjson", "error-missing-instagram-accesstoken"]

# 作者配置
[author]
  name = "CATS LAB"
  email = "catslabsdu@gmail.com"
  link = ""

# 菜单配置
[menu]
  [[menu.main]]
    weight = 1
    identifier = "笔记"
    # 你可以在名称 (允许 HTML 格式) 之前添加其他信息, 例如图标
    pre = ""
    # 你可以在名称 (允许 HTML 格式) 之后添加其他信息, 例如图标
    post = ""
    name = "笔记"
    url = "/posts/"
    # 当你将鼠标悬停在此菜单链接上时, 将显示的标题
    title = ""
  [[menu.main]]
    weight = 2
    identifier = "标签"
    pre = ""
    post = ""
    name = "标签"
    url = "/tags/"
    title = ""
  [[menu.main]]
    weight = 3
    identifier = "分类"
    pre = ""
    post = ""
    name = "分类"
    url = "/categories/"
    title = ""

[params]
  # 网站默认主题样式 ["auto", "light", "dark"]
  defaultTheme = "light"
  # 公共 git 仓库路径，仅在 enableGitInfo 设为 true 时有效
  gitRepo = ""
  #  哪种哈希函数用来 SRI, 为空时表示不使用 SRI
  # ["sha256", "sha384", "sha512", "md5"]
  fingerprint = "sha256"
  #  日期格式
  dateFormat = "2006-01-02"
  # 网站标题, 用于 Open Graph 和 Twitter Cards
  title = "CATS LAB"
  # 网站描述, 用于 RSS, SEO, Open Graph 和 Twitter Cards
  description= "实验室学习资料共享博客"
  # 网站图片, 用于 Open Graph 和 Twitter Cards
  images = []

  # 页面头部导航栏配置
  [params.header]
    # 桌面端导航栏模式 ["fixed", "normal", "auto"]
    desktopMode = "fixed"
    # 移动端导航栏模式 ["fixed", "normal", "auto"]
    mobileMode = "auto"
    #  页面头部导航栏标题配置
    [params.header.title]
      # LOGO 的 URL
      logo = "/favicon.ico"
      # 标题名称
      name = "CATS LAB"
      # 你可以在名称 (允许 HTML 格式) 之前添加其他信息, 例如图标
      pre = ""
      # 你可以在名称 (允许 HTML 格式) 之后添加其他信息, 例如图标
      post = ""
      #  是否为标题显示打字机动画
      typeit = false

  # 页面底部信息配置
  [params.footer]
    enable = true
    #  自定义内容 (支持 HTML 格式)
    custom = '' 
    #  是否显示 Hugo 和主题信息
    hugo = true
    #  是否显示版权信息
    copyright = true
    #  是否显示作者
    author = true
    # 网站创立年份
    since = 2024
    # ICP 备案信息，仅在中国使用 (支持 HTML 格式)
    icp = ""
    # 许可协议信息 (支持 HTML 格式)
    license = '<a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>'
   
  #  Section (所有文章) 页面配置
  [params.section]
    # section 页面每页显示文章数量
    paginate = 20
    # 日期格式 (月和日)
    dateFormat = "01-02"
    # RSS 文章数目
    rss = 10

  #  List (目录或标签) 页面配置
  [params.list]
    # list 页面每页显示文章数量
    paginate = 20
    # 日期格式 (月和日)
    dateFormat = "01-02"
    # RSS 文章数目
    rss = 10

  #  应用图标配置
  [params.app]
    # 当添加到 iOS 主屏幕或者 Android 启动器时的标题, 覆盖默认标题
    title = "CATS LAB"
    # 是否隐藏网站图标资源链接
    noFavicon = true
    # 更现代的 SVG 网站图标, 可替代旧的 .png 和 .ico 文件
    svgFavicon = "favicon-32x32.png"
    # Android 浏览器主题色
    themeColor = "#ffffff"
    # Safari 图标颜色
    iconColor = "#5bbad5"
    # Windows v8-10磁贴颜色
    tileColor = "#da532c"

  #  搜索配置
  [params.search]
    enable = true
    # 搜索引擎的类型 ["lunr", "algolia"]
    type = "algolia"
    # 文章内容最长索引长度
    contentLength = 4000
    # 搜索框的占位提示语
    placeholder = ""
    #  最大结果数目
    maxResultLength = 10
    #  结果内容片段长度
    snippetLength = 50
    #  搜索结果中高亮部分的 HTML 标签
    highlightTag = "em"
    #  是否在搜索索引中使用基于 baseURL 的绝对路径
    absoluteURL = false
    [params.search.algolia]
      index = "blog"
      appID = "MHT5MEZUE6"
      searchKey = "462626f430ff595a97c125945eaac26d"

  # 主页配置
  [params.home]
    #  RSS 文章数目
    rss = 10
    # 主页个人信息
    [params.home.profile]
      enable = true
      # Gravatar 邮箱，用于优先在主页显示的头像
      gravatarEmail = ""
      # 主页显示头像的 URL
      avatarURL = "/avatar.png"
      #  主页显示的网站标题 (支持 HTML 格式)
      title = "CATS LAB"
      # 主页显示的网站副标题 (允许 HTML 格式)
      subtitle = ""
      # 是否为副标题显示打字机动画
      typeit = true
      # 是否显示社交账号
      social = true
      #  免责声明 (支持 HTML 格式)
      disclaimer = "内容为学习资料分享，仅供参考，侵权请联系删除。"
    # 主页文章列表
    [params.home.posts]
      enable = true
      # 主页每页显示文章数量
      paginate = 6
      #  被 params.page 中的 hiddenFromHomePage 替代
      # 当你没有在文章前置参数中设置 "hiddenFromHomePage" 时的默认行为
      defaultHiddenFromHomePage = false

  # 作者的社交信息设置
  [params.social]
    GitHub = "CATSLAB-SDU"
    Linkedin = ""
    Twitter = ""
    Instagram = ""
    Facebook = ""
    Telegram = ""
    Medium = ""
    Gitlab = ""
    Youtubelegacy = ""
    Youtubecustom = ""
    Youtubechannel = ""
    Tumblr = ""
    Quora = ""
    Keybase = ""
    Pinterest = ""
    Reddit = ""
    Codepen = ""
    FreeCodeCamp = ""
    Bitbucket = ""
    Stackoverflow = ""
    Weibo = ""
    Odnoklassniki = ""
    VK = ""
    Flickr = ""
    Xing = ""
    Snapchat = ""
    Soundcloud = ""
    Spotify = ""
    Bandcamp = ""
    Paypal = ""
    Fivehundredpx = ""
    Mix = ""
    Goodreads = ""
    Lastfm = ""
    Foursquare = ""
    Hackernews = ""
    Kickstarter = ""
    Patreon = ""
    Steam = ""
    Twitch = ""
    Strava = ""
    Skype = ""
    Whatsapp = ""
    Zhihu = ""
    Douban = ""
    Angellist = ""
    Slidershare = ""
    Jsfiddle = ""
    Deviantart = ""
    Behance = ""
    Dribbble = ""
    Wordpress = ""
    Vine = ""
    Googlescholar = ""
    Researchgate = ""
    Mastodon = ""
    Thingiverse = ""
    Devto = ""
    Gitea = ""
    XMPP = ""
    Matrix = ""
    Bilibili = ""
    Discord = ""
    DiscordInvite = ""
    Lichess = ""
    ORCID = ""
    Pleroma = ""
    Kaggle = ""
    MediaWiki= ""
    Plume = ""
    HackTheBox = ""
    RootMe= ""
    Phone = ""
    Email = "yinxkai@gmail.com"
    RSS = false # 

  #  文章页面全局配置
  [params.page]
    #  是否在主页隐藏一篇文章
    hiddenFromHomePage = false
    #  是否在搜索结果中隐藏一篇文章
    hiddenFromSearch = false
    #  是否使用 twemoji
    twemoji = false
    # 是否使用 lightgallery
    lightgallery = false
    #  是否使用 ruby 扩展语法
    ruby = true
    #  是否使用 fraction 扩展语法
    fraction = true
    #  是否使用 fontawesome 扩展语法
    fontawesome = true
    # 是否在文章页面显示原始 Markdown 文档链接
    linkToMarkdown = true
    #  是否在 RSS 中显示全文内容
    rssFullText = false
    #  目录配置
    [params.page.toc]
      # 是否使用目录
      enable = true
      #  是否保持使用文章前面的静态目录
      keepStatic = false
      # 是否使侧边目录自动折叠展开
      auto = true
    #  代码配置
    [params.page.code]
      # 是否显示代码块的复制按钮
      copy = true
      # 默认展开显示的代码行数
      maxShownLines = 50
    #  KaTeX 数学公式
    [params.page.math]
      enable = true
      #  默认行内定界符是 $ ... $ 和 \( ... \)
      inlineLeftDelimiter = ""
      inlineRightDelimiter = ""
      #  默认块定界符是 $$ ... $$, \[ ... \],  \begin{equation} ... \end{equation} 和一些其它的函数
      blockLeftDelimiter = ""
      blockRightDelimiter = ""
      # KaTeX 插件 copy_tex
      copyTex = true
      # KaTeX 插件 mhchem
      mhchem = true
    #  Mapbox GL JS 配置
    [params.page.mapbox]
      # Mapbox GL JS 的 access token
      accessToken = ""
      # 浅色主题的地图样式
      lightStyle = "mapbox://styles/mapbox/light-v10?optimize=true"
      # 深色主题的地图样式
      darkStyle = "mapbox://styles/mapbox/dark-v10?optimize=true"
      # 是否添加 NavigationControl
      navigation = true
      # 是否添加 GeolocateControl
      geolocate = true
      # 是否添加 ScaleControl
      scale = true
      # 是否添加 FullscreenControl
      fullscreen = true
    #  文章页面的分享信息设置
    [params.page.share]
      enable = true
      Twitter = true
      Facebook = true
      Linkedin = false
      Whatsapp = false
      Pinterest = false
      Tumblr = false
      HackerNews = true
      Reddit = false
      VK = false
      Buffer = false
      Xing = false
      Line = true
      Instapaper = false
      Pocket = false
      Flipboard = false
      Weibo = true
      Blogger = false
      Baidu = false
      Odnoklassniki = false
      Evernote = false
      Skype = false
      Trello = false
      Mix = false
    #  评论系统设置
    [params.page.comment]
      enable = true
      # Disqus 评论系统设置
      [params.page.comment.disqus]
        # 
        enable = false
        # Disqus 的 shortname，用来在文章中启用 Disqus 评论系统
        shortname = ""
      # Gitalk 评论系统设置
      [params.page.comment.gitalk]
        # 
        enable = true
        owner = "CATSLAB-SDU"
        repo = "CATSLAB-SDU.github.io"
        clientId = "cee9a7299c7ed214f720"
        clientSecret = "b6b040a40001c2d2129236573ac7695d3fec20e3"
        id= "location.pathname" # 文章页面的链接地址就是ID
        labels= "gitalk" # Github issue labels. If you used to use Gitment, you can change it
        perPage= 15 # Pagination size, with maximum 100.
        pagerDirection= "last" # Comment sorting direction, available values are 'last' and 'first'.
        createIssueManually= true # 设置为true，如果是管理员登录，会自动创建issue，如果是false，需要管理员手动添加第一个评论(issue)
        distractionFreeMode= false # Enable hot key (cmd|ctrl + enter) submit comment.
      # Valine 评论系统设置
      [params.page.comment.valine]
        enable = false
        appId = ""
        appKey = ""
        placeholder = ""
        avatar = "mp"
        meta= ""
        pageSize = 10
        # 为空时自动适配当前主题 i18n 配置
        lang = ""
        visitor = true
        recordIP = true
        highlight = true
        enableQQ = false
        serverURLs = ""
        #  emoji 数据文件名称, 默认是 "google.yml"
        # ["apple.yml", "google.yml", "facebook.yml", "twitter.yml"]
        # 位于 "themes/LoveIt/assets/lib/valine/emoji/" 目录
        # 可以在你的项目下相同路径存放你自己的数据文件:
        # "assets/lib/valine/emoji/"
        emoji = ""
      # Facebook 评论系统设置
      [params.page.comment.facebook]
        enable = false
        width = "100%"
        numPosts = 10
        appId = ""
        # 为空时自动适配当前主题 i18n 配置
        languageCode = "zh_CN"
      #  Telegram Comments 评论系统设置
      [params.page.comment.telegram]
        enable = false
        siteID = ""
        limit = 5
        height = ""
        color = ""
        colorful = true
        dislikes = false
        outlined = false
      #  Commento 评论系统设置
      [params.page.comment.commento]
        enable = false
      #  utterances 评论系统设置
      [params.page.comment.utterances]
        enable = false
        # owner/repo
        repo = ""
        issueTerm = "pathname"
        label = ""
        lightTheme = "github-light"
        darkTheme = "github-dark"
      # giscus comment 评论系统设置 (https://giscus.app/zh-CN)
      [params.page.comment.giscus]
        # 你可以参考官方文档来使用下列配置
        enable = false
        repo = ""
        repoId = ""
        category = "Announcements"
        categoryId = ""
        # 为空时自动适配当前主题 i18n 配置
        lang = ""
        mapping = "pathname"
        reactionsEnabled = "1"
        emitMetadata = "0"
        inputPosition = "bottom"
        lazyLoading = false
        lightTheme = "light"
        darkTheme = "dark"
    #  第三方库配置
    [params.page.library]
      [params.page.library.css]
        # someCSS = "some.css"
        # 位于 "assets/"
        # 或者
        # someCSS = "https://cdn.example.com/some.css"
      [params.page.library.js]
        # someJavascript = "some.js"
        # 位于 "assets/"
        # 或者
        # someJavascript = "https://cdn.example.com/some.js"
    #  页面 SEO 配置
    [params.page.seo]
      # 图片 URL
      images = []
      # 出版者信息
      [params.page.seo.publisher]
        name = ""
        logoUrl = ""

  #  TypeIt 配置
  [params.typeit]
    # 每一步的打字速度 (单位是毫秒)
    speed = 100
    # 光标的闪烁速度 (单位是毫秒)
    cursorSpeed = 1000
    # 光标的字符 (支持 HTML 格式)
    cursorChar = "|"
    # 打字结束之后光标的持续时间 (单位是毫秒, "-1" 代表无限大)
    duration = -1

  # 网站验证代码，用于 Google/Bing/Yandex/Pinterest/Baidu
  [params.verification]
    google = ""
    bing = ""
    yandex = ""
    pinterest = ""
    baidu = ""

  #  网站 SEO 配置
  [params.seo]
    # 图片 URL
    image = "/favicon-32x32.png"
    # 缩略图 URL
    thumbnailUrl = "/favicon-32x32.png"

  #  网站分析配置
  [params.analytics]
    enable = true
    # Google Analytics
    [params.analytics.google]
      id = ""
      # 是否匿名化用户 IP
      anonymizeIP = true
    # Fathom Analytics
    [params.analytics.fathom]
      id = ""
      # 自行托管追踪器时的主机路径
      server = ""
    # Plausible Analytics
    [params.analytics.plausible]
      dataDomain = ""
    # Yandex Metrica
    [params.analytics.yandexMetrica]
      id = ""

  #  Cookie 许可配置
  [params.cookieconsent]
    enable = false
    # 用于 Cookie 许可横幅的文本字符串
    [params.cookieconsent.content]
      message = ""
      dismiss = ""
      link = ""

  #  第三方库文件的 CDN 设置
  [params.cdn]
    # CDN 数据文件名称, 默认不启用
    # ["jsdelivr.yml"]
    # 位于 "themes/LoveIt/assets/data/cdn/" 目录
    # 可以在你的项目下相同路径存放你自己的数据文件:
    # "assets/data/cdn/"
    data = ""

  #  兼容性设置
  [params.compatibility]
    # 是否使用 Polyfill.io 来兼容旧式浏览器
    polyfill = false
    # 是否使用 object-fit-images 来兼容旧式浏览器
    objectFit = false

# Hugo 解析文档的配置
[markup]
  # 语法高亮设置
  [markup.highlight]
    codeFences = true
    guessSyntax = true
    lineNos = true
    lineNumbersInTable = true
    # false 是必要的设置
    # (https://github.com/dillonzq/LoveIt/issues/158)
    noClasses = false
  # Goldmark 是 Hugo 0.60 以来的默认 Markdown 解析库
  [markup.goldmark]
    [markup.goldmark.extensions]
      definitionList = true
      footnote = true
      linkify = true
      strikethrough = true
      table = true
      taskList = true
      typographer = true
    [markup.goldmark.renderer]
      # 是否在文档中直接使用 HTML 标签
      unsafe = true
  # 目录设置
  [markup.tableOfContents]
    startLevel = 2
    endLevel = 6

# 网站地图配置
[sitemap]
  changefreq = "weekly"
  filename = "sitemap.xml"
  priority = 0.5

# Permalinks 配置
[Permalinks]
  # posts = ":year/:month/:filename"
  posts = ":filename"

# 隐私信息配置
[privacy]
  #  Google Analytics 相关隐私 (被 params.analytics.google 替代)
  [privacy.googleAnalytics]
    # ...
  [privacy.twitter]
    enableDNT = true
  [privacy.youtube]
    privacyEnhanced = true

# 用于输出 Markdown 格式文档的设置
[mediaTypes]
  [mediaTypes."text/plain"]
    suffixes = ["md"]

# 用于输出 Markdown 格式文档的设置
[outputFormats.MarkDown]
  mediaType = "text/plain"
  isPlainText = true
  isHTML = false

# 用于 Hugo 输出文档的设置
[outputs]
  # 
  home = ["HTML", "RSS", "JSON"]
  page = ["HTML", "MarkDown"]
  section = ["HTML", "RSS"]
  taxonomy = ["HTML", "RSS"]
  taxonomyTerm = ["HTM本地启动，http://localhost:1313/ 查看效果
```

本地启动，http://localhost:1313/ 查看效果

本地启动测试。

```text
hugo server
```

![image](https://cdn.jsdelivr.net/gh/yinxiangkai/ImageBed@main/202403261443241.png)

## 3 部署到Github

进入pubilc创建仓库,并推送到公开仓库

```powershell
cd public
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/CATSLAB-SDU/CATSLAB-SDU.github.io.git
git push -u origin main
```

在github中进行配置  
​![image](assets/image-20240326104411-81jsv9o.png)​

## 4 自动部署

在站点根目录执行 `Hugo`​ 命令生成最终页面，注意替换仓库和主题。

```powershell
hugo --theme=LoveIt --baseURL="https://CATSLAB-SDU.github.io/"
```

在本地创建文件hugo.yaml

```powershell
 mkdir .github/workflows/
```

```powershell
new-item .github/workflows/main.yaml -type file
```

把下面内容复制到main.yaml中

```powershell
# Sample workflow for building and deploying a Hugo site to GitHub Pages
# This is a basic workflow to help you get started with Actions

name: CI

# Controls when the action will run.
on:
  # Triggers the workflow on push or pull request events but only for the master branch
  push:
    branches: [main]
  pull_request:
    branches: [main]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v2
        with:
          submodules: true # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0 # Fetch all history for .GitInfo and .Lastmod

      - name: Hugo setup
        # You may pin to the exact commit or the version.
        # uses: peaceiris/actions-hugo@2e89aa66d0093e4cd14751b3028fc1a179452c2e
        uses: peaceiris/actions-hugo@v2.4.13
        with:
          # The Hugo version to download (if necessary) and use. Example: 0.58.2
          hugo-version: latest # optional, default is latest
          # Download (if necessary) and use Hugo extended version. Example: true
          extended: true # optional, default is false

      - name: Build
        run: hugo

      - name: Pushes to another repository
        uses: cpina/github-action-push-to-another-repository@master
        env:
          API_TOKEN_GITHUB: ${{ secrets.API_TOKEN_GITHUB }}
        with:
          source-directory: "public"
          destination-github-username: "CATSLAB-SDU"
          destination-repository-name: "CATSLAB-SDU.github.io"
          user-email: catslabsdu@gmail.com

```

在github中设置`Developer settings/Personal access tokens`​，新建一个密钥，权限设置把`Repo`​打勾。

![image](https://cdn.jsdelivr.net/gh/yinxiangkai/ImageBed@main/202403261442618.png)​

回到私人仓库的设置里，新建密钥，将刚才生成的个人密钥填进去，名字设为`API_TOKEN_GITHUB`​(跟 CI 脚本里的名称对应即可)

![image](https://cdn.jsdelivr.net/gh/yinxiangkai/ImageBed@main/202403261442516.png)​

完成该设置后更新私有库后公共库会自动更新。

‍

## 4 细节设置

[Favicon Generator for perfect icons on all browsers (realfavicongenerator.net)](https://realfavicongenerator.net/)利用该网站生成头像等资源发在static目录下面。

‍

## 5 评论设置

首先要去`Github -> Settings -> Developer Settings -> OAuth App`​ 里注册一个新的 OAuth App 来给 Gitalk 使用。并修改`config.toml`:

```powershell
 [params.page.comment]
      enable = true  <-- 启用评论系统
      [params.page.comment.gitalk]
        enable = true
        owner = "JellyZhang"  <-- Github用户名
        repo = "blog-comment" <-- 用来存放评论的repo名称
        clientId = ""  <-- 申请好的OAuth的ClientId
        clientSecret = "" <-- 申请好的OAuth的ClientSecret
```

![image](https://cdn.jsdelivr.net/gh/yinxiangkai/ImageBed@main/202403261442238.png)​

## 6 搜索设置

在[AI search that understands | Algolia](https://www.algolia.com/)创建账号,创建一个`application`​与`index`:

![image](https://cdn.jsdelivr.net/gh/yinxiangkai/ImageBed@main/202403261442779.png)​

然后将下面的内容填写到`config.toml`​中：

```powershell
#  搜索配置
  [params.search]
    enable = true
    # 搜索引擎的类型 ["lunr", "algolia"]
    type = "algolia"
    # 文章内容最长索引长度
    contentLength = 4000
    # 搜索框的占位提示语
    placeholder = ""
    #  最大结果数目
    maxResultLength = 10
    #  结果内容片段长度
    snippetLength = 50
    #  搜索结果中高亮部分的 HTML 标签
    highlightTag = "em"
    #  是否在搜索索引中使用基于 baseURL 的绝对路径
    absoluteURL = false
    [params.search.algolia]
      index = "blog"
      appID = "9DJIN648JY"
      searchKey = "e0475938b166cc32e8e52c7b0c3e6071"

```

![image](https://cdn.jsdelivr.net/gh/yinxiangkai/ImageBed@main/202403261442164.png)​

‍

修改main.yaml，将下面内容加到最后面：

```powershell
 - name: Algolia Index Uploader
        # You may pin to the exact commit or the version.
        # uses: rxrw/algolia-index-uploader@294d1d600c4a2197a64903b6161cc80acea1becb
        uses: rxrw/algolia-index-uploader@v1
        with:
          # Your Algolia IndexPath
          index_path: public/index.json 
          # Algolia Index Id
          algolia_index_id: 9DJIN648JY
          # Algolia Index Name
          algolia_index_name: CATSLAB
          # Algolia Admin Key
          algolia_index_admin_key: d4f23d91632cfbe0eeb5b9a6debbe280
```

‍

## 7 日常维护

拉取WebSite仓库，然后将相应的markdown文件放到content/posts目录下，在文件前面加入：

```powershell
+++
title = "title"
date = "2024-03-06T16:02:29+08:00"
draft = fasle
subtitle = ""
tags = ["笔记1","笔记1"]
categories = ["方向"]
license = '<a rel="license external nofollow noopener noreferrer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>'
+++

```

。并执行根目录下的脚本文件run.sh即可。

‍

## 8 日常维护

markdown中的图片需要上传到网络图床。

下载PicGo这里推荐山东大学的镜像网站：[v2.3.1 (sdu.edu.cn)](https://mirrors.sdu.edu.cn/github-release/Molunerfinn_PicGo/v2.3.1/)

进行图床设置：

```powershell
仓库名:   CATSLAB-SDU/ImageBed

分支：main

token：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

存储路径：

自定义域名：https://cdn.jsdelivr.net/gh/CATSLAB-SDU/ImageBed@main
```

![image](https://cdn.jsdelivr.net/gh/yinxiangkai/ImageBed@main/202403261442541.png)​

‍

