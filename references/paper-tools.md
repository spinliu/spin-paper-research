# 论文搜索工具参考

## L1 轻量级 Web Search

**适用场景**：快速了解一个新主题，确定关键词，摸清研究框架。

### 使用 online-search Skill

读取 skill：`~/Library/Application Support/QClaw/openclaw/config/skills/online-search/SKILL.md`

执行轻量搜索，关键词组合：
```
# 主题 + 核心概念
{研究主题} {核心概念} review
{研究主题} latest research 2024 2025

# 中文补充
{中文主题} 研究进展 综述 最新
```

输出：关键词列表、相关概念、研究方向概览。

### 使用 multi-search-engine Skill（无 API Key）

读取 skill：`~/Library/Application Support/QClaw/openclaw/config/skills/multi-search-engine/SKILL.md`

多引擎并发搜索，适合快速摸底。

---

## L2 专业论文搜索

**适用场景**：精准定位论文、找高引论文、整理文献综述。

### PubMed（生物医学首选）

```bash
# 高引论文（按引用排序）
curl --noproxy '*' -fsSL --max-time 15 \
  "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=pubmed&term={关键词}&retmode=json&retmax=20&sort=relevance"

# 获取论文详情
curl --noproxy '*' -fsSL --max-time 15 \
  "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esummary.fcgi?db=pubmed&id={PMID1,PMID2}&retmode=json"

# 获取摘要
curl --noproxy '*' -fsSL --max-time 15 \
  "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esummary.fcgi?db=pubmed&id={PMID}&retmode=json"
```

PMC 全文：
```bash
curl --noproxy '*' -fsSL --max-time 15 \
  "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=pmc&term={关键词}&retmode=json&retmax=10"
```

### arXiv（计算机科学/物理/数学）

```bash
# 搜索最新论文
curl --noproxy '*' -fsSL --max-time 15 \
  "https://export.arxiv.org/api/query?search_query=all:{关键词}&max_results=10&sortBy=submittedDate&sortOrder=descending"

# 按相关性排序
curl --noproxy '*' -fsSL --max-time 15 \
  "https://export.arxiv.org/api/query?search_query=all:{关键词}&max_results=10&sortBy=relevance"
```

### Crossref（学术综合）

```bash
curl --noproxy '*' -fsSL --max-time 15 \
  "https://api.crossref.org/works?query={关键词}&rows=10&select=DOI,title,author,published-print,is-referenced-by-count,container-title"
```

按引用量排序示例：
```bash
curl --noproxy '*' -fsSL --max-time 15 \
  "https://api.crossref.org/works?query=hypochlorous+acid&rows=10&select=DOI,title,author,published-print,is-referenced-by-count,container-title&sort=is-referenced-by-count"
```

### 高引论文筛选标准

| 引用量 | 论文类型 | 价值 |
|--------|---------|------|
| >100 | 奠基性综述/开创性工作 | 必读 |
| 20-100 | 重要进展/方法验证 | 优先读 |
| 5-20 | 新兴方向/子领域 | 按需读 |
| <5 | 最新进展（<2年） | 关注前沿 |

---

## L3 Tavily AI Search（深度调研）

**适用场景**：生成结构化调研报告、跨方向综述、识别研究空白。

### 配置检查

```bash
# 检查 API Key
echo $TAVILY_API_KEY

# 如果未配置，提示用户：
# "Tavily 需要 API Key 才能使用。请到 https://tavily.com 注册免费账号，获取 Key 后配置。"
```

### 执行深度调研

```bash
python3 ~/.qclaw-oversea/workspace/skills/tavily/scripts/tavily_search.py \
  "{研究主题} comprehensive research review" \
  --depth advanced \
  --max-results 10 \
  --include-answer
```

### 调研报告生成

Tavily 输出结构化结果后，整理成：

```
## {主题} 深度调研报告

### 一、主题概述
{领域定义、研究背景、重要性}

### 二、关键研究问题
{核心问题列表}

### 三、主流方法分类
{按方法类型分组，每组列出代表论文}

### 四、各方向代表论文
{表格：论文名 | 作者/年份 | 核心贡献 | 引用量}

### 五、当前研究空白
{哪些问题还没被解决}

### 六、可关注的前沿方向
{最有潜力的研究方向}
```

---

## 搜索词优化策略

1. **从泛到精**：先用宽泛词摸底，再缩小到子方向
2. **中英混用**：中文关键词找中文资源，英文找国际文献
3. **同义词替换**：同一概念不同表达都搜一遍
4. **引文追踪**：找到一篇好论文后，追踪它的引用和被引

## 输出规范

每次 L2 及以上搜索，输出必须包含：
- 论文标题、作者、年份、期刊/会议
- 引用量或影响力指标
- 核心贡献一句话
- PMC/arxiv/DOI 链接（方便后续精读）