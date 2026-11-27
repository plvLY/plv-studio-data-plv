# plv-studio-data-plv

PLV Studio（主体 `plv`）的公开数据仓库，由 `plv-studio-framework` 在构建期读取。

## 目录

```
profile.json                 品牌、作者、首页 Hero、精选、SEO
works/index.json             作品 slug 列表（顺序即展示顺序）
works/{slug}.json            作品元数据
works/{slug}.md              可选补充正文
progress.json                八阶段进度与 changelog
skills.json                  能力与证据（可选）
documents/{slug}/{doc-id}.md 设计文档
images/                      封面、头像、OG 图
_headers                     数据站点缓存与安全响应头
```

## 约定

- 所有 JSON 必须包含 `"schemaVersion": "1.0"`。
- `slug` 只能使用小写字母、数字和连字符。
- 公开仓库，不得提交 `visibility: private` 的内容。
- 字段、枚举与校验规则以框架仓库文档 `doc V2/03-A-数据契约.md` 为准。

## 本地 HTTP 服务

本仓库是静态数据仓库，没有后端程序或构建步骤，可直接通过静态 HTTP 服务访问 JSON、Markdown 和图片资源。此服务不提供 PLV Studio 网站界面；正式网站由 `plv-studio-framework` 在构建期读取这些数据。

在仓库根目录启动服务：


使用 Python：

```powershell
python -m http.server 4321 --bind 127.0.0.1
```

或使用 Node.js：

```powershell
npx --yes serve . -l 4321
```

服务启动后可访问：

- 数据目录：<http://127.0.0.1:4321/>
- 站点信息：<http://127.0.0.1:4321/profile.json>
- 作品列表：<http://127.0.0.1:4321/works/index.json>
- 进度数据：<http://127.0.0.1:4321/progress.json>

根目录没有 `index.html`，访问 `/` 显示文件目录是正常的。修改数据后，刷新页面即可读取最新文件。

如需允许局域网内其他设备访问，可将 Python 启动命令改为：

```powershell
python -m http.server 4321 --bind 0.0.0.0
```

其他设备通过 `http://<本机局域网IP>:4321/` 访问，并按需允许 Windows 防火墙放行。

## 更新流程

1. 修改 JSON 或 Markdown。
2. push 到 `master`。
3. Cloudflare Pages 数据项目自动部署。
4. Deploy Hook 触发框架项目重建，约 1 ~ 2 分钟生效。
