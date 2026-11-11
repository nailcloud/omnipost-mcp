# OmniPost MCP 使用指南

OmniPost MCP 是 OmniPost 的 AI Agent 接入说明与配置示例项目。本仓库只提供使用文档、工具清单、调用流程和客户端配置示例，不包含 OmniPost MCP 服务端源码。

## OmniPost MCP 能做什么

通过一个 MCP 连接，让 Claude、Codex、Cursor、OpenClaw、Cline 等客户端调用 OmniPost 的内容运营能力：

- **发布与分发**：查看平台和账号状态，检查内容，创建发布提案，支持草稿、预览、定时和自动执行模式。
- **内容管理**：创建和更新文章草稿，查看文章，管理灵感和笔记。
- **知识库**：列出知识库，导入文本或链接，搜索私有知识内容。
- **媒体处理**：上传图片、视频等 Base64 或 Data URL 媒体并获得可用于发布的 URL。
- **营销策划**：营销诊断、活动计划、跨平台内容矩阵、转化信号复盘。
- **评论运营**：扫描未回复评论和高价值评论，分析评论并生成回复草稿。
- **创作辅助**：改写、翻译、摘要等创意工具。
- **治理与观测**：权限检查、积分检查、系统健康、文章表现和 Agent 操作记录。

## 接入方式

### Claude Code

```bash
claude mcp add omnipost --transport sse https://你的域名/v1/mcp/sse
```

如果使用本地开发环境：

```bash
claude mcp add omnipost --transport sse http://127.0.0.1:3001/v1/mcp/sse
```

### Claude Desktop / Cursor / Cline

参考 [`examples/mcp-client-config.json`](./examples/mcp-client-config.json)，将 `url` 改成你的 OmniPost MCP SSE 地址，并按客户端格式粘贴配置。

### Codex / OpenClaw / 自定义 Agent

使用标准 MCP SSE 客户端连接：

```text
SSE:      https://你的域名/v1/mcp/sse
Messages: https://你的域名/v1/mcp/messages?sessionId={sessionId}
```

连接后先调用 `list_platforms`，再根据账号状态选择草稿、预览或发布流程。

## 推荐工作流

1. 调用 `check_authority` 确认当前动作被允许。
2. 调用 `list_platforms` 查看平台、账号和可用内容类型。
3. 调用 `manage_article_draft` 创建或更新主稿。
4. 需要图片或视频时调用 `upload_media`。
5. 调用 `propose_publish`，默认先使用 `dryRun: true` 做校验。
6. 校验通过后选择 `draft`、`preview` 或 `auto` 执行模式。
7. 发布后调用 `list_articles`、`inspect_performance` 或 `scan_comments` 做跟踪。

示例提示词：

```text
查看我已连接的平台，检查这篇文章是否适合知乎和 CSDN，先创建草稿，不要直接发布。
```

```text
把当前文章改写成小红书和知乎两个版本，分别生成标题、正文、话题和发布建议，然后进入预览。
```

## 工具清单

完整清单见 [`docs/tools.md`](./docs/tools.md)。工具按发布、内容、知识库、营销、评论、创作和治理分类，并注明常用参数和副作用。

## 安全与权限

- MCP 会话应使用 OmniPost 的 API Key、登录态或网关认证，不要把密码、Cookie 或私钥写进客户端配置。
- 发布工具支持 dry run；生产环境建议先校验，再草稿或预览，最后由用户确认发布。
- 评论分析默认只生成回复草稿，不自动发送回复。
- 媒体上传只接受明确提供的内容，不要上传包含密钥或个人隐私的文件。
- `.env`、Token、Cookie、数据库连接信息不属于本项目内容。

## 常见问题

### 连接成功但看不到工具

确认 SSE 地址和消息回调地址属于同一版本，并检查网关是否把有状态 SSE 会话固定到同一个 API MCP 实例。

### 发布工具返回校验错误

先调用 `list_platforms` 读取平台能力，再根据平台要求补齐标题、正文、封面、媒体、话题或定时参数。

### 想只保存草稿

调用 `propose_publish` 时使用 `dryRun: false`，并为目标平台设置 `mode: "draft"`。

## 项目边界

本仓库是 OmniPost MCP 的公开使用文档和配置示例。MCP 服务实现、OmniPost 核心服务、平台适配器、数据库和商业逻辑继续保留在私有应用项目中。

## License

文档和示例采用 MIT License。OmniPost 应用本体的授权以其私有项目和产品条款为准。
