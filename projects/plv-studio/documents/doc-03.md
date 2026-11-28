---
id: doc-03
title: 产品设计
project: plv-studio
stage: 3
order: 3
date: '2026-11-22'
summary: 设计 Token、全局组件与六个页面的 UI 规格。
visibility: public
---

# 产品设计

## 视觉方向

Modern Editorial + Engineering Studio：编辑式排版、工程感网格、等宽字体用于日期与阶段编号。

- 主背景 `#F8FAFC`，卡片 `#FFFFFF`，主文字 `#111827`。
- 品牌蓝 `#2563EB`，成功 `#16A34A`，警示 `#D97706`。
- 暗色主题逐项校验对比度，不使用简单反相。
- 正文行宽 760px，内容容器 1200px。

## 核心组件

SiteHeader、SiteFooter、ThemeToggle、SectionHeading、StatusBadge、TechBadge、ProgressBar、StageRail、StageTimeline、ProjectCard、FeaturedCard、DevelopingCard、LiveStats、CapabilityTabs、ReleaseLog、DocumentCard、DocToc、DocPager。

## 页面要点

- 首页：Hero 使用品牌主张 + Live Stats；精选第 1 个使用终端风格主卡。
- 正在开发：卡片展示"起始 → 当前阶段 → 最近更新"三点式历程和八阶段缩略轨道。
- 详情页：主内容 760px + 280px 侧栏；侧栏含目录、技术栈、项目事实、外部入口。
- 文档页：三栏阅读，移动端目录抽屉；代码块构建期高亮并提供复制。
- 404：等宽大字、两个明确入口。

## 主题

浅色 / 暗色 / 跟随系统三选一，首屏内联脚本设置 `data-theme`，避免闪烁。
