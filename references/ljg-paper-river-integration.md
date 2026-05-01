# ljg-paper-river 论文倒读法集成

## 何时触发

用户要求理解一篇论文的学术脉络、问题演化史、或说"来龙去脉"时触发。

典型触发词：
- "这篇论文的来龙去脉"
- "论文溯源"
- "倒读这篇"
- "这篇论文的前世今生"
- "理解这篇论文的脉络"
- "这篇论文站在谁肩上"

## 执行流程

### Step 1：读取 ljg-paper-river Skill

读取：`~/.claude/skills/ljg-skills/skills/ljg-paper-river/SKILL.md`

### Step 2：获取目标论文

```bash
# PubMed
curl --noproxy '*' -fsSL --max-time 10 \
  "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esummary.fcgi?db=pubmed&id={PMID}&retmode=json"

# arXiv
curl --noproxy '*' -fsSL --max-time 15 \
  "https://export.arxiv.org/api/query?id_list={arxiv-id}" | head -80
```

### Step 3：提取批判链线索

重点读目标论文的：
- 引言（introduction）中批判前人工作的部分
- Related Work / Background 对前人方法的评述
- 找出它明确说"前人 X 有问题 Y"的地方

锁定核心前序论文（通常 1-3 篇）。

### Step 4：递归溯源（最多 5 层）

对每篇核心前序论文重复溯源：
- 它在批判谁？
- 它在改进谁？
- 递归直到：该领域的奠基论文，或已达 5 层

可用 PubMed/Crossref API 获取每层论文的基本信息。

### Step 5：前沿延伸

反方向找目标论文的后续工作：
```bash
# 在 PubMed 中搜索引用了目标论文的后续论文
curl --noproxy '*' -fsSL --max-time 10 \
  "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=pubmed&term={PMID}[pmid]&retmode=json&retmax=5"
```

### Step 6：正向费曼叙事

从最老的奠基论文开始，正向讲。每篇讲三件事：
1. 它看到了前人方案的什么具体问题
2. 它的解法核心思路
3. 这个解法留下了什么新问题（过渡到下一篇）

### Step 7：输出 ljg-paper-river 格式

按 ljg-paper-river 的格式输出，包含：
- 两张 ASCII 图：溯源地图 + 问题-解法总览
- 正向费曼叙事（以问题演化为线索）
- 整条线的洞见

输出文件：`{时间戳}--paper-river-{简短标题}__paper_river.org`
保存到：`~/Documents/notes/`

## 注意事项

- 最多递归 5 层，找不到就停
- 不确定的关系诚实说，不编造
- 重点在"差异驱动叙事"——每篇论文的讲解重心是"它和前一篇的差异在哪"
- 如果目标论文是开创性工作（无前序），只做前沿延伸