# 项目主计划

## 目标

在作者的微信收发、扫码绑定和 Obsidian 本地写入能力上，做一个适合个人知识工作流的微信入口：消息不丢、来源可追溯、链接可自动整理，普通记录保持纯机械写入。

## 当前版本

- 分支：`feature/link-summary-v0.1`
- 候选版本：`0.5.1`
- 上游：`ArtemisLin/obsidian-wechat-diary`
- 开发仓库：`Dongfangxiaoli/obsidian-wechat-diary`
- 生产 Vault：`C:\Obsidian-Template-main`（开发阶段只读，仅在授权后备份并安装候选版）
- 生产文件：`0.5.1`，升级前 `0.5.0` 完整备份位于 `C:\Obsidian-Template-main\.obsidian\plugin-backups\wechat-diary-20260828200345473`；运行实例待关闭再开启插件后生效。

## 当前架构

`微信 iLink → DiaryAgent → DiaryWriter（日记原文） → LinkSummarizer（仅独立公开 URL） → 用户配置的 OpenAI 兼容接口 → Obsidian 网络剪藏`

## 已完成

- fork、克隆并配置 `origin` / `upstream`。
- 确认复用现有 `requestUrl`、`AiClient`、`DiaryWriter` 与微信回执链路。
- V0.1 链接总结实现：URL 识别、内网地址拦截、正文提取、AI 摘要、Markdown 落库、去重和失败回执。
- `node --check main.js` 通过；`node tests/bindtest.js` 全部 416 项通过。
- 已建立隔离 Vault `C:\Users\L\Desktop\obsidian插件-测试库`，候选插件文件通过硬链接指向开发分支。
- Obsidian 1.13.7 GUI 冒烟通过：候选插件成功加载，状态栏正常显示，设置页可见“微信”“链接总结”“AI”及其字段。
- 已备份并安装到生产 Vault；三个发布文件与开发分支 SHA-256 一致，插件列表显示 `WeChat Workbench v0.4.0`，原微信绑定保持“已连接”。
- 生产真实链路已跑通到落库：微信独立 URL 先写入日记，OpenCode Go `deepseek-v4-flash-vision-exp` 生成摘要，并创建 `03资源/网络剪藏/2026-08-28-Go-da56b8be.md` 与去重记录。
- 升级现场确认 Obsidian 的“重新加载插件列表”不会替换已运行实例；覆盖发布文件后必须关闭再开启该插件，才会执行新版 `main.js`。
- `0.5.0` 候选功能：新剪藏按 `年/日期/` 归档，最多 3 张正文图片临时参与多模态总结，不写入 Vault；接口不接受图片时自动退回纯文本。
- 生产升级文件校验完成：`main.js`、`manifest.json`、`styles.css` 与开发分支 SHA-256 一致，`data.json` 升级前后未变。
- `0.5.0` 生产运行实例已重新加载，Obsidian 主界面状态栏确认“📖 微信日记: 已连接”。
- 真实公众号链接首次验收定位到两层原因：默认请求被返回“环境异常”验证页；移动端请求可取得真实文章，但该页约 3.26MB，超过原 2MB 上限。
- `0.5.1` 候选修复：仅对 `mp.weixin.qq.com` 使用微信移动端请求头并将 HTML 内存上限调到 6MB；其他站点仍保持 2MB，验证页改为明确回执。
- `0.5.1` 回归测试 421/421 通过，生产发布文件已安装且 SHA-256 与开发分支一致，`data.json` 未变。

## 未完成

- `0.5.1` 运行实例待关闭再开启插件，然后重试同一公众号链接。

## 关键约束

- 原链接必须先写入日记，摘要失败不得造成数据丢失。
- API Key 继续使用 Obsidian 密钥存储，不进入 Vault 或 Git。
- 保留 AGPL-3.0 与原作者署名。
- 不抓取 localhost、局域网和常见链路本地地址。
- 第一版只支持可直接读取 HTML 的公开网页；不承诺小红书、抖音视频、登录页、OCR 或视频转写。

## 下一步

1. 关闭再开启 WeChat Workbench，确认状态栏仍为“已连接”。
2. 重发同一公众号链接，验证微信回执、按日归档和多模态总结。

## 待 Sol 审核

无。本阶段是现有插件内的低风险增量功能，不涉及总体架构、工程安全算法或不可逆迁移。
