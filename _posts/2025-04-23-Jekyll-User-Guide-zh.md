---
layout    : post
title     : "如何使用jekyll创建个人博客"
date      : 2025-04-23
lastupdate: 2025-04-23
categories: Jekyll
render_with_liquid: false
cheatsheet: false
---

>  本文简单介绍Jekyll如何安装,及简单使用

## Jekyll 究竟是什么?

Jekyll 是一个简单的博客形态的静态站点生产机器。它有一个模版目录，其中包含原始文本格式的文档，通过一个转换器（如 [Markdown](http://daringfireball.net/projects/markdown/)）和我们的 [Liquid](https://github.com/Shopify/liquid/wiki) 渲染器转化成一个完整的可发布的静态网站，你可以发布在任何你喜爱的服务器上。Jekyll 也可以运行在 [GitHub Page](http://pages.github.com/) 上，也就是说，你可以使用 GitHub 的服务来搭建你的项目页面、博客或者网站，而且是**完全免费**的。

<br>

## 安装

**首先我们来介绍如何在linux中通过容器部署的方式安装Jekyll**

推荐使用 `jekyll:3.6.0` 的镜像

```bash
// 拉取镜像
docker pull jekyll/jekyll:3.6.0

// 建立容器
docker run -itd --name jekyll -p 2222:4000 jekyll/jekyll:3.6.0 /bin/bash

// 进入容器内部
docker exec -it jekyll /bin/bash
bash-4.3#
```

由于`jekyll`的默认目录处于`/home/jekyll/`,所以我们先进入这个目录,进行接下来的操作

```bash
bash-4.3# pwd
/home/jekyll

// 使用jekyll new <dir_name>,可以创建一个拥有默认文件的jekyll目录
bash-4.3# jekyll new myblog 
Running bundle install in /home/jekyll/myblog...
  Bundler: The dependency tzinfo-data (>= 0) will be unused by any of the platforms Bundler is installing for. Bundler isinstalling for ruby but the dependency is only for x86-mingw32, x86-mswin32, x64-mingw32, java. To add those platforms tothe bundle, run `bundle lock --add-platform x86-mingw32 x86-mswin32 x64-mingw32 java`.
  Bundler: Fetching gem metadata from https://rubygems.org/...
  Bundler: Retrying dependency api due to error (2/4): Bundler::HTTPError Network error while fetching https://index.rubygems.org/api/v1/dependencies?gems=jekyll%2Cjekyll-feed%2Cminima%2Ctzinfo-data (Net::OpenTimeout)
  Bundler:
  Bundler:
  Bundler: Resolving dependencies...
  Bundler: Network error while fetching
  Bundler: https://rubygems.org/quick/Marshal.4.8/jekyll-3.6.3.gemspec.rz (Connection reset
  Bundler: by peer - SSL_connect)


// 启动服务,让web可以被访问, 加上`-B`可以后台运行
bash-4.3# jekyll server
Configuration file: /home/jekyll/myblog/_config.yml
            Source: /home/jekyll/myblog
       Destination: /home/jekyll/myblog/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 0.247 seconds.
 Auto-regeneration: enabled for '/home/jekyll/myblog'
    Server address: http://0.0.0.0:4000/
  Server running... press ctrl-c to stop.
```

<br>

**_site**目录在整个jekyll中的拥有很重要的戏份,默认创建时,会被收录到.gitignore文件中,防止使用git提交时受到影响

```bash
bash-4.3# ls -al
total 24
drwxr-sr-x    3 jekyll   jekyll         120 Apr 23 15:40 .
drwxr-sr-x    1 jekyll   jekyll          35 Apr 23 15:40 ..
-rw-r--r--    1 jekyll   jekyll          35 Apr 23 15:40 .gitignore
-rw-r--r--    1 jekyll   jekyll         398 Apr 23 15:40 404.html
-rw-r--r--    1 jekyll   jekyll         937 Apr 23 15:40 Gemfile
-rw-r--r--    1 jekyll   jekyll        1652 Apr 23 15:40 _config.yml
drwxr-sr-x    2 jekyll   jekyll          51 Apr 23 15:40 _posts
-rw-r--r--    1 jekyll   jekyll         539 Apr 23 15:40 about.md
-rw-r--r--    1 jekyll   jekyll         213 Apr 23 15:40 index.md

bash-4.3# cat .gitignore
_site
.sass-cache
.jekyll-metadata
```

<br>
<br>
<br>

## Jekyll 目录结构及作用

![1](/assets/img/jekyll-user-guide/jekyll-1.png)

<br>

### **核心目录**

| 目录/文件                     | 作用                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| **`_config.yml`**             | 主配置文件，定义网站全局变量（如标题、URL、插件等）。        |
| **`_posts/`**                 | 存放博客文章，文件命名格式为 `YYYY-MM-DD-title.md`（如 `2023-10-01-hello-world.md`）。 |
| **`_drafts/`**                | 存放未发布的草稿文章（不会自动构建，需通过 `--drafts` 参数预览）。 |
| **`_layouts/`**               | 存放 HTML 模板（如 `default.html`、`post.html`），通过 `layout: 模板名` 调用。 |
| **`_includes/`**              | 存放可复用的代码片段（如 `header.html`、`footer.html`），通过 `{% include file.html %}` 引入。 |
| **`_data/`**                  | 存放结构化数据（YAML/JSON/CSV），如 `authors.yml`，可通过 `site.data.authors` 访问。 |
| **`_site/`**                  | Jekyll 生成的静态网站（默认输出目录，可修改 `destination` 配置）。 |
| **`assets/`**                 | 存放 CSS、JS、图片等资源，会被复制到 `_site` 中。            |
| **`index.md` / `index.html`** | 网站首页，可使用 Liquid 模板和 Front Matter（YAML 头信息）。 |

<br>
<br>

## 核心配置配置详解

<br>

### 主配置文件(_config.yml)

<br>

_config.yml是Jekyll网站的核心配置文件，它决定了网站的基本行为和全局设置。这个YAML格式的文件主要有以下功能：

* **网站基本信息**：配置网站标题、描述、URL等元数据**
* **构建选项**：设置Markdown解析器、代码高亮等构建参数**
* **插件管理**：指定要加载的插件和扩展
* **变量定义**：创建可在全站使用的全局变量
* **目录结构**：自定义源文件和输出目录

修改此文件后需要重启Jekyll服务才能生效。

<br>
<br>

### 文章仓库 (_posts)

核心作用

`_posts` 是 Jekyll 的核心目录之一，专门用于存放博客文章内容。

```bash
bash-4.3# ls
2025-04-23-welcome-to-jekyll.markdown
bash-4.3# pwd
/home/jekyll/myblog/_posts
bash-4.3# ls
2025-04-23-welcome-to-jekyll.markdown
```

![4](/assets/img/jekyll-user-guide/jekyll-3.png)

**文件命名规则**

- 必须遵循格式：`YYYY-MM-DD-标题.md`
- 日期和标题通过连字符连接
- 文件扩展名通常为 `.md` 或 `.html`

**文件内容**

每个文件包含：

1. 开头的 YAML 头信息（Front Matter）
2. 使用 Markdown 或 HTML 编写的内容主体

**生成规则**

Jekyll 会自动：

- 按日期排序文章
- 将文件转换为静态页面
- 生成永久链接（URL）

这个目录使博客文章管理变得简单高效，是 Jekyll 静态博客系统的核心功能之一。

<br><br>

### 页面元数据(FrontMatter)

FrontMatter 是一种元数据声明，通常以 `YAML` 或 `TOML` 格式编写，用于定义文章的基本信息。

<br>

任何内容文件开头可添加YAML头信息：

```yaml
---
# 文档元数据配置区（由三重短横线界定）
layout: post            # 必需字段，指定模板类型（必须首位声明）
title: "Jekyll入门指南"  # 文档标题（必填项）
date: 2023-10-15        # 发布时间（可选，支持手动覆盖）
categories: [教程, 技术] # 内容分类体系（支持多级分类）
tags: [jekyll, 静态网站] # 内容标签（支持多标签标注）
permalink: blog/title/  # 固定URL路径（避免因日期/分类变更导致链接失效）
---
```

<br>

**permalink另外一种用法 ** 

```yaml
permalink: /:categories/:day
```

**参数说明：**

1. **动态路径变量**：

   - `:categories`：自动替换为文档配置的类别层级路径
   - `:day`：提取文档日期中的"日"部分（两位数格式）

2. **高级特性**：

   - 支持Jekyll内置的

     日期格式化变量：

     - `:year` - 四位年份（如2023）
     - `:month` - 两位月份（01-12）
     - `:day` - 两位日期（01-31）
     - `:title` - 文档标题（需URL编码）

3. **最佳实践建议**：

   - 建议在`_config.yml`中设置全局默认模板
   - 文档级配置会覆盖全局设置
   - 使用`pretty`模式自动添加尾部斜杠

4. **示例配置**：

   ```yaml
   # 全局默认配置（_config.yml）
   defaults:
     - scope:
         path: ""
       values:
         permalink: /blog/:year/:month/:title/
   ```

<br>

## 如何编写文章

<br>

首先，在每篇文章的开头，建议添加专属的 **FrontMatter**, 例如:

```yaml
---
layout    : post
title     : "Install kubernetes 1.32.3 version with kubeadm"
date      : 2025-04-22
lastupdate: 2025-04-22
categories: kubernetes
---
```

然后，在正文部分，你可以像在 **Markdown 文件** 中一样，对内容进行灵活处理。无论是添加 **加粗** 、*斜体* ，还是插入代码块或列表，Markdown 都能让你轻松实现格式化需求。


<br>

<br>

看完上面的内容,相信你有了一定的了解,快去试试吧!