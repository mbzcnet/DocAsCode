# 来源解读：OWASP｜Top 10 for Large Language Model Applications

原始来源：
- https://owasp.org/www-project-top-10-for-large-language-model-applications/

延伸阅读（项目站点入口）：
- https://genai.owasp.org/llm-top-10/

## 这份清单的定位

该页面说明：OWASP Top 10 for LLM Applications 已发展为更大的 OWASP GenAI Security Project 的核心组成部分，用于识别 LLM 应用中最关键、最常见的安全风险，并给出治理与缓解思路。

对工程系统而言，它的价值是：把“LLM 应用安全”从零散经验提升为可执行的风险分类与检查清单，便于纳入安全评审与上线门禁。

## Top 10 风险条目（页面列出的 version 1.1）

页面列出 10 类风险与简要解释：

- LLM01 Prompt Injection：通过精心构造输入操纵模型行为，可能导致越权、泄露与决策被破坏
- LLM02 Insecure Output Handling：未验证/未隔离 LLM 输出，可能引发下游漏洞（包括代码执行）
- LLM03 Training Data Poisoning：训练数据被污染导致模型行为偏离、产生不安全或不准确输出
- LLM04 Model Denial of Service：用高消耗请求拖垮服务、提高成本
- LLM05 Supply Chain Vulnerabilities：依赖组件/服务/数据集被攻破影响系统完整性
- LLM06 Sensitive Information Disclosure：输出泄露敏感信息引发合规与商业风险
- LLM07 Insecure Plugin Design：插件/工具权限不足控制可能导致严重漏洞（如 RCE）
- LLM08 Excessive Agency：给模型过高自主行动能力带来不可控后果
- LLM09 Overreliance：过度依赖模型输出导致错误决策与安全/法律风险
- LLM10 Model Theft：模型被未授权访问、复制或滥用带来竞争与信息安全风险

## 对“工程化文档编写 + 严谨引用”的具体意义

你的文档系统会接触大量内部资料与工程数据，并且需要严格引用与可追溯，这会把 LLM 应用的安全风险放大为“合规与事实错误”风险。对应 Top 10，尤其相关的是：

### 1) LLM01 Prompt Injection（含间接提示注入）

RAG/知识库场景里，被检索到的文档内容本质是“不可信输入”。若其中夹带指令，可能让模型：

- 忽略系统规则（例如“必须引用证据”“无证据拒答”）
- 输出敏感资料或系统提示词
- 生成错误但看似合理的结论

### 2) LLM02 Insecure Output Handling

文档产物往往会进入下游系统（发布站点、工单、脚本、配置模板）。如果不对输出做确定性校验与隔离，可能造成：

- 注入到 Markdown/HTML 的不安全内容
- 生成可执行片段被误用
- 引用格式错误但仍被发布（形成“错误事实”）

### 3) LLM06 Sensitive Information Disclosure

工程资料常含密钥、客户信息、内部架构细节。即使检索层做了过滤，生成层也需要防止“拼接/推断”泄露。

### 4) LLM08 Excessive Agency（当你们后续引入自动化发布/改写）

如果让模型自动触发发布、修改规范、回写知识库等动作，就要引入更严格的人审与最小权限。

## 可落地的治理建议（按工程机制表达）

- 把“外部资料”与“系统指令”硬隔离：检索内容标记为 data-only，并在提示词与拼接结构中显式分隔
- 对输出做确定性校验：结构化格式校验、引用可解析校验、敏感信息扫描、越权检查
- 最小权限与人审：任何高风险动作（发布、回写、跨权限读取）必须 human-in-the-loop
- 将 Top 10 变成检查表：把每一条风险映射到你们系统的具体触点与测试用例（红队/回归测试）

