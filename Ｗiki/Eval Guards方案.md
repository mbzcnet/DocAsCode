<br />

**eval guards方案**：RAG实时评估防护栏，检测幻觉、相关性、事实性，自动阻挡错误输出，提升准确率至近100%（结合SME）。

**NVIDIA NeMo Guardrails**：开源框架，支持RAG grounding、幻觉检测（Patronus Lynx）、输入/输出moderation、jailbreak防护，低延迟集成。

**AWS Bedrock Guardrails**：托管服务，上下文grounding checks（99%事实验证）、内容过滤、PII redaction、Automated Reasoning，直接集成Bedrock KB RAG评估。

<br />

**NVIDIA NeMo Guardrails**：开源Python工具包（Apache 2.0），为LLM应用添加可编程防护栏（rails），拦截输入/输出，确保安全、可靠、对齐。专为RAG提供grounding、幻觉检测、jailbreak防护、低延迟。

**核心功能**：

- **5类rails**：Input（输入过滤、jailbreak）、Dialog（对话流控制）、Retrieval（RAG chunk过滤）、Execution（工具调用门控）、Output（事实检查、幻觉阻挡、PII redaction）。
- 使用**Colang**（DSL语言）定义行为流，支持Colang 2.0。
- 集成LangChain、LlamaIndex、LangGraph；支持NIM微服务和第三方模型（如Patronus Lynx幻觉检测）。

**实施方案**（简要步骤）：

1. **安装**：`pip install nemoguardrails`（Python 3.10+），或用Docker/Helm部署微服务（生产推荐Kubernetes）。
2. **配置**：创建`config.yml`指定LLM模型；用Colang文件定义rails流程。
3. **集成RAG**：在检索后加retrieval rails过滤，在生成前/后加output rails验证。
4. **部署**：本地Docker Compose测试；生产用Helm chart + GPU加速，集成OpenTelemetry监控。
5. **评估**：内置eval工具测合规率、延迟、token使用。

适用于企业RAG，提升准确率，减少幻觉30-45%。结合SME验证可接近100%可靠。
