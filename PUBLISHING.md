# 发文说明

## 推荐方式：命令发布

文章正文可先写在 Markdown 文件中，然后运行：

```powershell
.\scripts\publish.ps1 -Type article -Title "文章标题" -File ".\draft.md" -Category "随笔" -Tags 思考,生活
```

发布一条小记：

```powershell
.\scripts\publish.ps1 -Type note -Content "今天想记录的内容"
```

脚本会自动：

- 更新 `data/articles.json` 或 `data/notes.json`
- 生成独立永久链接
- 生成文章或小记详情页
- 为详情页接入按路径区分的评论

完成后提交并推送到 GitHub，Pages 工作流会重新生成页面并发布。

## 网页编辑器

也可以打开 `editor.html` 编辑并导出 JSON。将导出的文件替换到 `data/` 目录后提交推送即可。文章和小记的新路径会由编辑器自动生成。

## 评论

评论使用 Utterances。正文或小记内容渲染完成后，页面才开始加载评论脚本。仓库需安装 Utterances App，并开启 GitHub Issues。

> 注意：替换 JSON 时必须覆盖整个文件。不要只复制 `"articles": [...]` 这一段追加到旧文件，否则 GitHub Actions 无法解析 JSON。