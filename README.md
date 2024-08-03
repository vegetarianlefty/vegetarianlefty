- #                              Docsify快速搭建文档

  - ## docsify

    > 一个神奇的文档网站生成器。

    ## docsify 概述

    docsify 可以快速帮你生成文档网站。不同于 GitBook、Hexo 的地方是它不会生成静态的 `.html` 文件，所有转换工作都是在运行时。如果你想要开始使用它，只需要创建一个 `index.html` 就可以开始编写文档并直接[部署在 GitHub Pages](zh-cn/deploy.md)。

    查看[快速开始](zh-cn/quickstart.md)了解详情。

    ## 特性

    - 无需构建，写完文档直接发布
    - 容易使用并且轻量 (压缩后 ~21kB)
    - 智能的全文搜索
    - 提供多套主题
    - 丰富的 API
    - 支持 Emoji
    - 兼容 IE11
    - 支持服务端渲染 SSR ([示例](https://github.com/docsifyjs/docsify-ssr-demo))

  # 快速开始

  使用brew安装node(推荐)

  ```bash
  # 配置npm 配置源淘宝源加速（推荐）永久
  npm config set registry https://registry.npm.taobao.org
  
  # 安装淘宝cnpm（推荐）
  npm install -g cnpm --registry=https://registry.npm.taobao.org
  
  # 安装node环境
  brew install node
  
  # 查看node状态
  node -v
  ```

  推荐全局安装 `docsify-cli` 工具，可以方便地创建及在本地预览生成的文档。

  ```bash
  # 使用cnpm安装docsify-cli脚手架
  cnpm install i docsify-cli -g
  
  # 查看
  docsify -v
  ```

  ## 初始化项目

  如果想在项目的 `./docs` 目录里写文档，直接通过 `init` 初始化项目。

  ```bash
  docsify init ./docs
  
  # 初始化docsify的index.html主页
  docsify init <path> [--local false] [--theme vue]
  # path 默认是当前目录,可以指定相对目录和绝对目录,将目录下的文件作为文档文件
  # --local 默认是false 即使用unpkg.com作为baseurl
  # --theme 默认是vue 即使用vue风格 还有buble, dark 和 pure可选
  
  # 启动docsify本地静态网页服务
  docsify serve <path> [--open false] [--port 3000]
  # path 默认是当前目录,可以指定相对目录和绝对目录,将目录下的文件作为文档文件
  # --open 默认是false 即启动不打开网页
  # --port 默认是3000 即端口默认3000
  
  # 生成稳定的侧边栏目录（v4.4.3后才支持）
  docsify generate <path> [--sidebar _sidebar.md]
  # path 默认是当前目录,可以指定相对目录和绝对目录,将目录下的文件作为文档文件
  # --sidebar 默认_sidebar.md 将目录下的文档结构生产侧边栏目录
  ```

  ## 开始写文档

  初始化成功后，可以看到 `./docs` 目录下创建的几个文件

  - `index.html` 入口文件
  - `README.md` 会做为主页内容渲染
  - `.nojekyll` 用于阻止 GitHub Pages 忽略掉下划线开头的文件

  直接编辑 `docs/README.md` 就能更新文档内容，当然也可以[添加更多页面](zh-cn/more-pages.md)。

  ## 本地预览

  通过运行 `docsify serve` 启动一个本地服务器，可以方便地实时预览效果。默认访问地址 http://localhost:3000 。

  ```bash
  docsify serve docs
  ```

   更多命令行工具用法，参考 [docsify-cli 文档](https://github.com/docsifyjs/docsify-cli)。

  ## 自定义组件

  ```
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Document</title>
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
    <meta name="description" content="Description">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0">
    <link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify@4/lib/themes/vue.css">
  </head>
  <body>
    <body>
      <div id="app"></div>
      <script>
        window.$docsify = {
          name: 'mydoc',       //
          repo: '/',           //渲染github挂件 配置本地或线上地址
          search: 'auto',      //默认的全局搜索 详细参数请看插件文档
          loadSidebar: true,   //开启侧边栏 使用_sidebar.md 
          subMaxLevel: 2,      //侧边栏层级最大层级2
          loadNavbar: true,    //加载导航栏 需要编写_navbar.md
          autoHeader: true    //配合loadSidebar 自动添加标题
        }
      </script>
    <!-- 全文搜索插件 -->
    <script src="//unpkg.com/docsify/lib/plugins/search.min.js"></script>
    <!-- 图片缩放插件 -->
    <script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/zoom-image.min.js"></script>
    <!-- 代码拷贝插件 -->
    <script src="//cdn.jsdelivr.net/npm/docsify-copy-code"></script>
    <!-- Docsify v4 -->
    <script src="//cdn.jsdelivr.net/npm/docsify@4"></script>
  </body>
  </html>
  ```



  
