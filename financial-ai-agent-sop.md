# 总结：Anthropic 公开金融 AI Agent 的“SOP”

> 原文标题：《Anthropic 真的惊为天人：金融 AI Agent 的 SOP 被公开了》
> 项目地址：https://github.com/anthropics/financial-services
> 官方公告：https://www.anthropic.com/news/finance-agents

## 一句话概括

Anthropic 开源了 Claude for Financial Services 的完整参考实现，把投行、研究、私募、财富管理、基金运营、KYC 等金融业务拆成**可安装、可部署、可审计**的 Agent 工作流模板。文章认为它的意义不在单个功能，而在于提供了一套“行业 AI 落地”的打法。

## 核心结构：三层

| 层级 | 作用 | 内容 |
| --- | --- | --- |
| 端到端 Agent | 跑完一个完整工作流 | 10 个自包含 Agent，各自打包所需的技能和执行方式 |
| 垂直行业插件 | 沉淀行业技能、命令和规则 | 7 类插件，全部是 Markdown / JSON / YAML 文件，无需构建 |
| MCP 连接器 | 接入真实金融数据和企业系统 | Daloopa、Morningstar、S&P Global、FactSet、Moody's、LSEG、PitchBook 等 |

### 1. 10 个端到端 Agent

| Agent | 做什么 |
| --- | --- |
| Pitch Agent | 生成 pitch deck（comps、先例交易、LBO、品牌化排版） |
| Meeting Prep Agent | 客户会前 briefing pack |
| Market Researcher | 行业概览、竞争格局、peer comps、标的清单 |
| Earnings Reviewer | 读电话会和 filings，更新模型，起草研报 |
| Model Builder | 在 Excel 中做 DCF、LBO、三表模型、可比公司分析 |
| Valuation Reviewer | 私募估值与 LP 报告 |
| GL Reconciler | 总账核对、找差异、路由审批 |
| Month-End Closer | 月末关账、accruals、roll-forwards、偏差解释 |
| Statement Auditor | 审计 LP statement |
| KYC Screener | 解析 KYC 文档，规则引擎检查并标记缺口 |

### 2. 7 类垂直插件

- **financial-analysis**：comps、DCF、LBO、三表模型、deck QC、Excel 审计
- **investment-banking**：CIM、teaser、流程信、买方名单、并购模型、项目跟踪
- **equity-research**：财报笔记、首次覆盖、模型更新、投资逻辑跟踪
- **private-equity**：寻源、筛选、尽调、IC memo、投后监控
- **wealth-management**：客户复盘、财务规划、调仓、报告、税损收割
- **fund-admin**：GL 核对、差异追踪、NAV 校验
- **operations**：KYC 和规则引擎

### 3. 数据连接器才是壁垒

金融 AI 的难点不在“模型会不会分析”，而在于：能否拿到对的数据、能否在权限范围内使用、能否留下可审计的过程。自己逐家对接数据商 API（鉴权、字段、权限、错误处理、合规）往往要耗费数月。Anthropic 把 Agent 和数据基础设施一起推出，这是它区别于普通 demo 的关键。

## 部署路径

- **个人**：Claude Cowork / Claude Code，装在分析师电脑上
- **企业**：Claude Managed Agents API，接在公司自己的工作流引擎后面
- **Office**：接入 Excel、PowerPoint、Word、Outlook（Microsoft 365）
- **自有云**：Vertex AI、Bedrock 或内部 LLM gateway

金融工作的交付物大量落在 Excel、PPT、Word 和邮件里，因此 Office 集成尤其重要。

## 上手指引

重点目录：

- `plugins/agent-plugins/`：10 个金融 Agent
- `plugins/vertical-plugins/`：行业插件
- `managed-agent-cookbooks/`：企业用 Managed Agents API 部署的模板（企业团队最值得看）
- `claude-for-msft-365-install/`：Office 安装工具

Claude Code 安装示例（以官方 README 为准）：

```bash
claude plugin marketplace add anthropics/claude-for-financial-services
claude plugin install financial-analysis@claude-for-financial-services
claude plugin install pitch-agent@claude-for-financial-services
claude plugin install market-researcher@claude-for-financial-services
```

> 注：原文给出的 marketplace 名称是 `anthropics/claude-for-financial-services`，而项目地址是 `anthropics/financial-services`，安装前请对照仓库 README 确认。

常用命令：`/comps`、`/dcf`、`/earnings`、`/ic-memo`、`/client-review`

## 作者观点与启示

1. **竞争阶段转移**：AI Agent 的竞争正从“模型能力”转向“行业工作流”。谁能把行业知识、数据源、工具链、权限、审批流程和交付标准打包起来，谁就更可能成为企业离不开的基础设施。
2. **可复制的打法**：不做万能聊天助手，而是选一个高价值行业，把其中**高频、复杂、可标准化、需要交付结果**的流程做成可安装、可部署、可审计的 Agent。
3. **国内机会**：法务合同审查、跨境电商选品、制造业报价、医疗病历质控、财务报表合并、销售线索跟进、政企标书制作等场景，都在等待类似的“Agent SOP”。
4. **结论**：未来最值钱的可能不是 prompt，而是被 AI 重写过的行业工作流。

## 资料来源

- Anthropic 官方公告：https://www.anthropic.com/news/finance-agents
- GitHub 项目：https://github.com/anthropics/financial-services
- 原始讨论帖：https://x.com/GoSailGlobal/status/2053304501231350074/photo/1
