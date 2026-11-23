# 使用指南

1. 修改 docs 目录中的 Markdown 文件。
2. 在本地预览并检查页面及链接。
3. 提交修改并推送到 main 分支。
4. 在 Actions 页面查看构建与部署状态。
5. 打开网站检查内容是否更新。

## 常用命令

本地预览（改文件会自动刷新浏览器）：

```bash
python -m mkdocs serve
```

构建网站并严格检查警告，警告也会被当作错误处理：

```bash
python -m mkdocs build --strict
```

发布一次更新，只需要这三条命令：

```bash
git add docs/index.md
git commit -m "更新首页内容"
git push origin main
```

## 关于 --strict 的体会

`--strict` 会把警告升级为错误，构建直接失败。第一次用觉得它很烦，因为一个写错的页面链接就能让构建中断；但用下来发现这正是它的价值：**宁可在这里失败，也不要让一个 404 的链接被部署到线上**。这次工作流里也用了 `--strict`，等于把检查放进了 CI，本地忘了检查也拦得住。

## 相关页面

- 返回[首页](index.md)
- 查看[API 参考](api.md)
