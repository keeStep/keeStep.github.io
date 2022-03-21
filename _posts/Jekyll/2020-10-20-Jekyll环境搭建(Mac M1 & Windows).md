---
layout: post
comments: true
title: Jekyll 环境搭建(Mac M1 & Windows)
date: 2022-03-20
categories: test
tags: Jekyll 
---

## 1.Mac M1 安装

> 编辑于 2022-03-20

- Homebrew
- rbenv
- Ruby
- jekyll
- Run

### 1.1.安装 Homebrew
> 如果你之前折腾过 Mac M1 的 Homebrew，这一步可以跳过；

- 校验方式：

``` bash
$ which brew
# 路径在下面即可
/opt/homebrew/bin/brew
```

- 安装方式

``` bash
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 1.2.使用 Homebrew 安装 rbenv

> 安装

``` bash
$ brew install rbenv
```

> 修改环境变量

将下述的环境变量添加到~/.zshrc的末尾

``` vi
# ~/.zshrc
# rbenv
export RBENV_ROOT=/opt/homebrew/opt/rbenv
export PATH=$RBENV_ROOT/bin:$PATH
eval "$(rbenv init -)"
# Use native openssl libraries for building
export PATH="/opt/homebrew/opt/openssl@1.1/bin:$PATH"
export LDFLAGS="-L/opt/homebrew/opt/openssl@1.1/lib"
export CPPFLAGS="-I/opt/homebrew/opt/openssl@1.1/include"
export PKG_CONFIG_PATH="/opt/homebrew/opt/openssl@1.1/lib/pkgconfig"
export RUBY_CONFIGURE_OPTS="--with-openssl-dir=/opt/homebrew/opt/openssl@1.1"
```

运行source ~/.zshrc或者重启Terminal来使环境变量生效

> 安装可能会遇到下面报错

``` bash
Error: Failure while executing; tar --extract --no-same-owner --file /Users/kee/Library/Caches/Homebrew/downloads/c56222a07033b84ad671913218ead61e3ce0891661374516ea1989d722a8dca2--m4-1.4.18.arm64_big_sur.bottle.1.tar.gz --directory /private/tmp/d20220320-52061-qvmfcj exited with 1. Here’s the output: tar: Error opening archive: Failed to open ‘/Users/kee/Library/Caches/Homebrew/downloads/c56222a07033b84ad671913218ead61e3ce0891661374516ea1989d722a8dca2–m4-1.4.18.arm64_big_sur.bottle.1.tar.gz’
```

这是由于新版Homebrew的已经去除Bintray相关请求，而国内镜像还没有去除这块，所以需要删除你/系统设置的Homebrew的国内镜像

- 临时删除

> export HOMEBREW_BOTTLE_DOMAIN=''

- 永久删除

> 修改文件 ~/.zprofile 中相关属性值

### 1.3.安装 Ruby

下面三个都执行（版本号以 2.7.2为例）

``` bash
rbenv install 2.7.2
rbenv global 2.7.2
rbenv rehash
```

安装结果验证

``` bash
ruby -v
```

### 1.4.安装 Jekyll

``` bash
gem install jekyll bundler jekyll-paginate
```

### 1.5 初始化Jekyll项目或克隆已有项目

初始化Jekyll项目

``` bash
jekyll new myblog
```

克隆已有项目

``` bash
# 比如
git clone https://github.com/keeStep/keeStep.github.io.git
```

### 1.6.启动项目

- 进入项目根目录

``` bash
cd myblog
```

- 构建网站并启动服务

``` bash
bundle exec jekyll serve 或 jekyll serve
```

> bundle exec 会在当前项目依赖的上下文环境中执行命令。
> - 相同的 gem
> - 相同的依赖关系
> - 相同的版本号

- 启动完成会提示项目访问地址，点击访问即可

如： 127.0.0.1:4000

![image](/assets/img/jekyll-run.png)

---

## 2.Windows 安装

> 编辑于 2020-10-20

### 2.1.安装 Ruby 环境

注意勾选msys2, 不然后面jekyll会安装不成功

### 2.2.安装 RubyGems

Windows中下载ZIP (下载页面 [https://rubygems.org/pages/download](https://rubygems.org/pages/download))，下载后解压到任意路径。
打开Windows的cmd界面，输入命令： $ cd 解压的路径

### 2.3.切换镜像源

gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/

gem sources -l

### 2.4.安装 Jekyll

gem install jekyll

### 2.5.安装 jekyll-paginate

gem install jekyll-paginate

验证 jekyll :  jekyll -v

### 2.6.安装 Bundler

gem install bundler

安装 一个名为 Bundler 的程序 —— 用于自动安装其他所需的程序

### 2.7.本地启动服务

在命令行中切换到你的网站仓库内

bundle install（这一步不要）

jekyll serve

### 2.8.查看网站

 127.0.0.1:4000 或 localhost:4000

注意：如端口被占用修改端口 jekyll serve -P 5555

