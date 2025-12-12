# 博客模板

基于 Hugo + Stack 主题的静态博客

## 快速开始

### clone项目
```sh
git clone --recursive git@github.com:crackhopper/hugo-blog-templater.git
```

### 安装 Hugo 工具
访问 https://gohugo.io/installation/ 安装 Hugo


### 安装 Python 工具
我们自定义了一些处理脚本，需要用到 Python 环境和 Python 包。
```sh
pip install -r requirements.txt
```

### 修改 `config.toml` 文件
按照自己的博客配置来修改里面的选项。

不懂的可以问AI。

### 配置 Github Pages 仓库 (repo_to_deploy)
前置条件：
1. 在github上创建一个新的仓库，用于部署博客。
2. 仓库名称为 `username.github.io` ，其中 `username` 为你的 GitHub 用户名。
3. 确保仓库是公开的（Public）。
4. 配置仓库为 GitHub Pages 部署。
   - 在仓库设置中，找到 "Pages" 选项。
   - 选择 "Deploy from a branch"。
   - 部署分支选择 `master` 分支。 (注意，这里的名字需要和 .env 中的分支一致)
   - 部署路径选择根目录 `/`。
   - 点击保存。


配置部署仓库：
1. 复制环境变量示例：`cp .env.example .env`（Windows 可使用 `Copy-Item`）
2. 填写 `.env` 中的：
   - `DEPLOY_REPO`：GitHub Pages 仓库地址
   - `DEPLOY_BRANCH`：用于部署的分支
   - `DEPLOY_DIR`：部署子模块目录（默认 `repo_to_deploy`）
3. 调用 `scripts/init-deploy-submodule.ps1` 将 `DEPLOY_DIR` 初始化为 git submodule（若尚未存在）

## 工作流

### 启动开始写作脚本
在项目根目录执行 `.\scripts\start-writing.ps1` 可以一键启动完整的写作环境

原理：
1. 动态检测 contents 目录下的文件变化，以及 静态资源目录 `static/` 下的文件变化。
2. 当检测到文件变化时。使用 python 脚本，处理 obsidian 语法转化为标准markdown语法。写入到临时工作目录 `.hugo_temp_content` 中。
3. 页面会完成刷新。访问 `http://localhost:1313/` 即可查看最新的博客。

### 创建新文章
在 `content` 目录下，对应的子目录创建 Markdown 文件。
- posts/ - 博客文章
- projects/ - 项目
- page/ - 独立页面

### 快键和工具配置
#### 新文章(Front Matter)
使用 `alt + shift + f12` 或 `alt + shift + insert` 插入 `new` 模板。内容如下：

```yaml
---
title: 文章标题
date: 2024-01-01T10:00:00+08:00
tags:
  - 标签1
  - 标签2
draft: false  # true 表示草稿，false 表示已发布
---
```

如果需要更新文章的 Front Matter。再次使用快键，选择 `header` 模板，即可更新 Front Matter。

#### 链接其他文章
使用 templater 工具。 绑定了快键 `alt + e` :
1. 选择 `link-notes`
2. 输入要插入的文章标题 （可以部分输入，会进行检索）
3. 从检索结果中选择要插入的文章。


#### 插入图片
已经配置了插件 `image-converter` 

直接截图后粘贴，即可插入图片。并且支持调整图片大小。

#### 清理未使用图片
安装了 `oz-clear-unused-images` 插件。

在 obsidian 页面左侧菜单栏可以找到对应的按钮，点击后可以清除多余图片。

#### 其他插件
不深入介绍，自己研究
- `OA-file-hider` 可以在obsidian中隐藏不必要的目录。
- `obsidian-heading-shifter` 方便快速调整目录层级
- `templater` 比较强大的模板插件。可以自定义一些模板，方便快速插入内容。（可以访问到obsidian的api）

### 发布文章
发布文章前，需要先配置好部署仓库，参见上面的 #[配置 Github Pages 仓库 (repo_to_deploy)]

运行部署脚本 `.\scripts\deploy.ps1` 即可发布文章到配置的仓库。

等待 github pages 的在线构建完成后，即可访问 xxx.github.io 查看发布的文章。(xxx是你的 github 用户名)


部署原理：

**将生成的静态前端页面直接发布到对应仓库的对应分支，剩下的由github处理** （只要保证构建的结果可以被任意 http server host 即可） 

手动测试构建是否成功：
- 通过进入到发布的目录，使用 `python -m http.server 8000` 启动一个简单的 http server。
- 访问 `http://localhost:8000/` 即可查看发布的文章。
- 确保发布的目录可以推动到github，且推动到正确的分支。且该仓库配置了正确的 github pages 部署。


## 高级：自定义样式
这块主要给 AI Agent 看。AI可以快速帮你调整主题。

主题文件夹中的：
- layouts
- assets
里面的内容，可以按照原始的目录接口，拷贝到我们的目录下。随后可以自定义修改。

## 参考资源

- [Hugo 官方文档](https://gohugo.io/documentation/)
- [Stack 主题文档](https://stack.jimmycai.com/)
- [Hugo 数学公式支持](https://gohugo.io/content-management/mathematics/)
