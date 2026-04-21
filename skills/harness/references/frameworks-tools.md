# Harness Engineering 工具与框架

---

## 开源框架

### DeerFlow (字节跳动)

**定位**：Long-horizon SuperAgent Harness

**特点**：
- 开源地址：github.com/bytedance/deer-flow
- 编排子代理、记忆和沙盒
- 支持研究和代码任务
- 模块化设计

**核心架构**：
```
输入 → Planner → Sub-agents → Memory → Sandboxes → 输出
```

**适用场景**：需要长时运行的研究型 Agent

---

### LangGraph (LangChain)

**定位**：构建有状态的多角色应用

**特点**：
- 基于图的工作流
- 支持循环和条件分支
- 内置状态管理
- 与 LangChain 生态集成

**核心概念**：
```python
from langgraph.graph import StateGraph

graph = StateGraph(TaskState)
graph.add_node("planner", planner_node)
graph.add_node("generator", generator_node)
graph.add_node("evaluator", evaluator_node)
graph.add_edge("planner", "generator")
graph.add_edge("generator", "evaluator")
```

---

### AutoGen (Microsoft)

**定位**：构建多 Agent 应用

**特点**：
- 多 Agent 对话框架
- 工具支持
- 人机协作
- 丰富的示例库

**适用场景**：需要多 Agent 协作的任务

---

### CrewAI

**定位**：多 Agent 编排

**特点**：
- Role-based Agent
- 任务共享和协作
- 流程可视化
- 简单易用

---

### Semantic Kernel (Microsoft)

**定位**：企业级 AI 编排

**特点**：
- 规划器（Planner）
- 内存集成
- 技能（Skills）概念
- .NET 优先

---

## Agent 协议

### MCP (Model Context Protocol)

**定位**：Agent 与外部工具的标准连接协议

**架构**：
```
┌─────────┐    MCP     ┌─────────┐
│  Host   │ ◀─────────▶│  Client │
│ (模型)   │            │ (工具)   │
└─────────┘            └─────────┘
```

**核心消息类型**：
- `initialize` - 初始化连接
- `tools/list` - 列出可用工具
- `tools/call` - 调用工具
- `resources/*` - 资源访问

### ACP (Agent Communication Protocol)

**定位**：Agent 之间通信的协议

**功能**：
- Agent 发现
- 消息路由
- 能力协商

### A2A (Agent-to-Agent)

**定位**：企业级 Agent 互操作

**特性**：
- 标准化接口
- 任务委托
- 状态同步

---

## 开发工具

### 工具类

#### 1. ToolBench

评估 Agent 工具使用能力的基准测试。

#### 2. API-Bank

工具增强型 LLM 的基准测试，包含 53 个常用工具。

#### 3. ToolAlpaca

开源的多工具学习基准。

---

## 测试框架

### 1. RL-Eval

强化学习风格的 Agent 评估框架。

### 2. AgentBench

多领域 Agent 评估基准。

### 3. WebArena

Web 环境下 Agent 任务的评估平台。

### 4. Windows Agent Arena

Windows 操作环境中的 AI Agent 性能基准。

---

## 监控与可观测性

### 追踪工具

#### 1. LangSmith

LangChain 官方监控平台。

#### 2. Weights & Biases (W&B)

ML 实验追踪，可用于 Agent 行为分析。

#### 3. Phoenix (Arize)

LLM 应用的可观测性平台。

### 日志系统

#### 1. Structured Logging

```json
{
  "timestamp": "2026-04-21T10:30:00Z",
  "level": "INFO",
  "agent_id": "agent_001",
  "event": "tool_call",
  "tool_name": "read_file",
  "latency_ms": 45,
  "success": true
}
```

---

## 沙盒与隔离

### 沙盒方案

| 方案 | 特点 | 适用场景 |
|------|------|----------|
| Docker | 轻量、标准化 | 通用隔离 |
| gVisor | 强隔离、快速 | 安全敏感 |
| Firecracker | 微型 VM | AWS Lambda 风格 |
| WebAssembly | 跨平台、高效 | 浏览器内运行 |

### 资源限制

```yaml
sandbox:
  cpu_limit: "2 cores"
  memory_limit: "4GB"
  disk_limit: "10GB"
  network: "none"  # 或 "limited"
  timeout: "30min"
```

---

## 记忆系统

### 短期记忆

```typescript
interface ShortTermMemory {
  sessionId: string;
  recentEvents: MemoryEvent[];
  currentTask: TaskState;
  contextWindow: TokenWindow;
}
```

### 长期记忆

```typescript
interface LongTermMemory {
  projectKnowledge: KnowledgeGraph;
  agentPreferences: PreferenceMap;
  errorPatterns: ErrorPattern[];
  successfulStrategies: Strategy[];
}
```

### 记忆检索

| 方法 | 说明 |
|------|------|
| Vector Search | 基于语义相似度 |
| BM25 | 关键词匹配 |
| Knowledge Graph | 关系型检索 |
| Hybrid | 混合多种方法 |

---

## 评估工具

### 1. RAGAS

RAG 系统评估框架。

### 2. Giskard

LLM 模型评估和测试。

### 3. TruLens

神经网络可解释性和评估。

### 4. DeepEval (Confluence)

单元测试风格的 LLM 评估。

---

## 安全与权限

### 权限模型

```yaml
permission_model:
  default: "deny"
  
  tools:
    read_file:
      - require: ["path_validation"]
      - allow: ["project_directory"]
    
    write_file:
      - require: ["path_validation", "backup"]
      - allow: ["project_directory"]
      - deny: ["system_files"]
    
    execute_command:
      - require: ["command_whitelist"]
      - deny: ["dangerous_commands"]
```

### 审计日志

```json
{
  "audit_entry": {
    "timestamp": "2026-04-21T10:30:00Z",
    "agent_id": "agent_001",
    "action": "write_file",
    "resource": "/project/config.yaml",
    "approved_by": "human_review",
    "risk_level": "medium"
  }
}
```

---

## 框架选型指南

| 场景 | 推荐框架 |
|------|----------|
| 快速原型 | LangGraph / CrewAI |
| 企业级应用 | Semantic Kernel / AutoGen |
| 长时研究任务 | DeerFlow |
| 多 Agent 协作 | AutoGen / LangGraph |
| 简单任务流 | CrewAI |
| 复杂状态管理 | LangGraph |
