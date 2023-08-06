---
title: Hexo的详细使用及采坑见闻
date: 2020-10-23 01:29
tags: Hexo
categories: 博客搭建
---


> 原文 [Github Pages部署个人博客搭建](https://sisibeloved.github.io/hexoBlog/2018/04/12/Github-Pages%E9%83%A8%E7%BD%B2%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2-Hexo%E7%AF%87/#%E4%BF%AE%E6%94%B9%60_config.yml%60)


---

## 前言
---
想在Github上搭建一个个人博客搭建，在网上找了不少的文章，但有的是使用的旧版本，有的语焉不详，最后还是磕磕绊绊地搭起来了，因此写了这篇文章，对自己踩过的坑进行一个总结。水平有限，还请见谅。

## 系统环境
---
- Windows 10 教育版
- Node.js v8.7.0
- Npm v5.4.2
- Git v2.13.0

## 在GitHub上创建Github Pages项目
---
### 1. 创建新仓库
Github Pages分为两类，用户或组织主页，项目主页。

创建用户或组织主页，只需创建一个名称为{yourusername}.github.io的新仓库即可。这边的yourusername填写自己的用户名。Github会识别并自动将该仓库设为Github Pages。用户主页是唯一的，填其他名称只会被当成普通项目。
创建项目主页。先新建一个仓库，名称随意，或是使用原有的仓库都可以。在项目主页 -> Settings -> Options -> Github Pages中，将Source选项置为master branch，然后Save，这个项目就变成一个Github Pages项目了。

### 2. 分支管理
Github Pages会自动部署静态网页文件，而上一步是将master分支作为部署的默认分支。

Github Pages部署分支设置中，可以有三种设置：

- master 分支
- master 分支下的/doc文件夹
- gh-pages 分支
**其中gh-pages分支的选项需要创建这个分支才会显示出来。**

我个人是这样设置分支的：
新建一个blog-src分支用来管理Hexo的源代码，
gh-pages分支用来管理Hexo生成的静态网页文件，即部署到Github Pages上的文件，
master分支保留（个人习惯）。
你也可以另开一个项目用来管理Hexo源代码的版本。

## 安装Hexo
统一说明一下以下的代码示例，<>中的是必填参数，[]中的是选填参数。

### 1. 安装Hexo
> npm install -g hexo-cli

### 2. 生成Hexo项目
在你想创建博客搭建的文件夹中初始化Hexo。

> hexo init [projectname]

如果带了项目名称，会生成一个带有该名称的文件夹；如果没带参数，则必须在空文件夹下运行，不然会报错。

### 3. 拉取Github项目到本地
> git clone https://github.com/yourusername/yourprojectname.git

然后把之前生成的Hexo项目文件夹下的内容全部复制过来。关于Git的使用请自行掌握，因为贴Git的代码很容易引起各种各样的错误。最后把项目push到blog-src分支上（换成你自己的源码分支）。

## 使用Hexo

### 1. 常用命令
- hexo generate [-d]
- hexo serve [-p port]
- hexo deploy [-g]
Hexo命令大多可以缩写，如`hexo serve --port 5000`可以缩写成`hexo s -p 5000`。

更多命令和参数可参阅[官方文档](https://hexo.io/zh-cn/docs/commands.html)。

### 2. 编译
[可选]
清除缓存：

> hexo clean

编译：

> hexo g

### 3. 运行
启用服务：

> hexo s

默认启动地址为http://localhost:4000/

如果4000端口被占用：

> hexo s -p [port]

### 4. 部署
在博客搭建所在文件夹下：

``` s
npm install hexo-deployer-git --save
```

这是用npm安装hexo部署到git的插件，--save会把这个包写入到package.json，切换到其他电脑时可以不需要重新安装。

#### 修改_config.yml：

``` yaml
deploy:
  type: git
  repo: git@github.com:yourusername/yourprojectname.git
  branch: gh-pages
```

将repo和branch换成自己的。

##### 注意：

1.repo可以在Github上复制，但记得选 `Clone with SSH` ，只能走Git协议，走HTTPS协议会报错

2.branch选择Github Pages中设置的那个分支，而不是拉取这个项目的分支

## 博文写作
### 项目结构

``` tree
.
├── .deploy
├── public
├── scaffolds
|   ├── draft.md
|   ├── page.md
|   └── post.md
├── scripts
├── source
|   ├── _drafts
|   └── _posts
├── themes
├── _config.yml
└── package.json
```
scaffolds：脚手架，即对应的layout生成新博文时的头信息模板

source：源文件，博文会根据layout分层放置

关于添加模板和草稿参看写作 | Hexo。

### 新建博文

> hexo n [layout] <filename>

#### 头信息
post的模板md：

``` yaml
---
title: {{ title }}
date: {{ date }}
tags:
---
```

实际生成的md：

``` yaml
---
title: RocketMQ安装及部署
date: 2018-04-09 10:04:41
tags:
---
```

时间格式在_config.yml中设置，Hexo会根据时间来解析博文。

### 更换主题
Hexo自带的默认主题是landscape，不过我们可以从Theme | Hexo或Github上找到喜欢的主题。

#### 更换主题的三种方法
从Github主页下载release包，解压到themes/文件夹下

``` s
$ git clone --branch v6.0.0 https://github.com/theme-next/hexo-theme-next themes/next
```
(仅限linux)

``` s
$ mkdir themes/next
$ curl -s https://api.github.com/repos/theme-next/hexo-theme-next/releases/latest | grep tarball_url | cut -d '"' -f 4 | wget -i - -O- | tar -zx -C themes/next --strip-components=1
```
设置主题
在项目的_config.yml中，将theme：改成themes/下对应文件夹的名称。如果是解压出来的“hexo-theme-next-6.1.0”就写“hexo-theme-next-6.1.0”，如果是自建的“next”就写“next”。

#### 更换Next主题
Next是Hexo最受欢迎的主题之一，不过我在使用的时候发现Github主页上v6.0.0的文档并不详尽，官网也还没上线，旧有的文档可以借鉴，但有些小地方还是会有差别。下面用v5和v6来区分新版本。

v5的文档地址

旧的主页（更新到v5.1.2）:
https://github.com/iissnan/hexo-theme-next

新的主页（v6.0.0之后从这儿开始更新）:
https://github.com/theme-next/hexo-theme-next

#### 配置Next主题
Next版本 ： v6.1.0

修改主题的_config.yml（注意区分项目和主题）。整个文件有一千行，各种配置项非常丰富，而且配上了大量的注释，我也只是挑了一小部分，大家可以按需修改。

切换风格
官方提供了四种风格，各有千秋。

``` yaml
# ---------------------------------------------------------------
# Scheme Settings
# ---------------------------------------------------------------

# Schemes
#scheme: Muse
#scheme: Mist
#scheme: Pisces
scheme: Gemini
```


#### 语言选择
在项目的_config.yml中修改language选项：

``` yaml
# 写法1
language: zh-CN
# 写法2，会使用最上面一种语言
language:
- zh-CN
- _en
```
与v5不同的是语言的写法，简体中文是zh-CN，英文是_en。语言文件位于主题的language/文件夹下，对应的yml文件的名称就是语言的名称，可以修改对应的语言文件来修改一些指定字段的表述，也可以自定义对应语言的字段。可以参考国际化 | Hexo。

#### 社交页面
按需要解除注释并修改。下方的icons_only是’只显示图标’，我觉得比较简洁就改为true了。

``` yaml

# Social Links.
# Usage: `Key: permalink || icon`
# Key is the link label showing to end users.
# Value before `||` delimeter is the target permalink.
# Value after `||` delimeter is the name of FontAwesome icon. If icon (with or without delimeter) is not specified, globe icon will be loaded.
social:
  GitHub: https://github.com/yourname || github
  E-Mail: yourname@youremail.com || envelope
  #Google: https://plus.google.com/yourname || google
  #Twitter: https://twitter.com/yourname || twitter
  #FB Page: https://www.facebook.com/yourname || facebook
  #VK Group: https://vk.com/yourname || vk
  #StackOverflow: https://stackoverflow.com/yourname || stack-overflow
  #YouTube: https://youtube.com/yourname || youtube
  #Instagram: https://instagram.com/yourname || instagram
  #Skype: skype:yourname?call|chat || skype

social_icons:
  enable: true
  icons_only: true
  transition: false
  # Dependencies: exturl: true in Tags Settings section below.
  # To encrypt links above use https://www.base64encode.org
  # Example encoded link: `GitHub: aHR0cHM6Ly9naXRodWIuY29tL3RoZW1lLW5leHQ= || github`
  exturl: false

# Follow me on GitHub banner in right-top corner.
# Usage: `permalink || title`
# Value before `||` delimeter is the target permalink.
# Value after `||` delimeter is the title and aria-label name.
#github_banner: https://github.com/yourname || Follow me on GitHub
```


## 常见问题

### 1. Github发送邮件 - 编译未通过

The page build failed for the master branch with the following error:

The tag fancybox on line 77 in themes/landscape/README.md is not a recognized Liquid tag.

见修改_config.yml
选择Github Pages分支
不要使用Git直接推送，应该在_config.yml中设置deploy的配置，然后使用 `hexo d -g` 命令进行推送

### 2. hexo d -g 失败

error: failed to execute prompt script (exit code 1)
fatal: could not read Username for 'https://github.com': No error
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
见修改_config.yml
将HTTPS协议改成Git协议，注意github.com后面，HTTPS是/，Git是:(没错就是冒号)

### 3. hexo d -g 自定义域名失效
很简单！在 source 下创建文件 CNAME , 填上你的域名即可
`文件名不需要后缀；域名不需要http，www..`

![CNAME](/img/cname.jpg)

