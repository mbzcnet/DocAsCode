# 来源解读：OpenTelemetry｜Traces（分布式追踪的核心概念）

原始来源：
- Traces（概念页）：https://opentelemetry.io/docs/concepts/signals/traces/

延伸阅读（规范：Tracing API，细化“TracerProvider/Tracer/Span”的定义与约束）：
- https://opentelemetry.io/docs/specs/otel/trace/api/

## 这套标准解决什么问题

OpenTelemetry 将 Trace 定义为“一个请求在系统中的完整路径”，用于理解一次请求从进入到结束期间经过了哪些组件、每一步做了什么、耗时与失败在哪里。

这对任何“多步骤、多服务”的系统都很关键：单体应用也好、微服务也好，只有把事件串起来，才能定位端到端问题。

## Trace / Span 的核心抽象（概念页要点）

### Trace

- Trace 是由多个 Span 构成的集合
- Trace 通过 trace_id 将多个 Span 串在一起
- parent_id 表示父子层级关系（根 Span 没有 parent）

概念页用示例说明：同一个 trace_id 的多个 Span，通过 parent-child 关系表示一次操作的层级结构。

### Span

Span 表示一个“工作单元/操作”，包含：

- 名称（name）
- 起止时间（start/end）
- SpanContext（trace_id、span_id、flags、tracestate 等）
- 属性（attributes）
- 事件（events）
- 链接（links）
- 状态（status）
- 类型（SpanKind：Client/Server/Internal/Producer/Consumer）

概念页强调：Span 很像结构化日志，但它具备“上下文关联、层级与跨进程关联”的能力。

## 规范层面强调的工程约束（Tracing API 页要点）

Tracing API 规范给出稳定（Stable）的组件定义与行为边界：

- TracerProvider：Tracer 工厂，持有配置（通常应用生命周期内初始化一次）
- Tracer：创建 Span
- Span：追踪操作的 API（记录 attributes、events、status，最终 End）

另外规范强调：

- Span 名称应避免高基数（不要把 user_id 直接拼进 span name；应作为 attribute）
- SpanContext 需要符合 W3C Trace Context 语义（TraceId/SpanId/TraceFlags/TraceState）
- “没有 SDK 时”API 行为应是 no-op（便于库在无观测配置时仍可安全调用）

## 对本项目（AI 工程化文档编写）的直接价值

你的目标是工程化文档编写，尤其强调“引用严谨、不可出错”。这类系统的典型链路往往是多阶段流水线：

- 资料接入/解析（ingest/parse）
- 切分与索引（chunk/index）
- 查询与检索（retrieve/rerank/filter）
- 生成与校验（generate/validate/cite）
- 发布/回写（publish）

在这种系统里，“为什么错/错在哪/证据从何而来”必须可审计。Traces/Spans 很适合把“每次生成/改写”的执行过程记录为可追踪链路：

- 每次检索：查询、过滤条件、命中文档、命中 chunk、重排结果
- 每次生成：用到的证据对象集合、拒答原因、输出结构化校验结果
- 每次发布：产物版本、关联的索引快照、是否通过门禁

这样你可以用一个 trace_id 把“引用的证据对象”与“生成路径”关联起来，从而支持：

- 事故复盘（哪次索引/哪条证据导致错误）
- 质量回归（哪次策略变更导致 groundedness 下降）
- 合规审计（某结论引用了哪些来源、是否越权）

## 建议的 Span 设计（落地提示）

- 每次“文档生成/改写请求”作为根 Span（Internal/Server）
- 子 Span：retrieve、rerank、synthesize、citation_map、policy_check、publish
- 把高基数信息放在 attributes：doc_id、index_version、source_ids、chunk_ids（注意脱敏与 ACL）
- 为失败路径记录 status=Error，并附失败分类（引用缺失、越权、证据不足、格式不合规等）

