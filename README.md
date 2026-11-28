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
engineering/index.json       工程分类、内容索引与精选标记
engineering/{category}/*.md  工程内容正文（workflows / models / practices / standards）
engineering/updates/index.json 工程动态记录
images/                      封面、头像、OG 图
_headers                     数据站点缓存与安全响应头
```

## 约定

- 所有 JSON 必须包含 `"schemaVersion": "1.0"`。
- `slug` 只能使用小写字母、数字和连字符。
- 公开仓库，不得提交 `visibility: private` 的内容。
- 字段、枚举与校验规则以框架仓库文档 `doc V2/03-A-数据契约.md` 为准。
- 工程内容的 `id` 与 `path` 发布后保持稳定，重命名会导致已发布链接失效。

## 工程内容

工程内容放在 `engineering/` 目录，由框架在构建期读取并按分类渲染。

```
engineering/
├─ index.json              分类卡片、内容索引与精选标记
├─ workflows/              工作流程与开发流程
├─ models/                 工程模型
├─ practices/              实践方法
├─ standards/              技术架构与规范
└─ updates/index.json      工程动态
```

### 索引结构

`engineering/index.json` 包含两部分：

- `categories`：工程首页的分类卡片，共 5 个（工作流程、开发流程、工程模型、实践方法、技术架构与规范）。字段为 `id`、`name`、`nameEn`、`summary`、`href`、`order`、`wide?`。
- `items`：全部工程内容元数据，`category` 只能是 `workflows` / `models` / `practices` / `standards`。

内容条目字段：

| 字段 | 必填 | 说明 |
| --- | --- | --- |
| `id` | 是 | 唯一标识，详情页路径为 `/engineering/{category}/{id}/` |
| `title` | 是 | 内容标题 |
| `category` | 是 | 所属分类 |
| `summary` | 是 | 摘要，用于列表与首页 |
| `path` | 是 | 正文路径，如 `models/project-lifecycle.md`，必须位于同名分类目录 |
| `status` | 是 | `draft` / `published` / `archived`，页面只读取 `published` |
| `featured` | 否 | 是否进入首页精选，默认 `false` |
| `order` | 是 | 分类内展示顺序，同一分类不可重复 |
| `updatedAt` | 是 | 最近更新日期 `YYYY-MM-DD` |
| `steps` | 否 | 流程步骤 `{ title, summary? }[]`，用于首页时间线 |
| `highlights` | 否 | 模型结构要点，用于卡片内的结构示意 |

### 工程动态

`engineering/updates/index.json` 的 `updates` 数组字段：`id`、`title`、`summary`、`date`、`category`、`href?`。`href` 为站内 `/engineering/...` 路径；页面按 `date` 倒序展示。

### 添加内容

1. 在对应分类目录新增 Markdown 正文，不需要 frontmatter。
2. 在 `engineering/index.json` 的 `items` 中登记元数据。
3. 需要进入首页精选时，设置 `featured: true` 并调整 `order`。
4. 在 `engineering/updates/index.json` 记录本次更新。

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
- 工程索引：<http://127.0.0.1:4321/engineering/index.json>
- 工程动态：<http://127.0.0.1:4321/engineering/updates/index.json>

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
