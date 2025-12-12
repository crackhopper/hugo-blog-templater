# AI Agent 开发指南

本文档为 AI Agent 提供项目开发指南，帮助快速理解和修改项目。

## 自定义主题的方式

**重要原则：不要直接修改主题仓库（`themes/Stack/`），而是在项目根目录的对应路径创建文件来覆盖主题配置。**

### 工作原理

Hugo 的模板查找顺序是：
1. 首先查找项目根目录的 `layouts/`、`assets/`、`static/` 等目录
2. 如果找不到，再查找主题目录 `themes/Stack/` 中的对应文件

因此，在项目根目录创建与主题目录结构相同的文件，可以覆盖主题的默认配置。

### 目录结构对应关系

```
项目根目录/              →  覆盖  →  主题目录/
├── layouts/             →        →  themes/Stack/layouts/
│   ├── _default/       →        →  themes/Stack/layouts/_default/
│   ├── partials/       →        →  themes/Stack/layouts/partials/
│   └── index.html      →        →  themes/Stack/layouts/index.html
├── assets/             →        →  themes/Stack/assets/
└── static/             →        →  themes/Stack/static/
```

### 实际示例

#### 示例 1：覆盖数学公式渲染组件

**需求**：修改数学公式渲染逻辑，使其支持在文章列表摘要中显示数学公式。

**步骤**：
1. 找到主题中的文件：`themes/Stack/layouts/partials/article/components/math.html`
2. 在项目根目录创建对应路径：`layouts/partials/article/components/math.html`
3. 复制主题文件内容并修改
4. 保存后，Hugo 会优先使用项目根目录的文件

**文件路径**：
- 主题文件：`themes/Stack/layouts/partials/article/components/math.html`
- 覆盖文件：`layouts/partials/article/components/math.html`

#### 示例 2：覆盖列表页面模板

**需求**：在列表页面添加数学公式支持。

**步骤**：
1. 找到主题中的文件：`themes/Stack/layouts/_default/list.html`
2. 在项目根目录创建：`layouts/_default/list.html`
3. 复制并修改内容

**文件路径**：
- 主题文件：`themes/Stack/layouts/_default/list.html`
- 覆盖文件：`layouts/_default/list.html`

### 最佳实践

1. **先查看主题文件**：在修改前，先查看 `themes/Stack/` 中对应的文件，了解其结构和功能
2. **保持目录结构一致**：确保覆盖文件的目录结构与主题中的目录结构完全一致
3. **只修改需要的部分**：可以只复制需要修改的部分，其他部分继续使用主题的默认配置
4. **记录修改原因**：在文件中添加注释，说明为什么需要覆盖这个文件

### 当前项目中的覆盖文件

以下文件已经覆盖了主题的默认配置：

- `layouts/partials/article/components/math.html` - 扩展了数学公式渲染范围，支持文章列表摘要
- `layouts/_default/list.html` - 添加了数学公式支持
- `layouts/index.html` - 添加了数学公式支持
- `layouts/partials/article-list/compact.html` - 自定义了文章列表的摘要显示

### 注意事项

1. **不要修改 `themes/Stack/` 目录**：这是主题仓库，修改后无法通过 git submodule 更新
2. **使用 git 跟踪覆盖文件**：覆盖文件应该提交到项目仓库，而不是主题仓库
3. **主题更新**：当主题更新时，覆盖文件不会受到影响，但需要检查是否有新的功能或修复需要同步

## 项目结构

- `content/` - 博客内容（Markdown 文件）
- `layouts/` - 自定义模板文件（覆盖主题）
- `assets/` - 自定义资源文件（CSS、JS 等）
- `static/` - 静态文件（图片等）
- `themes/Stack/` - Stack 主题（git submodule，不要直接修改）
- `config.toml` - Hugo 配置文件

## 常用修改场景

### 修改页面布局
在 `layouts/` 目录下创建对应的模板文件，覆盖主题的默认布局。

### 修改样式
在 `assets/css/` 或 `assets/scss/` 目录下创建自定义样式文件，并在 `config.toml` 中引用。

### 添加自定义功能
在 `layouts/partials/` 目录下创建自定义组件，然后在需要的地方引用。

