---
name: web-content-master
description: Unified web content reading and extraction workflow for OpenClaw using web_fetch + browser + feishu_doc. Use when the user asks to read, extract, summarize, or batch-process links (WeChat articles, Xiaohongshu, Bilibili, Zhihu, normal webpages, or Feishu docx/wiki).
---

# Web Content Master（网页读取大师）

目标：给一批链接时，自动判断平台与反爬强度，选择最稳的读取方式，并输出可用于“拉片/证据索引/资料库沉淀”的结构化结果。

## 选择策略（先选方法，再读内容）

### 1) 飞书文档（docx/wiki）
**触发**：URL 包含 `feishu.cn/docx/`、`my.feishu.cn/docx/`、`feishu.cn/wiki/`、`my.feishu.cn/wiki/`

**方法**：优先 `feishu_doc.read` / `feishu_doc.list_blocks`

**要点**：
- 需要定位具体内容：用 `list_blocks` 找 block，再 `get_block/update_block`。
- 评论不在正文里：如需评论闭环，走 Drive Comment API（当前会话工具未暴露时，可用外部 OpenAPI 直连方案）。

### 2) 普通网页（大多数站点）
**方法**：优先 `web_fetch(url, extractMode="markdown")`

**适用**：资讯站、博客、百科、媒体文章、非强登录页面。

### 3) 需要动态渲染 / 强反爬 / 需要登录
**方法**：改用 `browser`（打开→snapshot→必要时滚动/展开→再提取）

**适用**：小红书、部分公众号、部分平台内容区需要 JS 渲染。

## 执行流程（单链接）

1. 识别平台类型（飞书/普通网页/强反爬）
2. 选读取工具：feishu_doc → web_fetch → browser（兜底）
3. 抽取信息（按用户要求）：
   - 标题/作者/发布时间（能拿就拿）
   - 正文（尽量保留段落结构）
   - 关键引用段落（用于“证据版”）
   - 链接列表（文内引用/相关推荐）
4. 以可复用的结构输出（见“输出模板”）

## 批量读取流程（多链接）

1. 先对每个 URL 做平台分类（飞书/普通/强反爬）
2. 先跑 web_fetch 组（快、便宜）
3. 失败或空内容的，自动升级到 browser 组
4. 输出每条的状态（success/failed）+ 失败原因 + 下一步建议（例如需要用户提供正文/截图/登录态）

## 输出模板（建议默认）

对每个 URL 输出：

- URL
- 平台：feishu|wechat|xiaohongshu|bilibili|zhihu|general
- 读取方式：feishu_doc|web_fetch|browser
- 标题（如有）
- 摘要（3-5条要点）
- 可引用片段（用于证据索引，最多3条，每条≤120字）
- 失败原因（如有）

## 特别注意（本项目常见坑）

- 微信文章可能被防爬：web_fetch 抽不出时，用 browser；仍失败就让用户复制正文/截图。
- 小红书通常需要 browser 且可能需要登录态：若用户提到“Chrome 扩展/Browser Relay”，用 `browser` 的 chrome profile 进行接管。
- 使用 `web_search` 时：`search_lang` 必须用 `zh-hans`（不要用 `zh`）。

## 快捷指令示例（给用户看的话术）

- “读取这个链接并提炼3条要点 + 2条可引用原文”：
  - 直接用 web_fetch/feishu_doc 读取，再按模板输出。

- “批量读取以下链接并做证据索引”：
  - 先 web_fetch 批量，失败再 browser。
