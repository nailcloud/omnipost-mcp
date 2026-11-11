# 国内 AI Agent 平台配置说明

本页整理 OmniPost MCP 在主要国内 AI Agent / Agent 平台中的接入方式。平台界面和权限策略会更新，配置完成后请先查看 MCP 连接状态，再调用 OmniPost 的平台发现能力。

## 通用配置

OmniPost 官方 MCP 包使用本地 stdio 启动：

```text
命令：npx
参数：-y @omnipost/mcp
环境变量：OMNIPOST_API_KEY=<YOUR_API_KEY>
```

标准 JSON：

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

## Kimi Code / Kimi Code Desktop

Kimi Code 官方支持 stdio、HTTP 和 SSE MCP。配置文件位置：

- 用户级：`~/.kimi-code/mcp.json`
- 项目级：`.kimi-code/mcp.json`

写入上面的 JSON 后：

- 使用 `/mcp` 查看连接状态
- 使用 `/mcp-config` 交互式添加、编辑或删除 MCP
- 已打开的旧会话可能需要新建会话后才会加载新配置

参考：[Kimi Code MCP 文档](https://www.kimi.com/code/docs/en/kimi-code-cli/customization/mcp.html)。

## 阿里云百炼（Model Studio）

百炼支持自定义 MCP 服务。进入 **MCP 管理 → 创建 MCP 服务 → 脚本部署**，使用 NPX 启动方式，填入：

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

根据账号选择基础模式或极速模式，并选择合适的部署地域。部署前请核验 npm 包来源、版本和权限；不要把真实 API Key 写进公开工作流模板。

参考：[百炼自定义 MCP 服务](https://help.aliyun.com/zh/model-studio/custom-mcp)。

## Qoder、Trae、扣子（Coze）、Dify 等

这些产品的 MCP 能力、入口名称和支持的传输方式可能随版本和套餐变化。若界面提供 **自定义 MCP / MCP Server / 工具服务**，按以下字段配置：

```text
Command: npx
Arguments: -y @omnipost/mcp
Environment: OMNIPOST_API_KEY=<YOUR_API_KEY>
```

如果平台只接受远程 MCP：

1. 在受信任的 MCP 网关或自有服务器中部署 `@omnipost/mcp`。
2. 为服务配置安全的 API Key 注入方式。
3. 在平台中填写 Streamable HTTP 或 SSE 地址。
4. 先调用平台发现和账号状态工具，再进行草稿或发布操作。

不要把 API Key 放进提示词、公开 Bot 配置、公开工作流或前端代码中。

## 国内客户端的通用排障

- **看不到工具**：重新打开会话，确认 Node.js 和 `npx` 可执行。
- **鉴权失败**：检查 `OMNIPOST_API_KEY` 是否在 MCP 进程环境中，而不是只写在当前终端。
- **平台不能发布**：先读取实时平台能力、账号状态和必填字段。
- **只想测试连接**：先列出工具和平台，不要直接执行发布。
- **远程部署失败**：确认网关支持 stdio 转 Streamable HTTP/SSE，并检查出站网络和超时配置。
