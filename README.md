# my-docs

个人技术文档网站：使用 Markdown + MkDocs Material 编写，通过 GitHub Actions 自动构建并部署到 GitHub Pages。

- **网站地址**：<https://humanguest2.github.io/my-docs/>
- **个人实验报告**：[个人实验报告](report/README.md)

## 目录结构

```text
my-docs/
├── .github/workflows/deploy.yml   # 自动构建与部署工作流
├── docs/                          # 网站内容（Markdown 源码）
├── report/                        # 实践报告与截图
├── mkdocs.yml                     # 站点配置与导航
├── requirements.txt               # 构建依赖
└── .gitignore
```

## 本地预览

```bash
python -m venv .venv
# Windows PowerShell: .\.venv\Scripts\Activate.ps1
# Windows Git Bash:   source .venv/Scripts/activate
# macOS / Linux:      source .venv/bin/activate
python -m pip install -r requirements.txt
python -m mkdocs serve
```

浏览器打开 <http://127.0.0.1:8000/>。构建检查用 `python -m mkdocs build --strict`。

## 发布

修改 `docs/` 下的文件后推送到 `main`，Actions 会自动构建并部署，无需手动操作。
