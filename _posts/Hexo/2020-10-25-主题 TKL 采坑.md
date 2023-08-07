---
title: 主题 TKL 采坑及进阶
date: 2020-10-25 16:15:32
tags: 
- Hexo
- TKL
categories: 博客搭建
---

### 1.初始化

TKL主题的初始化到本地启动，根据 [TKL 的 README.md](https://github.com/SuperKieran/TKL) 操作即可

### 2.更换背景图

1. 图片放在 `themes\TKL\source\img` 下
2. 更新背景图指向：`themes\TKL\_config.yml` -> `cover: /img/bg_img.jpg` 改成你的图片

### 3.更换logo

同背景图

### 4.更换网页小图标

1. 将自己的图标文件放在 `themes\TKL\source\img` 下，不一定非得 `.ico` 格式才可以
2. 全局查找 `favicon.ico` 的使用处，替换为你想要的图标

### 5.右侧导航栏显示 TAGS

你会发现按照 [TKL 的 README.md](https://github.com/SuperKieran/TKL) 所提示的开启 `TAG`和`CATEGORY` 后，只有 `CATEGORY` 出来了。

- 原因：
    TKL 原代码有误
- 解决：
    `/themes/TKL\layout/_partial/header.ejs` 中 `<!-- Dropdown Menu -->` 的下方合适位置添加代码：
    > 其实就是修改了 该文件下 Categories 相关代码

        <!-- tags -->
        <% if (site.tags.length){ %>
        <li>
            <a class="sb-toggle-submenu">Tags<span class="sb-caret"></span></a>
            <ul class="sb-submenu">
                <% site.tags.sort('name').each(function(item){ %>
                <li><a href="<%- config.root %><%- item.path %>" class="animsition-link"><%= item.name %><small>(<%= item.posts.length %>)</small></a></li>
                <% }); %>
            </ul>
        </li>
        <% } %>

### 6.查找功能

按照 TKL 所提示的 [hexo-generator-search-zip](https://github.com/SuperKieran/hexo-generator-search-zip) 搞完后，public中的 search 相关文件是有了，但是功能怎么出来呢？

很简单，那是因为你没注意看他这个 README.MD 中的  [Guide](https://go.kieran.top/post/45/), 这里边也没什么，就是刚刚做的那些完成后，还有一个 TKL 主题的配置文件要改的，在主题下的 `_config.yml` 修改 `local_search.enable` 为 `true` 就好了

### 7.LINKS

待探索 ···

### 8.About

待探索 ···

### 9.RSS

待探索 ···

### 10.评论功能

待探索 ···

### 11.其他爬坑
#### 1.发布报错 (update 2023.05.10)
``` vim
    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    @    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
    Someone could be eavesdropping on you right now (man-in-the-middle attack)!
    It is also possible that a host key has just been changed.
    The fingerprint for the RSA key sent by the remote host is
    SHA256:uNiVztksCsDhcc0u9e8BujQXVUpKZIDTMczCvj3tD2s.
    Please contact your system administrator.
    Add correct host key in /c/Users/Administrator/.ssh/known_hosts to get rid of this message.
    Offending RSA key in /c/Users/Administrator/.ssh/known_hosts:1
    RSA host key for github.com has changed and you have requested strict checking.
    Host key verification failed.
    fatal: Could not read from remote repository.

    Please make sure you have the correct access rights
    and the repository exists.
    FATAL Something's wrong. Maybe you can find the solution here: https://hexo.io/docs/troubleshooting.html
    Error: Spawn failed
        at ChildProcess.task.on.code (D:\project\keestep\kejur.github.io\node_modules\hexo-util\lib\spawn.js:51:21)
        at emitTwo (events.js:126:13)
        at ChildProcess.emit (events.js:214:7)
        at ChildProcess.cp.emit (D:\project\keestep\kejur.github.io\node_modules\cross-spawn\lib\enoent.js:34:29)
        at Process.ChildProcess._handle.onexit (internal/child_process.js:198:12)
```
`解决方法`：
>删除 `C:\Users\Administrator\.ssh\known_hosts` 中 `github.com`相关内容

>然后重新 `hexo d -g`



---

[TKL作者的blog](https://go.kieran.top/)，以示瞻仰

>祝你生活愉快
