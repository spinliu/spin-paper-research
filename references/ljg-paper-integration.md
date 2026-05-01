# ljg-paper 论文精粹集成

## 何时触发

用户要求对指定论文进行深度解读时触发。

典型触发词：
- "精读这篇论文"
- "深度分析这篇"
- "帮我读一下"
- "这篇论文讲了什么"
- 发送 arXiv 链接 / PubMed PMID / PDF
- "翻译这篇论文"

## 执行流程

### Step 1：读取 ljg-paper Skill

读取：`~/.claude/skills/ljg-paper/SKILL.md`

### Step 2：获取论文内容

**arXiv 链接**：
```bash
curl --noproxy '*' -fsSL --max-time 15 \
  "https://export.arxiv.org/api/query?id_list={arxiv-id}" | head -100
```

**PubMed PMID**：
```bash
# 获取摘要
curl --noproxy '*' -fsSL --max-time 10 \
  "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esummary.fcgi?db=pubmed&id={PMID}&retmode=json"

# 获取 PMC 全文（如有）
curl --noproxy '*' -fsSL --max-time 10 \
  "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=pmc&term={PMID}[pmcid]&retmode=json&retmax=1"
```

**PDF 文件**：直接用 Read 工具读取。

### Step 3：按 ljg-paper 格式输出

ljg-paper 的 7 个 section：
1. **问题** — 具象开场，让读者亲历困境（用同一例子贯穿）
2. **翻译** — 不重述问题，重心是"如何做到"（核心机制 + 关键结果）
3. **核心概念** — 1-3 个必须过的概念，回到例子上落地
4. **洞见** — 思想结晶，一句话脱离上下文仍有力量
5. **博导审稿** — 选题眼光、方法成熟度、实验诚意、判决
6. **启发** — 迁移/混搭/反转三视角，能动手而非能想
7. **模板 Org 文件头** — Denote 规范，写入 `~/Documents/notes/`

### Step 4：输出时提示进阶选项

精读完成后，提示：

```
✅ 完成 {论文标题} 的精读。

如需继续深入：

🌊 论文溯源（ljg-paper-river）— 倒着挖这篇论文的来龙去脉，正着讲问题演化史

📄 继续精读其他论文 — 发送另一篇论文的链接或标题
```

## 多篇论文处理

当用户要求"精读多篇"时：

1. 先列出所有论文，统一做 L2 搜索获取元数据
2. 按引用量和时间排序
3. 逐一执行精读（每篇独立输出一个 Org 文件）
4. 最后给出多篇横向比较的洞见

## 注意事项

- ljg-paper 输出语言跟随用户原始请求（默认中文）
- 每个 Org 文件记录时间戳，保存到 `~/Documents/notes/`
- 不确定的地方诚实说"没看懂"，不硬编
- 博导审稿部分说好说差，不做老好人