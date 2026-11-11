# OmniPost MCP

OmniPost MCP 是 OmniPost AI 的模型上下文协议（MCP）接入方式。本仓库只包含公开使用说明、配置示例和能力介绍，不包含 MCP 服务端实现代码。

- 官方网站：https://www.omnipost.cc/
- 官方 MCP 包：`@omnipost/mcp`
- 适用客户端：Claude Desktop、Cursor、Windsurf、Kimi Code、Codex、OpenClaw、Cline，以及其他兼容 MCP 的 AI Agent

## 一、OmniPost 平台介绍

OmniPost 是 AI 原生的内容运营和社交媒体矩阵管理平台，覆盖“选题 → 创作 → 审核 → 排期 → 多平台发布 → 评论/私信回复 → 数据看板”完整链路。

核心能力：

- **灵感与选题**：发现热点、关键词和可复用的内容方向。
- **人设实验室**：分析账号语气、内容支柱和爆款规律，形成可复制的人设策略。
- **AI 创作中心**：生成文章、平台原生文案、图片、短视频、音频和音乐。
- **智能分发**：自动适配不同平台的格式、长度、话题标签、媒体和字段要求。
- **排期与发布**：支持草稿、预览、定时和自动发布，返回各平台独立结果。
- **评论与私信转化**：识别高意向用户，生成或执行规则化回复，并沉淀线索。
- **统一数据看板**：汇总曝光、互动、粉丝增长、发布状态和转化数据。
- **账号安全**：只有在用户明确授权后连接第三方账号，支持随时解绑。

## 二、支持的平台

平台实际可用能力取决于账号授权、套餐、平台规则和当前适配情况。连接 MCP 后，请优先读取实时平台能力。

### 中国大陆平台

微信公众号、小红书、抖音、知乎、今日头条、哔哩哔哩、微博、快手、百家号、搜狐号、网易号、新浪看点、企鹅号、大鱼号、简书、豆瓣小组、36Kr、少数派、掘金、CSDN、博客园、腾讯云开发者、思否、开源中国、51CTO、ITPUB、InfoQ 极客邦。

### 海外平台

X（Twitter）、LinkedIn、Facebook、Instagram、Threads、TikTok、YouTube、Pinterest，以及产品账号中开放的其他国际平台。

### 实时能力判断

不要只根据静态列表判断是否能发布。调用 MCP 后，应先查看：

1. OmniPost 是否在线；
2. 目标账号是否已授权且未过期；
3. 平台支持的内容类型；
4. 是否支持草稿、预览、排期或直接发布；
5. 当前内容需要哪些标题、摘要、标签、分类、封面和媒体字段。

## 三、官方 MCP 配置

### Claude Desktop（Local）

编辑 `claude_desktop_config.json`，加入：

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

### Cursor / Windsurf

进入 **Settings → MCP**，新增 MCP Server，粘贴上面的 JSON。客户端会使用以下命令启动本地 MCP：

```bash
npx -y @omnipost/mcp
```

### 自定义 AI Agent

```bash
OMNIPOST_API_KEY=<YOUR_API_KEY> npx -y @omnipost/mcp
```

Windows PowerShell：

```powershell
$env:OMNIPOST_API_KEY = "<YOUR_API_KEY>"
npx -y @omnipost/mcp
```

## 四、国内主流 AI Agent 平台配置

### Kimi Code / Kimi Code Desktop

Kimi Code 支持 stdio、HTTP 和 SSE MCP。配置文件：

- 用户级：`~/.kimi-code/mcp.json`
- 项目级：`.kimi-code/mcp.json`

写入：

```json
{
  "mcpServers": {
    "omnipost": {
      "command": "npx",
      "args": ["-y", "@omnipost/mcp"],
      "env": {
        "OMNIPOST_API_KEY": "<YOUR_API_KEY>"
      }
    }
  }
}
```

- `/mcp`：查看 MCP 连接状态
- `/mcp-config`：交互式添加或编辑 MCP

### 阿里云百炼（Model Studio）

进入 **MCP 管理 → 创建 MCP 服务 → 脚本部署**，选择 NPX 启动方式，使用：

```json
{
  "mcpServers": {
    "omnipost": {
      "command": "npx",
      "args": ["-y", "@omnipost/mcp"],
      "env": {
        "OMNIPOST_API_KEY": "<YOUR_API_KEY>"
      }
    }
  }
}
```

根据账号选择基础模式或极速模式及部署地域。部署前核验 npm 包来源、版本和权限，不要把真实 API Key 写入公开工作流模板。

### Qoder、Trae、扣子（Coze）、Dify 等

如果平台提供 **自定义 MCP / MCP Server / 工具服务**，填写：

```text
Command: npx
Arguments: -y @omnipost/mcp
Environment: OMNIPOST_API_KEY=<YOUR_API_KEY>
```

如果平台只接受远程 MCP：

1. 在受信任的 MCP 网关或自有服务器中部署 `@omnipost/mcp`。
2. 为服务配置安全的 API Key 注入方式。
3. 在平台中填写 Streamable HTTP 或 SSE 地址。
4. 先调用平台发现和账号状态工具，再执行草稿或发布。

不同产品版本和套餐的 MCP 入口可能不同，请以平台当前控制台为准。

## 五、推荐 Agent 工作流

```text
检查 OmniPost 状态
  → 查询账号和平台能力
  → 准备内容与平台字段
  → 预览 / 校验
  → 草稿或排期
  → 用户确认后发布
  → 查询结果与数据
```

推荐顺序：

1. 检查 OmniPost 和目标账号状态。
2. 读取实时平台能力。
3. 准备标题、摘要、正文、标签、封面和媒体。
4. 先预览或校验，再保存草稿、排期或直接发布。
5. 读取每个平台的成功、草稿、待人工操作、失败原因和链接。
6. 发布后复盘评论、转化和数据看板。

## 六、MCP 能力清单

`@omnipost/mcp` 会根据版本、账号授权、订阅方案和平台状态暴露工具。客户端连接后应以运行时返回的工具列表和参数 schema 为准。

### 平台与账号

- 查看 OmniPost 是否在线。
- 查看已连接的平台和账号。
- 查看内容类型、必填字段和发布方式。
- 判断账号是否需要重新授权。

### 内容分发

- 将已有内容转换为平台适配版本。
- 预览标题、正文、媒体、标签和链接。
- 创建平台草稿。
- 定时发布或直接发布已确认内容。
- 返回每个平台独立结果。
- 查询历史发布记录和公开链接。

### AI 内容运营

- 生成文章、短内容、图片、视频和音频素材。
- 改写标题、正文、标签和 CTA。
- 根据品牌知识、账号人设和平台规则生成内容。
- 生成小红书种草、抖音脚本、LinkedIn B2B 内容等场景化内容。

### 评论与转化

- 读取评论和互动信号。
- 识别高意向用户和关键词触发场景。
- 生成评论或私信回复草稿。
- 根据授权和规则执行自动回复或转人工。

### 数据与运营

- 汇总曝光、互动、粉丝增长和发布状态。
- 查询平台级或内容级表现。
- 记录失败原因、重新登录需求和后续重试信息。

## 七、常用提示词

```text
查看我已授权的平台和账号状态，把这篇内容适配到小红书、知乎和 CSDN，先做预览，不要直接发布。
```

```text
根据我的产品资料制定 7 天内容计划，生成小红书、抖音和 LinkedIn 的平台版本，并保存为草稿。
```

```text
检查最近 7 天的评论，找出高意向用户，生成回复草稿并列出需要人工确认的内容。
```

## 八、安全与排障

- 不要把 API Key 提交到 Git 仓库、截图、提示词或公开工作流。
- 使用 `<YOUR_API_KEY>` 占位符，并通过环境变量传入真实密钥。
- 生产环境优先使用草稿或预览，再进行人工确认。
- 只连接你拥有或明确获授权管理的第三方账号。
- 看不到工具时，确认 Node.js、`npx` 和 MCP 客户端版本，并重新打开会话。
- 鉴权失败时，确认 `OMNIPOST_API_KEY` 位于 MCP 进程环境中。
- 发布失败时，重新读取实时平台能力、账号状态和必填字段。

## 九、English Guide

OmniPost MCP connects OmniPost AI to Claude Desktop, Cursor, Windsurf, Kimi Code, Codex, OpenClaw, Cline and other MCP-compatible agents.

Official package:

```bash
npx -y @omnipost/mcp
```

Claude Desktop, Cursor and Windsurf use the same configuration:

```json
{
  "mcpServers": {
    "omnipost": {
      "command": "npx",
      "args": ["-y", "@omnipost/mcp"],
      "env": {
        "OMNIPOST_API_KEY": "<YOUR_API_KEY>"
      }
    }
  }
}
```

Kimi Code uses `~/.kimi-code/mcp.json` or `.kimi-code/mcp.json`. Alibaba Cloud Bailian supports custom MCP services through **MCP Management → Create MCP Service → Script deployment**. Qoder, Trae, Coze, Dify and similar products can use the same stdio fields when their current version exposes Custom MCP support; otherwise deploy the package behind a trusted Streamable HTTP or SSE gateway.

OmniPost provides topic discovery, persona analysis, AI writing/image/video/audio generation, platform adaptation, drafts, scheduling, publishing, comment and DM conversion, and unified analytics. Always read runtime platform capabilities and account state before publishing.

## 官方参考 / Official references

- [OmniPost 官网](https://www.omnipost.cc/)
- [OmniPost AI 英文平台介绍](https://www.omnipost.cc/en/)
- [Kimi Code MCP 文档](https://www.kimi.com/code/docs/en/kimi-code-cli/customization/mcp.html)
- [阿里云百炼自定义 MCP](https://help.aliyun.com/zh/model-studio/custom-mcp)
- [OmniGoAI MCP 内容分发指南](https://omnigoai.com/en/blog/mcp-content-distribution-guide/)
- [OmniGoAI MCP / CLI / HTTP 选择指南](https://omnigoai.com/en/blog/omnipost-cli-vs-mcp-vs-http/)

## Project boundary

This repository contains documentation and configuration examples only. The OmniPost MCP server implementation, core application, account connectors, platform adapters and commercial logic remain outside this public documentation repository.
