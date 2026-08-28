# 项目主计划

## 目标

在作者的微信收发、扫码绑定和 Obsidian 本地写入能力上，做一个适合个人知识工作流的微信入口：消息不丢、来源可追溯、链接可自动整理，普通记录保持纯机械写入。

## 当前版本

- 分支：`feature/link-summary-v0.1`
- 候选版本：`0.4.0`
- 上游：`ArtemisLin/obsidian-wechat-diary`
- 开发仓库：`Dongfangxiaoli/obsidian-wechat-diary`
- 生产 Vault：`C:\Obsidian-Template-main`（开发阶段只读，不直接覆盖）

## 当前架构

`微信 iLink → DiaryAgent → DiaryWriter（日记原文） → LinkSummarizer（仅独立公开 URL） → 用户配置的 OpenAI 兼容接口 → Obsidian 网络剪藏`

## 已完成

- fork、克隆并配置 `origin` / `upstream`。
- 确认复用现有 `requestUrl`、`AiClient`、`DiaryWriter` 与微信回执链路。
- V0.1 链接总结实现：URL 识别、内网地址拦截、正文提取、AI 摘要、Markdown 落库、去重和失败回执。
- `node --check main.js` 通过；`node tests/bindtest.js` 全部 416 项通过。

## 未完成

- Obsidian GUI 冒烟验收。
- DeepSeek/OpenAI 兼容接口的真实 Key 验证。
- 生产 Vault 安装只在候选版验证后执行。

## 关键约束

- 原链接必须先写入日记，摘要失败不得造成数据丢失。
- API Key 继续使用 Obsidian 密钥存储，不进入 Vault 或 Git。
- 保留 AGPL-3.0 与原作者署名。
- 不抓取 localhost、局域网和常见链路本地地址。
- 第一版只支持可直接读取 HTML 的公开网页；不承诺小红书、抖音视频、登录页、OCR 或视频转写。

## 下一步

1. 在隔离测试 Vault 中加载候选版，验证设置页、微信回执和笔记落库。
2. 使用真实 AI 配置验证一个公开网页链接。
3. 验收通过后再决定是否替换生产 Vault 中的 `wechat-diary`。

## 待 Sol 审核

无。本阶段是现有插件内的低风险增量功能，不涉及总体架构、工程安全算法或不可逆迁移。
