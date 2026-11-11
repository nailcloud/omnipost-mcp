# OmniPost MCP 工具清单

## 发布与平台

- `list_platforms`：列出平台、账号、登录状态、内容类型和发布能力。
- `propose_publish`：校验内容并创建发布提案；支持 `dryRun`、`draft`、`preview`、`auto`、定时和幂等键。
- `upload_media`：上传 Base64 或 Data URL 媒体并返回发布可用 URL。

## 内容与知识库

- `manage_article_draft`：创建或更新文章草稿。
- `list_articles`：列出近期文章和草稿。
- `manage_note` / `list_notes`：管理灵感、选题和个人笔记。
- `list_notebooks`：列出知识库。
- `ingest_knowledge`：导入文本或链接。
- `query_knowledge`：搜索私有知识库。

## 营销与创作

- `marketing_diagnose`：分析产品、受众、定位、渠道和转化路径。
- `marketing_campaign_plan`：生成活动阶段、日历、KPI、素材和审批清单。
- `marketing_content_matrix`：生成跨平台标题、钩子、正文、标签、CTA 和视觉 brief。
- `marketing_conversion_review`：复盘评论和表现数据中的转化信号。
- `invoke_creative_tool`：执行改写、翻译、摘要等创作技能。
- `generate_visual_content`：根据提示生成图片或视频素材。

## 评论与运营

- `scan_comments`：扫描未回复或高价值评论。
- `analyze_comments`：分析评论并生成回复草稿，不直接发送。
- `inspect_performance`：查看文章表现、互动和趋势。

## 治理与观测

- `check_authority`：检查动作是否被授权。
- `check_credits`：检查积分和用量限制。
- `check_system_health`：读取系统健康指标。
- `record_thought`：记录可审计的 Agent 操作步骤。
- `log_usage`：记录工具用量和成本。
