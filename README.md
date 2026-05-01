# spin-paper-research

论文科研全链路研究技能。

围绕一个研究主题，提供从快速摸底到深度精读的全套工具链。

## 五层工具链

| 层级 | 工具 | 何时用 | 输出 |
|------|------|--------|------|
| L1 轻量 | 轻量级 Web Search | 快速了解主题、找关键词 | 相关论文列表 + 关键词 |
| L2 专业 | PubMed / arXiv / Crossref | 精准定位论文、找高引论文 | 论文列表 + 元数据 + 摘要 |
| L3 深度 | Tavily AI Search | 深度调研、跨学科综述 | 结构化报告 + 来源分析 |
| L4 精读 | ljg-paper（论文精粹）| 单篇论文深度解读 | 结构化 Org 笔记 |
| L5 溯源 | ljg-paper-river（论文倒读法）| 理解论文脉络与演化 | 演化树 + 正向费曼叙事 |

## 安装

将 `spin-paper-research/` 整个文件夹放入 OpenClaw 的 skills 目录：

```bash
cp -r spin-paper-research/ ~/.qclaw-oversea/workspace/skills/
```

## 使用

当你说"研究 XXX 论文"或"帮我调研这个主题"时，skill 会自动走完 L1→L2→L3 流程，并在末尾提示是否进一步做精读（ljg-paper）或溯源（ljg-paper-river）。

## 依赖

- L3 Tavily AI Search 需要 `TAVILY_API_KEY` 环境变量（免费注册：https://tavily.com）
- 其他层级无需额外配置

## 版本

- **v0.1** — 初始版本，包含 SKILL.md + 3 个 references 文件

## 引用

如果对你有帮助，欢迎 star！