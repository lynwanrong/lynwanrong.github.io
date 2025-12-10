---
title: Github Pages Hexo
date: 2025-12-09 13:47:25
tags:
---

## 安装 fnm 

通过 fnm 管理  Node.js

## 初始化

```bash
npm install hexo-cli hexo 	# 本地安装 Hexo
npx hexo-cli init .			# 初始化 y
npm install					# 安装依赖
npx hexo s					# 测试启动

npm install hexo-deployer-git --save # 安装 Git 部署插件
```

配置 `deploy` 信息

```yaml
# _config.yaml
deploy:
  type: git
  repo: git@github.com:xxxxxx/xxxxx.github.io.git
  branch: main
  name: 
  email: 你的邮箱  
```

部署

```bash
npx hexo cl && npx hexo g -d
```

## 添加 NexT 主题

安装主题

```bash
npm install next-theme/hexo-theme-next --save
```

复制配置文件

```bash
cp node_modules/hexo-theme-next/_config.yml -o _config.next.yml
```

修改配置文件

```yaml
# _config.yml 修改站点基本信息
title:       # 这里会替换掉图中的 "Hexo"
subtitle: ''     # 这里会显示在标题下方（官方图中的 "Theme for Hexo" 位置）
description: ''
keywords:
author:     # 这里会替换掉图中的 "John Doe"
language: zh-CN       # 建议设为中文
timezone: ''

# _config.next.yml 配置菜单
menu:
  home: / || fa fa-home
  # about: /about/ || fa fa-user
  # tags: /tags/ || fa fa-tags
  # categories: /categories/ || fa fa-th
  archives: /archives/ || fa fa-archive
  # schedule: /schedule/ || fa fa-calendar
  # sitemap: /sitemap.xml || fa fa-sitemap
  # commonweal: /404/ || fa fa-heartbeat

# 关键：开启菜单图标显示，否则只会显示文字
menu_settings:
  icons: true
  badges: true
  
# 开启搜索框 需要安装 npm install hexo-generator-searchdb --save
local_search:
  enable: true   # 改为 true
  trigger: auto
  top_n_per_article: 1
  
# 设置头像
avatar:
  url: /images/avatar.jpg	# 放在 source/ 下
  rounded: true   # 如果想要圆形的头像就设为 true
  rotated: false  # 鼠标放上去旋转
```

## 添加评论系统

对于托管在 GitHub Pages 上的博客，**Giscus** 是目前最推荐、最“极客”的评论系统。

### 第一步：准备 GitHub 仓库

Giscus 依赖 GitHub 的 Discussions 功能，默认这个功能在仓库里可能是关闭的。

1. 打开你的博客仓库页面 (`https://github.com/lynwanrong/lynwanrong.github.io`)。
2. 点击上方的 **Settings** (设置)。
3. 在左侧菜单往下找，找到 **Features** (特性) 区域。
4. **勾选 `Discussions`**。
   - *这一步非常关键，如果不开启，评论系统会报错。*

### 第二步：安装 Giscus App

你需要授权 Giscus 访问你的仓库（这样它才能帮你写入评论）。

1. 访问 [Giscus App 页面](https://github.com/apps/giscus)。
2. 点击 **Install**。
3. 选择 **Only select repositories**，然后只选择你的博客仓库 (`lynwanrong.github.io`)。
4. 点击 **Install** 完成授权。

### 第三步：获取配置参数 (Repo ID 和 Category ID)

Giscus 官网提供了一个自动生成配置的工具，这是获取那两个长字符串 ID 最简单的办法。

1. 访问 [giscus.app/zh-CN](https://giscus.app/zh-CN)。
2. 在页面上的配置部分：
   - **仓库**：输入 `lynwanrong/lynwanrong.github.io`。
   - **页面 <-> Discussion 映射关系**：推荐选 `pathname` (根据网址路径匹配)。
   - **Discussion 分类**：选择 `Announcements` (或者你自己去仓库 Discussions 里新建一个分类叫 Comments 也行)。
3. 滚动到页面最下方，你会看到一段生成的代码。**不要复制整个代码**，你只需要记下这两个值：
   - `data-repo-id="R_kgDO..."`
   - `data-category-id="DIC_kwDO..."`

### 第四步：配置 Hexo (Next)

打开你的 **`_config.next.yml`** 文件，进行两处修改。

#### 1. 激活评论系统

找到 `comments:` 模块（或者直接添加）：

```yaml
comments:
  # 开启评论
  active: giscus
  # 隐藏评论框直到点击 (可选，设为 false 就是直接显示)
  storage: true
  lazyload: false
  nav:
    giscus:
      text: Load Giscus
      order: 1
```

#### 2. 填入 Giscus 配置

在文件末尾（或者找个空地）添加 Giscus 的详细配置。请把你在**第三步**里获取到的 ID 填进去：

```yaml
giscus:
  enable: true
  repo: lynwanrong/lynwanrong.github.io
  repo_id: "这里填你的 repo-id"
  category: "Announcements"  # 或者是你在官网选择的其他分类
  category_id: "这里填你的 category-id"
  mapping: "pathname" # 映射方式
  reactions_enabled: 1 # 是否开启表情反应
  emit_metadata: 0
  theme: light # 或者 dark，也可以配置成 auto 跟随系统
  lang: zh-CN
  crossorigin: anonymous
  # input_position: bottom # 评论框在上方还是下方
```

### 第五步：部署并测试

保存文件，执行：

```bash
npx hexo cl && npx hexo g -d
```
