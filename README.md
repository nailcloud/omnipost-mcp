# OmniPost MCP 使用指南

OmniPost MCP 是 OmniPost AI 的模型上下文协议（MCP）接入方式。它把 OmniPost 的 AI 创作、平台分发、账号管理、评论转化和数据分析能力暴露给 Claude Desktop、Cursor、Windsurf、Codex、OpenClaw 以及其他兼容 MCP 的 AI Agent。

本仓库只包含公开使用指南、配置示例和能力说明，不包含 MCP 服务端实现代码。

## OmniPost 平台介绍

OmniPost 是一个 AI 原生的内容运营和社交媒体矩阵管理平台，覆盖“选题 → 创作 → 审核 → 排期 → 多平台发布 → 评论/私信回复 → 数据看板”完整链路。

平台主要能力包括：

- **灵感与选题**：发现热点、关键词和可复用的内容方向。
- **人设实验室**：分析已有账号的语气、内容支柱和爆款模式，形成可复制的人设策略。
- **AI 创作中心**：生成文章、平台原生文案、图片、短视频、音频和音乐。
- **智能分发**：一份内容适配多个平台的格式、长度、话题标签和媒体要求。
- **排期与发布**：支持草稿、预览、定时和自动发布，并返回各平台独立结果。
- **评论与私信转化**：识别高意向评论，按规则生成或发送回复，并沉淀线索。
- **统一数据看板**：汇总曝光、互动、粉丝增长、发布状态和转化数据。
- **账号安全**：只有在用户明确授权后连接第三方账号，支持随时解绑；平台授权数据用于账号识别、刷新授权、发布用户批准的内容和同步发布状态。

官方平台介绍：[www.omnipost.cc](https://www.omnipost.cc/)。

## 支持的平台

OmniPost 官方页面列出的平台包括：

### 中国大陆平台

微信公众号、 小红书、抖音、知乎、今日头条、哔哩哔哩、微博、快手、百家号、搜狐号、网易号、新浪看点、企鹅号、大鱼号、简书、豆瓣小组、36Kr、少数派、掘金、CSDN、博客园、腾讯云开发者、思否、开源中国、51CTO、ITPUB、InfoQ 极客邦。

### 海外平台

X（Twitter）、LinkedIn、Facebook、Instagram、Threads、TikTok、YouTube、Pinterest，以及 OmniPost 账号中实际开放的其他国际平台连接。

平台可用性会受账号授权状态、套餐、平台规则和当前适配情况影响。接入后应先调用平台发现工具读取实时能力，不要仅凭静态列表判断某个平台是否能直接发布。

完整平台说明见 [`docs/platforms.md`](./docs/platforms.md)。

## MCP 配置

### Claude Desktop（Local）

编辑 `claude_desktop_config.json`，加入官方 MCP 配置：

```json
{
  "mcpServers": {
    "omnipost": {
      "command": "npx",
      "args": [
        "-y",
        "@omnipost/mcp"
      ],
      "env": {
        "OMNIPOST_API_KEY": "<YOUR_API_KEY>"
      }
    }
  }
}
```

安装和运行依赖 Node.js，并确保 API Key 具备你要调用的 OmniPost 权限。

### Cursor / Windsurf

进入 **Settings → MCP**，新增一个 MCP Server，并粘贴与 Claude Desktop 相同的 JSON 配置。客户端会通过 `npx -y @omnipost/mcp` 启动本地 MCP 进程。

### 自定义 AI Agent

自定义 Agent 使用同一个本地启动命令：

```bash
npx -y @omnipost/mcp
```

通过环境变量传入密钥：

```bash
OMNIPOST_API_KEY=<YOUR_API_KEY> npx -y @omnipost/mcp
```

Windows PowerShell：

```powershell
$env:OMNIPOST_API_KEY = "<YOUR_API_KEY>"
npx -y @omnipost/mcp
```

客户端配置示例见 [`examples/mcp-client-config.json`](./examples/mcp-client-config.json)。

## 推荐使用流程

1. 启动 OmniPost MCP 客户端连接。
2. 先读取 OmniPost 状态、平台和账号能力。
3. 准备标题、摘要、正文、标签、封面和平台专属字段。
4. 先预览或执行校验，再决定保存草稿、排期还是直接发布。
5. 发布后读取每个平台的结果、编辑器地址或公开链接。
6. 需要持续运营时，再读取评论、转化和数据看板。

MCP 适合“Agent 读取上下文 → 调用工具 → 查看结果 → 再决定下一步”的工作流。长篇本地 Markdown 文件更适合 CLI，已有 CMS、队列或后台系统更适合 HTTP；三种接口最终使用同一套 OmniPost 分发能力。

## 常用任务示例

```text
查看我已授权的平台和账号状态，把这篇内容适配到小红书、知乎和 CSDN，先做预览，不要直接发布。
```

```text
根据我的产品资料制定 7 天内容计划，生成小红书、抖音和 LinkedIn 的平台版本，并保存为草稿。
```

```text
检查最近 7 天的评论，找出高意向用户，生成回复草稿并列出需要人工确认的内容。
```

## 工具能力

完整工具清单见 [`docs/tools.md`](./docs/tools.md)，包含发布、草稿、知识库、媒体、营销、评论、创作和运营观测能力。

## 安全注意事项

- 不要把 API Key 提交到 Git 仓库、截图或聊天记录中。
- 使用 `<YOUR_API_KEY>` 占位符替换示例中的密钥。
- 生产环境优先使用草稿或预览模式，再进行人工确认。
- 只连接你拥有或明确获授权管理的第三方账号。
- 平台能力、发布权限和必填字段可能随账号和平台规则变化，调用前读取实时平台能力。

## 官方参考

- [OmniPost 官网](https://www.omnipost.cc/)
- [OmniPost MCP 集成入口](https://www.omnipost.cc/)
- [OmniPost AI 平台功能与平台覆盖](https://www.omnipost.cc/en/)
- [OmniGoAI MCP 内容分发指南](https://omnigoai.com/en/blog/mcp-content-distribution-guide/)
- [OmniGoAI MCP / CLI / HTTP 选择指南](https://omnigoai.com/en/blog/omnipost-cli-vs-mcp-vs-http/)

## 项目边界

本仓库是 OmniPost MCP 的公开使用文档和配置示例。MCP 服务端、OmniPost 核心应用、账号连接、平台适配器和商业逻辑不在本仓库中。

## License

文档和示例采用 MIT License。OmniPost 产品本体的授权以官方条款为准。
