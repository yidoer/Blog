# 人间浊物 琅上清书

一个典雅、轻量、易维护的纯静态个人博客，支持 Markdown 文章编辑、双主题切换、评论系统和图形化编辑后台。

## 特性

- 📄 **纯静态架构** — 无需服务器，无需数据库，部署到 GitHub Pages 即可运行
- 📝 **Markdown 支持** — 文章使用 Markdown 格式，支持标题、粗体、斜体、引用、列表、链接、图片、代码、表格等
- 🎨 **双主题切换** — 浅色（宣纸）与深色（墨色）两种主题，支持自动保存偏好
- 💬 **评论系统** — 集成 Giscus（基于 GitHub Discussions），访客可使用 GitHub 账号评论
- 🖼️ **图片支持** — 编辑后台支持图片上传，自动转为 base64 嵌入文章
- ✏️ **图形化编辑后台** — 浏览器内即可新建、编辑、删除文章和小记，数据导出为 JSON
- 📱 **响应式设计** — 适配桌面、平板、手机等多种设备
- ⚡ **性能优化** — 无框架依赖，无外部 CDN（除 Google Fonts 和 Giscus），加载速度快
- 🖨️ **打印友好** — 文章页支持打印，自动隐藏导航和评论

## 目录结构

```
.
├── index.html          # 首页（封面 + 最新内容）
├── notes.html          # 小记列表页
├── articles.html       # 文章列表页（支持标签筛选）
├── article.html        # 文章详情页（Markdown 渲染 + 评论）
├── about.html          # 关于页面
├── editor.html         # 图形化编辑后台
├── css/
│   └── style.css       # 主题样式（含 CSS 变量系统）
├── js/
│   ├── app.js          # 核心逻辑（Markdown 解析、主题切换、数据加载）
│   └── editor.js       # 编辑后台逻辑
├── data/
│   ├── articles.json   # 文章数据
│   └── notes.json      # 小记数据
└── images/             # 图片目录（可存放文章中的图片）
```

## 快速开始

### 1. 本地预览

由于博客使用 `fetch` 加载 JSON 数据，需要使用本地服务器打开（不能直接用 `file://` 打开）。

**方式一：Python**
```bash
# Python 3
python -m http.server 8000

# Python 2
python -m SimpleHTTPServer 8000
```

**方式二：Node.js**
```bash
npx serve .
```

**方式三：VS Code**
安装 Live Server 插件，右键 `index.html` → "Open with Live Server"。

然后访问 `http://localhost:8000` 即可预览。

### 2. 配置评论系统

博客默认集成了 Giscus 评论系统，需要配置你的 GitHub 仓库：

1. 确保你的博客仓库是 **公开的（Public）**
2. 进入仓库设置 → **Discussions** → 启用 Discussions
3. 在仓库中创建一个 **Discussion 分类**（如 "Announcements"）
4. 访问 [Giscus 配置页面](https://giscus.app/zh-CN)，填写你的仓库信息，获取配置参数
5. 打开 `article.html`，修改 `GISCUS_CONFIG` 中的值：

```javascript
window.GISCUS_CONFIG = {
  repo: '你的用户名/你的仓库名',  // 例如：zhangsan/blog
  repoId: '',                    // 可选，Giscus 会自动获取
  category: 'Announcements',    // 你的 Discussion 分类名
  categoryId: '',               // 可选，Giscus 会自动获取
  mapping: 'pathname'           // 使用页面路径作为评论标识
};
```

### 3. 部署到 GitHub Pages

**方式一：GitHub Actions（推荐）**

1. 将本仓库所有文件推送到 GitHub 仓库
2. 进入仓库 → **Settings** → **Pages**
3. **Source** 选择 "GitHub Actions"
4. GitHub Actions 会自动运行，将博客部署到 `https://你的用户名.github.io/你的仓库名`

**方式二：直接部署（更简单）**

1. 将本仓库所有文件推送到 GitHub 仓库
2. 进入仓库 → **Settings** → **Pages**
3. **Source** 选择 "Deploy from a branch"
4. **Branch** 选择 `main`（或 `master`），文件夹选择 `/(root)`
5. 保存后等待几分钟，访问 `https://你的用户名.github.io/你的仓库名`

### 4. 绑定自定义域名（可选）

1. 在仓库根目录创建 `CNAME` 文件，写入你的域名，如 `blog.example.com`
2. 在你的域名 DNS 服务商处添加 CNAME 记录，指向 `你的用户名.github.io`
3. 等待 DNS 生效即可

## 使用编辑后台

1. 在浏览器中打开 `editor.html`（需使用本地服务器）
2. 在 **文章管理** / **小记管理** 标签页中新建、编辑、删除内容
3. 编辑时支持：
   - Markdown 语法高亮与实时预览
   - 图片上传（点击上传或拖拽到编辑器）
   - 常用 Markdown 工具栏快捷插入
4. 编辑完成后，切换到 **导出/导入** 标签页，导出 `articles.json` 和 `notes.json`
5. 用导出的文件替换 `data/` 目录下的同名文件
6. 提交到 GitHub，等待自动部署

> ⚠️ **提示**：编辑后台的数据临时保存在浏览器 `localStorage` 中。如果清除浏览器数据，未导出的修改将丢失。请务必定期导出备份！

## 写作指南

### 文章格式（`data/articles.json`）

```json
{
  "articles": [
    {
      "id": "article-001",
      "title": "文章标题",
      "date": "2024-12-25",
      "category": "随笔",
      "tags": ["标签1", "标签2"],
      "excerpt": "文章摘要，显示在列表页",
      "content": "# Markdown 内容\n\n正文..."
    }
  ]
}
```

### 小记格式（`data/notes.json`）

```json
{
  "notes": [
    {
      "id": "note-001",
      "date": "2024-12-25",
      "content": "小记内容，支持纯文本..."
    }
  ]
}
```

### 图片处理建议

- 编辑后台支持将图片转为 **base64** 直接嵌入 Markdown
- 对于较小的图片（< 200KB），base64 嵌入是方便的选择
- 对于较大的图片，建议：
  1. 将图片放入 `images/` 目录
  2. 在 Markdown 中使用相对路径引用：`![描述](images/photo.jpg)`
  3. 提交图片文件到 GitHub 仓库

## 自定义

### 修改主题色

编辑 `css/style.css` 中的 CSS 变量：

```css
:root {
  --accent-color: #4a6741;    /* 强调色（松绿） */
  --accent-light: #7a9f7a;    /* 浅强调色 */
  --accent-dark: #3a5233;     /* 深强调色 */
}
```

### 修改个人信息

编辑 `about.html` 中的内容。

### 修改站点名称

在 `index.html` 的 `<title>` 和 `.hero-title` 中修改。

### 修改字体

博客默认使用 Google Fonts 的 **Noto Serif SC**（宋体）和 **Noto Sans SC**（黑体）。

如需更换字体，修改 `css/style.css` 中的 `--font-serif` 和 `--font-sans` 变量，并在 HTML 中引入对应的字体。

如需离线使用字体，可将字体文件放入 `fonts/` 目录，使用 `@font-face` 引入。

## 浏览器兼容性

- Chrome 80+
- Firefox 75+
- Safari 13.1+
- Edge 80+

## 技术栈

- 纯 HTML5 + CSS3 + JavaScript（无框架）
- 自定义轻量 Markdown 解析器
- CSS 变量主题系统
- localStorage 数据持久化
- GitHub Pages 静态托管
- Giscus 评论系统

## 许可

MIT License — 可自由使用、修改和分发。

---

*以墨为友，以书为伴。*
