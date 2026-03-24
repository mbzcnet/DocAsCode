# 来源解读：LlamaIndex｜RAG 分阶段方法与核心概念

原始来源：
- Introduction to RAG：https://docs.llamaindex.ai/en/stable/understanding/rag/

延伸阅读（同站点总览页，了解其“工作流/代理/上下文增强”定位）：
- https://docs.llamaindex.ai/en/stable/

## LlamaIndex 怎么定义 RAG（工程视角）

LlamaIndex 将 RAG 解释为一种“上下文增强（context augmentation）”方式：LLM 没训练过你的数据，因此需要在推理时把与你问题相关的外部信息检索出来，连同问题一起交给 LLM 生成回答。

在它的表述里，“RAG”不是单个技巧，而是大多数数据增强型应用（QA、Chat、Agent）的底座能力。

## 五个关键阶段（非常适合用来做工程拆分）

LlamaIndex 把 RAG 拆为 5 个阶段，这个拆分几乎可以直接变成你的系统模块边界：

1) Loading（加载）
- 从数据源取回资料：文件、PDF、网站、数据库、API 等
- 本质是连接器（connector/reader）问题：怎么接、怎么增量、怎么带元数据

2) Indexing（索引）
- 将资料变成“可检索的数据结构”
- 常见实现：向量化（embeddings）+ 元数据策略（版本、来源、权限、类型等）

3) Storing（存储）
- 将索引与元数据持久化，避免每次重建
- 这一步本质决定了：能否版本化、能否回滚、能否审计复现

4) Querying（查询）
- 给定索引策略，有多种查询方式：子查询、多步查询、混合检索等
- 工程落点：路由（router）、重排（rerank）、过滤（filter）、拼接（context assembly）

5) Evaluation（评估）
- 用客观指标度量“准确、忠实、速度”，以及改动后的效果差异
- 工程落点：离线基准集 + 线上回归 + 观测指标闭环

对你们的场景（强引用严谨）而言，这 5 个阶段尤其关键，因为“引用零差错”往往卡在 Loading/Indexing/Querying 三段：资料不干净、切分不对、检索不稳，都会直接导致引用错误或证据不足。

## 关键概念：把“资料”变成可审计的“证据对象”

LlamaIndex 在 RAG 概念里强调了几个对工程化非常有用的抽象：

### Documents 与 Nodes

- Document：一个来源对象的容器（例如一个 PDF、一个 API 返回、一个网页）
- Node：最小原子单元（chunk），携带与 Document 的关联与元数据

在“严谨引用”的系统里，Node 可以自然扩展为你们的“证据对象”：

- 节点 id（稳定标识）
- 来源定位（URL/仓库路径/页码/段落/表格坐标）
- 版本语境（commit/tag、发布时间、数据截止日期）
- 权限语境（ACL/密级标签）

### Connectors / Indexes / Retrievers / Postprocessors / Synthesizers

它把一个 RAG 流程拆成可插拔部件：

- Connector：负责把外部数据拉进来
- Index：负责组织数据（向量、元数据等）
- Retriever：负责按策略检索相关片段
- Router：负责选择/组合检索器（按 query 选择最佳路径）
- Postprocessor：负责过滤/变换/重排检索结果
- Response Synthesizer：负责把“检索片段 + 问题”合成结构化答案

这与“文档工程化”的模块化思想一致：你可以分别为“引用正确性”做各阶段的可测指标，并对某一阶段做替换/优化，而不必推翻全栈。

## 对本项目的落地建议（结合“工程项目型文档”）

- 把 5 阶段拆分写进系统架构与任务列表：每个阶段都有明确输入/输出与可测指标
- 在 Node/证据对象中强制携带：来源、版本、位置、权限四类元数据
- Querying 阶段必须支持：元数据过滤（版本/权限）+ 重排（提升证据相关性）+ 引用边界（防提示注入）
- Evaluation 阶段要“产品化”：每次索引/切分/检索策略变更都触发回归评测，避免引用质量回退

