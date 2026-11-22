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

## 更新流程

1. 修改 JSON 或 Markdown。
2. push 到 `master`。
3. Cloudflare Pages 数据项目自动部署。
4. Deploy Hook 触发框架项目重建，约 1 ~ 2 分钟生效。
