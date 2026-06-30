# OMJ - xeon的AI助手

> **术语**: omj = oh-my-chanjet，即本插件系统。在对话中 omj 均指代 oh-my-chanjet。

xeon 的 制造编码助手扩展。

## 全量重构中.....稍后发布

## 概述

专为xeon的开发工作流定制。

## 核心原则

1. **领域优先** - 理解制造业务再编码
2. **质量驱动** - 遵循项目现有规范和编码标准
3. **效率导向** - 自动化日常工作流，减少重复劳动


## 智能体路由

| 任务类型 | 推荐智能体 | 说明 |
|---------|-----------|------|
| Java 后端开发/缺陷修复 | java-backend-dev | Dubbo、MyBatis、微服务 |
| 前端开发 | frontend-dev | React、TypeScript、cjet/mdf |
| 业务知识查询 | knowledge-retriever | 制造领域知识检索 |
| 日常办公/打卡 | daily-work | Obsidian 集成、Git 提醒 |
| 大规模需求/新功能 | xeon-agent | OpenSpec + Superpowers 协调 |
| 通信管理 (实验) | chief-of-staff | 多渠道消息分类 |


## 快速开始

### 项目特定命令

- `/xeon` - OpenSpec + Superpowers 协调工作流（新需求/大变更）
- `/defect-fix` - 缺陷修复工作流（Jira 集成）
- `/daily-checkin` - 日常打卡（Obsidian 集成）
- `/knowledge-search` - 从 Obsidian vault 检索相关知识并提供上下文（inject 模式可注入完整 Markdown）
- `/project-init` - 项目初始化和环境检查
- `/save-knowledge` - 手动保存知识到 Obsidian
- `/skill-search` - 技能发现与搜索（关键词/语义）
- `/xplan` - 独立规划命令（AI 智能判断 4 档工作流：quick/standard/full/full+tasks，默认集成知识库）

### Xeon Workflow

OpenSpec + Superpowers 协调工作流，用于处理新需求或大需求变更：

```
/xeon "需求描述" --full    # 完整 5 阶段流程
/xeon "需求描述" --auto    # 智能判断阶段
/xeon --continue          # 继续未完成变更
/xeon --status            # 查看当前变更状态
```

**阶段说明**：
1. Open - 创建变更提案，生成规格文档
2. Design - 深度技术设计
3. Build - 生成计划，执行实现
4. Verify - 验证实现
5. Archive - 归档变更

**前置条件**：
- 安装 OpenSpec CLI: `npm install -g @fission-ai/openspec@latest`
- 初始化项目: `openspec init`

### 知识提取

项目支持自动知识提取，将开发过程中的有价值知识保存到 Obsidian：

| 触发场景 | 知识类型 | 说明 |
|----------|----------|------|
| fixbug 完成 | bug-fix | 缺陷修复经验 |
| skill-finder 采纳 | skill-usage | 技能使用记录 |
| daily-checkin 完成 | task-info | 任务/看板信息 |
| planner/brainstorm 完成 | plan-doc | 规划/方案文档 |
| 会话结束 (Stop hook) | (手动) | 手动确认是否提取 |

配置见 `config/obsidian-path.json` 的 `knowledgeExtraction` 字段。

### Obsidian 知识检索

通过 `knowledge-finder` 技能在编码/规划任务中检索 Obsidian 知识库作为上下文：

- 全量索引 `\ob_xeon_work` vault（H1/H2 分块，mtime 增量）
- 关键词 0.4 + SentenceTransformer(`all-MiniLM-L6-v2`) 0.6 混合检索
- 缓存与 `skill-finder` 物理隔离：`~/.cache/oh-my-chanjet/knowledge_*`
- `list` 模式返回路径+摘要；`inject` 模式返回完整 Markdown 块
- 触发：用户显式命令 `/knowledge-search` 或 `knowledge-retriever` / `java-backend-dev` / `frontend-dev` 智能体内嵌

### 项目特定技能

以下技能已从 `.claude/skills/` 复制到框架中：

| 技能 | 用途 | 说明 |
|-----|------|------|
| fixbug | 缺陷修复 | 完整的 Jira 缺陷修复工作流 |
| fixbug-flow | 修复后续 | CodeReview、提交、推送、Jira 更新 |
| gen-gql | GraphQL 生成 | 根据 BO 元数据生成 GQL 查询语句 |
| env_checker | 环境检查 | CICD 状态 + SLS 日志分析 |
| todo-manager | 任务管理 | JSON 持久化的 CRUD 操作 |
| omc-reference | OMC 参考 | Agent 目录和工具参考 |
| cc-codex-collaboration | CC+Codex协作 | CC规划+Codex实现或方案对比 |
| claude-code-skills-guide | Skills指南 | Claude Code Skills 完全指南 |
| knowledge-extractor | 知识提取 | 从会话中提取知识保存到 Obsidian |
| knowledge-finder | 知识检索 | 语义检索 (sentence-transformers) |
| knowledge-structuring-skill | 知识整理 | 非结构化材料整理为框架 |
| skill-finder | 技能发现 | 关键词+语义混合检索技能 |
| xeon-workflow | Xeon 工作流 | 5 阶段 OpenSpec+Superpowers 协调 |
| obsidian-workflow | 笔记工作流 | Obsidian 集成、日记管理 |
| manufacture-domain | 制造领域知识 | 加工单、BOM、工序管理 |
| chanjet-frontend | 前端模式 | React/mobx/Processor 模式 |
| code-review-skill | 代码审查 | 正确性、安全性、兼容性审查 |
| conventional-commit-generator | 提交信息 | 从 diff 生成 Conventional Commits |
| debug-assistant | 调试辅助 | 基于错误栈的调试帮助 |
| unit-test-generator | 单元测试 | 自动生成单元测试 |
| xplan | 独立规划 | 4 档工作流：quick/standard/full/full+tasks |
| code-documenter | 代码文档 | 自动生成代码文档 |
| workflow-automation-agent | 工作流自动化 | 目标拆解为可执行工作流 |

### MCP 服务

项目已配置以下 MCP：


## 目录结构

```
oh-my-chanjet/
├── agents/              # 项目特定智能体
├── skills/              # 项目特定技能
├── commands/            # 项目特定命令
├── hooks/               # Hooks 配置
├── rules/               # 规则（暴露给 Claude Code）
├── mcp-configs/         # MCP 配置汇总
├── config/               # 配置文件
└── docs/                # 文档
```

## 扩展指南

详见 [docs/EXTENDING.md](docs/EXTENDING.md)。
