# OmniPost MCP for Domestic AI Agent Platforms

OmniPost MCP connects OmniPost AI to MCP-compatible agents. This repository contains usage documentation, client configuration examples, and platform capability notes. It does not publish the MCP server implementation.

## Official package

```bash
npx -y @omnipost/mcp
```

Set the API key through `OMNIPOST_API_KEY`. Keep the key in the client secret or environment configuration and never commit it.

## OmniPost capabilities

OmniPost covers the content operations loop: trend and topic discovery, persona analysis, AI writing/image/video/audio creation, platform adaptation, draft and scheduled publishing, comment and DM conversion, and unified analytics. The platform supports mainland China and international social/content platforms; the live account and capability inventory returned by MCP is authoritative.

See the Chinese guide for the full platform list: [`docs/platforms.md`](./docs/platforms.md).

## Claude Desktop

Edit `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "omnipost": {
      "command": "npx",
      "args": ["-y", "@omnipost/mcp"],
      "env": { "OMNIPOST_API_KEY": "<YOUR_API_KEY>" }
    }
  }
}
```

## Cursor / Windsurf

Open **Settings → MCP**, add a server, and paste the same JSON configuration. The client launches the local stdio server with `npx -y @omnipost/mcp`.

## Kimi Code

Kimi Code supports stdio, HTTP and SSE MCP servers. Add the following to the user-level `~/.kimi-code/mcp.json` or project-level `.kimi-code/mcp.json`:

```json
{
  "mcpServers": {
    "omnipost": {
      "command": "npx",
      "args": ["-y", "@omnipost/mcp"],
      "env": { "OMNIPOST_API_KEY": "<YOUR_API_KEY>" }
    }
  }
}
```

Use `/mcp` to inspect connection status. Use `/mcp-config` to add or edit the server interactively.

## Alibaba Cloud Bailian (百炼)

Bailian supports custom MCP services. In the Bailian console, open **MCP Management → Create MCP Service → Script deployment**. For an npm-based local MCP package, use an NPX configuration equivalent to:

```json
{
  "mcpServers": {
    "omnipost": {
      "command": "npx",
      "args": ["-y", "@omnipost/mcp"],
      "env": { "OMNIPOST_API_KEY": "<YOUR_API_KEY>" }
    }
  }
}
```

Choose the deployment mode and region appropriate for your account. Verify the package source and requested permissions before deploying a third-party MCP service.

## Qoder, Trae, Coze, Dify and other Chinese agent platforms

The exact UI and supported transport vary by product version. When the platform provides **Custom MCP / MCP Server / Tools**, use the same stdio command and environment variables:

```text
Command: npx
Arguments: -y @omnipost/mcp
Environment: OMNIPOST_API_KEY=<YOUR_API_KEY>
```

If the platform only accepts remote MCP, deploy the package behind an approved MCP gateway and configure the resulting Streamable HTTP or SSE URL. Do not paste the API key into prompts or public workflow definitions.

## Recommended workflow

1. Check OmniPost and account readiness.
2. Read the live platform capability list.
3. Prepare title, summary, body, tags, media and platform-specific fields.
4. Preview or validate before creating a draft, schedule, or direct publish.
5. Read per-platform results, URLs, and failure reasons.
6. Review comments, conversions and analytics after publishing.

## References

- [OmniPost](https://www.omnipost.cc/)
- [Kimi Code MCP documentation](https://www.kimi.com/code/docs/en/kimi-code-cli/customization/mcp.html)
- [Alibaba Bailian custom MCP](https://help.aliyun.com/zh/model-studio/custom-mcp)
- [OmniGoAI MCP distribution guide](https://omnigoai.com/en/blog/mcp-content-distribution-guide/)
