```
阶段 3A：内存 ConversationStore
阶段 3B：PostgreSQL 保存 conversations/messages
阶段 5：PostgreSQL 保存 tool_calls/runs
阶段 6：PostgreSQL + pgvector 保存文档向量
阶段 7：Redis + PostgreSQL 实现短期/长期 Memory
生产规模扩大后：按需要加入 OpenSearch
```

企业智能体对话建议采用 **PostgreSQL 为主数据库，Redis 为辅助缓存，pgvector 负责向量检索**。不要把所有数据都放进向量数据库或 Redis。

```
PostgreSQL + pgvector    核心业务数据、对话、记忆、知识向量
Redis                    活跃会话缓存、锁、限流、停止生成状态
S3 / MinIO               文档、附件、图片等大文件
OpenSearch（可选）        超大规模复杂全文与混合检索
```

### 1. PostgreSQL：核心数据库

适合保存

```
tenants          企业租户
users            用户
agents           智能体配置
conversations    会话
messages         用户/AI 消息
runs             单次 Agent 运行
tool_calls       工具调用
memories         长期记忆
documents        文档元数据
audit_logs       审计日志
```

企业场景通常需要事务、关联查询、多租户隔离和审计。PostgreSQL 支持事务与并发控制、行级安全策略，并能通过 `jsonb` 保存和索引工具参数、模型配置、事件载荷等半结构化数据。([postgresql.org](https://www.postgresql.org/docs/current/mvcc.html?utm_source=openai))

推荐原则：

- `tenant_id`、`conversation_id`、`user_id`、`created_at` 等常用查询字段使用普通列。
- 工具参数、模型原始响应、扩展事件数据使用 `jsonb`。
- 不要把整个消息表设计成单个大 JSON。
- 所有主要业务表都包含 `tenant_id`，为企业隔离和权限控制做准备。

### 2. pgvector：RAG 和长期记忆

阶段 6 加入 RAG 时，直接在 PostgreSQL 安装 `pgvector`：

```
document_chunks.embedding
memories.embedding
```

`pgvector` 支持精确及近似向量检索、余弦距离、HNSW、IVFFlat，也能结合 PostgreSQL 的过滤条件和全文检索实现混合搜索。这样早期不必单独部署一个向量数据库。([github.com](https://github.com/pgvector/pgvector?utm_source=openai))

适合保存：

- 企业知识库文档切片。
- 会话摘要向量。
- 用户长期偏好。
- Agent 经验与工具执行结果摘要。

### 3. Redis：辅助，不作为最终数据源

Redis 适合：

```
active_conversation:{id}   最近几轮会话缓存
generation:{run_id}        运行状态、停止生成标志
lock:{conversation_id}     同一会话并发锁
rate_limit:{user_id}       接口限流
stream:{run_id}            临时事件流
```

Redis 支持 TTL 会话、缓存、流和发布订阅，适合跨多个应用实例共享短期状态；但完整聊天记录和审计数据仍应落入 PostgreSQL。([redis.io](https://redis.io/docs/latest/develop/use-cases/?utm_source=openai))

### 4. OpenSearch：规模扩大后再加

如果后期出现以下需求，再考虑 OpenSearch：

- 数千万以上文档切片。
- 中文复杂全文检索。
- 关键词、向量、字段权重混合排序。
- 搜索相关性调优。
- 独立搜索集群扩缩容。

OpenSearch 原生支持语义搜索、关键词与向量混合搜索，但调优依赖实际数据和用户查询，不适合作为当前阶段的默认组件。([docs.opensearch.org](https://docs.opensearch.org/latest/search-plugins/search-relevance/optimize-hybrid-search/?utm_source=openai))

