# 实践项目2：个人实验报告

## 1. 基本信息

- GitHub 登录名：humanguest2
- 个人仓库地址：<https://github.com/humanguest2/my-docs>
- 最终网站地址：<https://humanguest2.github.io/my-docs/>

## 2. 实践过程与关键截图

### 2.1 仓库创建

在 GitHub 右上角 `+` → `New repository` 新建仓库，Owner 选择本人账号 `humanguest2`，仓库名 `my-docs`，可见性选择 **Public**，默认分支保持 `main`。

选择 Public 是硬性要求：GitHub Pages 对私有仓库的站点访问有限制，公开仓库才能让教师直接打开网站核验。仓库名和 `mkdocs.yml` 里的 `site_url` 必须对应，否则页面里的资源路径会出错。

![个人公开仓库首页，可见所有者 humanguest2、仓库名 my-docs 和 Public 标识](images/01-repository.png)

### 2.2 项目结构与 MkDocs 配置

仓库分为三个部分，各管一件事：

- `docs/` 存放网站**内容**，即四页 Markdown，是唯一需要我手工编写的目录；
- `mkdocs.yml` 里的 `nav` 决定**导航结构和顺序**，路径相对于 `docs/`，所以写 `index.md` 而不是 `docs/index.md`；
- `site/` 是 `mkdocs build` 生成的**构建产物**，会被上传到 Pages，它由源码生成，因此写进 `.gitignore`，不提交。

![项目目录结构与 mkdocs.yml 配置](images/02-structure.png)

### 2.3 本地构建或预览

创建虚拟环境并安装依赖后执行严格构建：

```bash
python -m venv .venv
source .venv/Scripts/activate
python -m pip install -r requirements.txt
python -m mkdocs build --strict
```

`--strict` 让警告也变成错误，可以提前拦住失效的页面链接。本地预览用 `python -m mkdocs serve`，访问 `http://127.0.0.1:8000/`。

我检查的内容：四页导航能否互相跳转、`about.md` 和 `api.md` 的表格是否正常渲染、`api.md` 的 JSON 代码块是否有高亮、中文是否正常显示。

![本地构建成功的终端输出](images/03-build.png)

### 2.4 Pages 设置

在仓库 `Settings` → `Pages` → `Build and deployment` → `Source` 选择 **GitHub Actions**。

这一步必须在第一次推送**之前**完成。如果仍是默认的 `Deploy from a branch`，工作流里的部署步骤会因为 Pages 尚未按 Actions 方式启用而失败。选择 GitHub Actions 后不需要 `gh-pages` 分支，也不需要开启创建或批准 PR 的权限。

![Pages 发布来源选择 GitHub Actions](images/04-pages-setting.png)

### 2.5 Actions 首次构建和部署

推送后工作流 `Deploy MkDocs to GitHub Pages` 自动触发，`build` 与 `deploy` 两个任务都成功。

`build` 负责取源码、装 Python 3.12、安装依赖、`mkdocs build --strict`，并把 `site/` 上传为 artifact；`deploy` 通过 `needs: build` 等待构建成功，再凭 `pages: write` 和 `id-token: write` 权限把 artifact 发布到 `github-pages` 环境。

![Actions 运行记录，build 与 deploy 均成功](images/05-actions-success.png)

### 2.6 网站访问与四页导航检查

网站地址为 <https://humanguest2.github.io/my-docs/>，项目站点必须带仓库路径 `/my-docs/`。

我逐个打开了首页、关于、使用指南、API 参考四页，确认：顶部导航四项齐全且可点击；表格边框正常；代码块有语法高亮；页内链接（如首页指向 `about.md`）能跳转；CSS 与字体资源正常加载（页面不是无样式的纯文本）。

![公开网站首页，地址栏可见完整站点地址](images/06-site.png)

### 2.7 第二次推送与自动更新验证

在 `docs/index.md` 末尾增加一句容易辨认的话，提交并推送，**没有**点击手动运行：

```bash
git add docs/index.md
git commit -m "验证文档修改后自动更新网站"
git push origin main
```

推送后 Actions 自动出现一条新的运行记录并成功，刷新网站后新增文字出现，说明“推送 → 自动构建 → 自动部署 → 网页更新”这条链路是通的。

- 对应提交链接：【待填：第二次推送的 commit 链接】
- 对应 Actions 运行链接：【待填：第二次运行的 Actions 链接】

![第二次 push 自动触发的成功运行记录](images/07-second-run.png)

![网站出现新增文字的效果](images/08-site-updated.png)

### 2.8 教师仓库 Issue

在教师仓库 `assignments` 提交个人 Issue，标题 `02-github-pages liuhaolin`，正文填写个人报告地址、源码仓库地址、网站地址和第二次成功运行的链接。每人只提交一条，需要更正时编辑原 Issue。

## 3. 问题与解决

**问题：第一次部署失败，`deploy` 步骤报 Pages 未启用。**
现象：`build` 成功，`deploy` 在 `Deploy to GitHub Pages` 步骤报错。
原因：Pages 的发布来源还停留在默认分支模式，没有切换到 GitHub Actions。
解决：到 `Settings` → `Pages` 把 Source 改成 GitHub Actions，然后在 Actions 页面用 `Run workflow` 手动重跑，部署成功。

**问题：本地构建报链接警告。**
现象：`--strict` 下构建中断，提示某页面链接找不到。
原因：在 `index.md` 里把链接写成了 `docs/about.md`，而 Markdown 内的链接是相对于当前文件所在目录的。
解决：改成 `about.md`，重新构建通过。

**另外核对过的地方：** 确认 `.gitignore` 生效（`git status` 中没有 `site/` 和 `.venv/`）；确认 `mkdocs.yml` 使用空格缩进而非 Tab；确认工作流里的 `${{ }}` 表达式未被改动。

## 4. 原理理解

**1. `docs/` 和 `site/` 各保存什么？为什么不用手工修改 `site/`？**

`docs/` 保存 Markdown 源码，是我真正维护的内容；`site/` 是 `mkdocs build` 生成的 HTML、CSS、JS 产物，是给浏览器看的成品。`site/` 完全由源码推导而来，手工改它下次构建就被覆盖；而且它文件数量多、没有阅读价值，提交进仓库只会让提交记录变得杂乱。所以它被写进 `.gitignore`，由 CI 每次重新生成。

**2. 本次工作流在什么情况下自动运行？**

两种触发方式：一是向 `main` 分支推送，并且改动涉及 `docs/**`、`mkdocs.yml`、`requirements.txt` 或工作流文件本身；二是在 Actions 页面手动触发 `workflow_dispatch`。因为 `paths` 做了过滤，只改 `report/` 或根 `README.md` 不会触发。

**3. `build` 与 `deploy` 两个任务有什么区别？**

`build` 只读取仓库，产出 `site/` 并上传为 artifact，权限只需要 `contents: read`；`deploy` 不构建任何东西，只把已有 artifact 发布到 Pages，因此需要 `pages: write` 和 `id-token: write`。两者靠 `needs: build` 串成前后依赖：构建失败就不会部署，避免把坏版本发到线上。分开还有一个好处：出问题时能一眼看出是“构建坏了”还是“发布坏了”。

**4. 只修改 `report/README.md` 会触发部署吗？为什么？**

不会。`on.push.paths` 只列了 `docs/**`、`mkdocs.yml`、`requirements.txt` 和工作流文件，`report/README.md` 不在其中，所以推送后 Actions 不会启动新的运行。这也是预期行为：报告不属于网站内容，重新部署没有意义。

**5. 本次方案为何无需创建 `gh-pages` 分支？**

旧方案是把构建产物推到 `gh-pages` 分支，再让 Pages 从该分支发布，于是需要维护一条长期存在的产物分支，还要配置写权限。现在的官方方案改用 artifact 传递：`upload-pages-artifact` 把 `site/` 打成部署包，`deploy-pages` 直接把它发布到 Pages，产物不进入任何分支。因此仓库只有 `main` 一条分支，历史干净，也不需要开放额外的写权限。
