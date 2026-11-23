# 我的技术文档

欢迎访问我的个人文档网站。本网站使用 MkDocs 构建，通过 GitHub Actions 自动部署到 GitHub Pages。

- 作者：humanguest2
- 实践主题：静态文档自动部署
- [关于本项目](about.md)
- [使用指南](guide.md)
- [API 参考](api.md)

## 为什么想做一个文档网站

平时写课程笔记和项目说明，内容散落在好几个 Markdown 文件里，想给别人看只能一个个发文件，对方还得自己找该先读哪个。做成网站以后，所有页面有统一的导航和一个固定不变的网址，我改完推送上去，链接不用重新发，看到的就是最新内容。

## 这个网站是怎么运转的

1. 我修改 `docs/` 目录里的 Markdown 文件；
2. 推送到 `main` 分支后，GitHub Actions 自动安装依赖并运行 `mkdocs build`；
3. 构建产物 `site/` 被打包成 artifact，再由 `actions/deploy-pages` 发布到 GitHub Pages；
4. 浏览器打开的就是最近一次构建的结果。

| 我关心的环节 | 对应位置 |
| --- | --- |
| 写内容 | `docs/*.md` |
| 控制导航和主题 | `mkdocs.yml` |
| 自动构建与部署 | `.github/workflows/deploy.yml` |
| 最终网站 | 本页所在地址 |

## 文档目录

- [关于本项目](about.md)：项目目标与技术组成
- [使用指南](guide.md)：本地预览与发布流程
- [API 参考](api.md)：仅用于练习的接口文档示例

自动部署验证：这是我第二次推送后更新的内容。
