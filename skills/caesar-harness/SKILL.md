---
name: harness
description: |
  Harness Engineering — AI Agent 运行时控制系统。当用户提到以下关键词时触发：
  - "harness"、"harness engineering"、"驾驭工程"
  - "agent 运行时"、"agent 框架"、"agent 架构"
  - "long-running agent"、"长时间运行"、"agent 稳定性"
  - "Generator-Evaluator"、"Planner-Generator-Evaluator"
  - "上下文工程"、"context engineering"、"prompt engineering"
  - "MCP"、"agent protocol"、"agent 协议"
  - "deer-flow"、"agent harness"、"superagent"
  
  同时在以下场景主动触发：
  - 设计或实现 AI Agent 系统
  - 讨论如何让 agent 更可靠、更稳定
  - 构建多 agent 协作框架
  - 讨论 agent 的工具调用、反馈循环、状态管理
  - 任何关于 AI Agent 工程化的讨论
version: 1.0.0
tags: [agent, harness, ai-engineering, llm, prompt-engineering]
---

# Harness Engineering Skill

**驾驭工程** — AI Agent 时代的软件工程新范式

Harness Engineering 是系统性地构建约束、工具、文档和反馈循环的学科，使 AI 编码 Agent 能够可靠地完成工作。其核心原则是：*每次发现 Agent 犯错，就花时间设计一个解决方案，确保它不再犯同样的错误。*

---

## 核心概念速查

| 概念 | 英文 | 说明 |
|------|------|------|
| 驾驭工程 | Harness Engineering | 优化 Agent 运行环境的系统工程 |
| 上下文工程 | Context Engineering | 优化 AI 获取正确信息的能力 |
| 提示词工程 | Prompt Engineering | 优化如何与 AI 沟通 |
| 工具系统 | Tool System | 连接模型与现实世界的桥梁 |
| 反馈循环 | Feedback Loop | Agent 自我纠错的机制 |
| 状态管理 | State Management | 长时运行 agent 的记忆与连续性 |

---

## 演进路径

```
Prompt Engineering (2023) → Context Engineering (2025) → Harness Engineering (2026)
     "说什么"                      "知道什么"                    "能稳定做事"
```

---

## 六大核心组件（Claude Code 架构启发）

### 1. 多层级 System Prompt

```
┌─────────────────────────────────────────┐
│  固定缓存部分（十几万token）             │
│  - Agent 身份定义                       │
│  - Co 指令                              │
│  - 工具定义                             │
│  - 安全策略                             │
├─────────────────────────────────────────┤
│  动态可替换部分                          │
│  - 会话状态                             │
│  - 当前时间                             │
│  - 可读取文件                           │
│  - 代码包依赖                           │
└─────────────────────────────────────────┘
```

### 2. Tool Schema

- **定义**：决定工具调用准确率的核心组件
- **设计原则**：内置核心工具在模型训练阶段完成适配
- **企业级**：拒绝无权限校验的第三方工具

### 3. ToolCallLoop

Harness 最核心部分，包含两种模式：

**规划模式（Plan）**：
1. 理解任务
2. 梳理文件系统
3. 明确可用工具
4. 生成执行方案

**执行模式（Act）**：
1. 在沙盒中按规划执行工具
2. 获取结果闭环
3. 错误检测与恢复

### 4. ContextManager

- 采用**指针索引式 Memory**
- 解决百万级 token 上下文的高效利用问题
- 实现上下文压缩（compaction）

### 5. SubAgent

- 子代理分解：复杂任务拆分为多个专门子代理
- 典型架构：Planner → Generator → Evaluator

### 6. VerificationHooks

- 验证代理输出的正确性
- 自动化测试集成
- 质量门禁

---

## 核心架构模式

### Generator-Evaluator 分离模式

借鉴 GAN（生成对抗网络）思想：

```
┌─────────────┐      ┌─────────────┐
│  Generator  │ ──→ │  Evaluator  │
│  (执行代理)  │ ←── │  (评判代理)  │
└─────────────┘      └─────────────┘
```

**为什么需要分离？** AI 自我评估过于宽容，需要独立的评判者来严格验收。

---

## OpenAI 知识地图详解

### 什么是知识地图（Knowledge Map）？

知识地图是 OpenAI 在 Harness Engineering 实践中提出的核心概念，用于**系统性地组织 Agent 的约束、反馈与控制系统**。

```
┌─────────────────────────────────────────────────────────────┐
│                    Knowledge Map（知识地图）                  │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   ┌─────────────┐    ┌─────────────┐    ┌─────────────┐   │
│   │   约束层    │    │   反馈层    │    │   控制层    │   │
│   │ Constraints │    │  Feedback   │    │   Control   │   │
│   └──────┬──────┘    └──────┬──────┘    └──────┬──────┘   │
│          │                   │                   │          │
│          ▼                   ▼                   ▼          │
│   ┌─────────────────────────────────────────────────┐     │
│   │              知识库 Knowledge Base                │     │
│   │   - 项目结构    - 代码规范    - 错误模式          │     │
│   │   - 工具定义    - 验收标准    - 领域知识          │     │
│   └─────────────────────────────────────────────────┘     │
│                          │                                │
│                          ▼                                │
│   ┌─────────────────────────────────────────────────┐     │
│   │              Agent 运行时                         │     │
│   │   规划 → 执行 → 观察 → 评估 → 反馈               │     │
│   └─────────────────────────────────────────────────┘     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 知识地图的三层结构

#### 1. 约束层（Constraints Layer）

定义 Agent 能做什么、不能做什么：

```yaml
constraints:
  # 硬性约束 - 不可逾越
  hard:
    - "禁止删除非项目文件"
    - "禁止提交未经测试的代码"
    - "禁止绕过安全检查"

  # 软性约束 - 最佳实践
  soft:
    - "优先使用项目已有的工具函数"
    - "遵循项目的代码风格"
    - "保持函数短小（<50行）"
```

#### 2. 反馈层（Feedback Layer）

建立实时反馈循环：

```yaml
feedback:
  # 即时反馈
  immediate:
    - "工具调用结果"
    - "语法错误检测"
    - "类型检查失败"

  # 延迟反馈
  delayed:
    - "测试结果"
    - "代码审查意见"
    - "性能分析报告"

  # 周期性反馈
  periodic:
    - "任务完成度评估"
    - "质量指标报告"
    - "错误模式分析"
```

#### 3. 控制层（Control Layer）

指导 Agent 行为的方向盘：

```yaml
control:
  # 任务控制
  task:
    - "目标澄清"
    - "进度跟踪"
    - "异常中断"

  # 上下文控制
  context:
    - "记忆压缩"
    - "状态持久化"
    - "交接棒传递"

  # 资源控制
  resource:
    - "token 预算"
    - "执行超时"
    - "重试限制"
```

### 知识地图的核心原则

| 原则 | 说明 | 示例 |
|------|------|------|
| **形式化** | 将隐式知识转为显式规则 | "代码规范" → ESLint 规则 |
| **分层** | 按紧急程度/重要程度分层 | 硬约束 > 软约束 > 建议 |
| **可追溯** | 每个决策有据可查 | 操作日志 + 决策理由 |
| **可组合** | 小型知识单元可组合使用 | 模块化约束块 |

### 知识地图与项目知识的映射

```
Knowledge Map
     │
     ├── 项目结构知识
     │      └── 文件树、模块边界、依赖关系
     │
     ├── 领域知识
     │      └── 业务逻辑、规则、术语
     │
     ├── 代码规范知识
     │      └── 风格指南、最佳实践、模式
     │
     ├── 工具知识
     │      └── 工具定义、参数、适用范围
     │
     ├── 错误模式知识
     │      └── 历史错误、解决方案、预防措施
     │
     └── 验收标准知识
            └── 测试用例、度量指标、里程碑
```

### OpenAI 100万行代码案例中的知识地图应用

在 OpenAI 的 100 万行代码项目中，知识地图发挥了关键作用：

**1. 冲刺合同作为约束**
```yaml
sprint_contract:
  goal: "完成用户认证模块"
  acceptance: "注册、登录、登出流程可测试"
  duration: "45分钟"
  constraints:
    - "必须通过现有测试套件"
    - "禁止破坏其他模块"
```

**2. 错误分类作为反馈**
```yaml
error_patterns:
  categorize:
    - name: "网络超时"
      action: "自动重试3次"
    - name: "语法错误"
      action: "阻止提交"
    - name: "逻辑错误"
      action: "人工审查"
```

**3. 自动化工具作为控制**
```yaml
automation:
  gates:
    - "pre-commit: 格式检查 + 单元测试"
    - "pre-merge: 集成测试"
    - "pre-deploy: 端到端测试"
```

### 实施知识地图的步骤

1. **审计现有知识**
   - 梳理项目中的隐式规则
   - 访谈领域专家
   - 分析历史错误模式

2. **形式化约束**
   - 将规则转为机器可执行格式
   - 定义优先级和生效范围
   - 建立版本管理

3. **建立反馈通道**
   - 实时反馈 vs 延迟反馈
   - 自动反馈 vs 人工反馈
   - 设置反馈阈值

4. **实现控制机制**
   - 检查点机制
   - 中断恢复
   - 资源限制

5. **持续迭代优化**
   - 定期回顾知识地图有效性
   - 根据错误模式更新约束
   - 优化反馈及时性

### 知识地图工具示例

```yaml
# knowledge_map.yaml
version: "1.0"
project: "my-agent-project"

constraints:
  coding:
    max_function_length: 50
    require_tests: true
    allow_unsafe: false

  git:
    require_signoffs: ["reviewer"]
    block_on_test_failure: true

feedback:
  immediate:
    - type: "linter"
      tools: ["eslint", "ruff"]
    - type: "type_checker"
      tools: ["mypy", "typescript"]

  delayed:
    - type: "ci_pipeline"
      trigger: "on_push"
    - type: "code_review"
      trigger: "on_merge_request"

control:
  context_window: 100000
  max_retries: 3
  checkpoint_interval: 5
```

---

### 三代理架构

```
┌─────────┐    ┌────────────┐    ┌───────────┐
│ Planner │ →  │ Generator  │ →  │ Evaluator │
│ (规划器) │    │ (生成器)   │    │ (评估器)  │
└─────────┘    └────────────┘    └───────────┘
     ↓              ↓                 ↓
   任务分解      逐个实现          严格验收
```

### 冲刺合同机制

将长任务分解为可管理的"冲刺"（sprint），每个冲刺有明确目标和验收标准。

---

## 关键工程实践

### 错误处理四层模型

1. **预防层**：设计防错的工作流
2. **检测层**：实时监控 agent 行为
3. **恢复层**：自动错误恢复机制
4. **学习层**：从错误中学习并修复系统

### Context Reset 交接棒策略

长时运行 agent 采用分段交接：
- 每个阶段有明确的上下文边界
- 通过指针索引而非完整上下文传递状态
- 降低幻觉风险

### 状态管理策略

```
短期记忆 → 中期记忆 → 长期记忆
   ↓           ↓           ↓
 会话状态    任务状态    项目知识
```

---

## 质量保障体系

### 验收标准设计

对于主观任务（设计、创意），定义可量化的四维标准：
- **设计质量**：美学、专业程度
- **原创性**：创新程度、差异化
- **工艺**：细节、完善度
- **功能性**：可用性、用户体验

### 测试驱动 Agent 开发

- **单元测试**：单个工具的正确性
- **集成测试**：工具链的配合
- **端到端测试**：完整任务的质量
- **回归测试**：防止引入新错误

---

## 故障排查指南

| 问题 | 原因 | 解决方案 |
|------|------|----------|
| agent 迷失方向 | 上下文过长 | 实施 compaction、明确阶段性目标 |
| 工具调用错误 | Tool Schema 不清晰 | 简化工具定义、增加示例 |
| 重复犯错 | 缺乏反馈机制 | 引入 Evaluator、记录错误模式 |
| 上下文污染 | 边界不清 | 隔离不同任务的上下文 |
| 资源耗尽 | 长时运行 | 实现检查点机制、定期摘要 |

---

## 参考文档索引

| 文档 | 内容 |
|------|------|
| `references/core-concepts.md` | 核心概念详解 |
| `references/openai-approach.md` | OpenAI Harness Engineering |
| `references/anthropic-approacher.md` | Anthropic 官方定义与实践 |
| `references/best-practices.md` | 最佳实践与模式 |
| `references/frameworks-tools.md` | 工具与框架 |
| `references/case-studies.md` | 真实案例 |
| `references/knowledge-map-updater.md` | 知识地图更新工作流 |

---

## 快速开始

### 新项目 Checklist

- [ ] 定义 Agent 身份与目标
- [ ] 设计工具 Schema（优先内置工具）
- [ ] 实现 ToolCallLoop（规划 + 执行）
- [ ] 建立 ContextManager（压缩策略）
- [ ] 引入 Evaluator（质量门禁）
- [ ] 设置 VerificationHooks（自动测试）
- [ ] 实现错误恢复机制
- [ ] 建立监控与日志

### 设计原则

1. **每次犯错，修复系统而非责怪模型**
2. **让 agent 学会"知道自己不知道"**
3. **设计高速公路，而非试图控制每辆车**
4. **拥抱渐进式复杂度**
5. **测量一切可测量的**

---

## 延伸阅读

- OpenAI: "Harness engineering: leveraging Codex in an agent-first world"
- Anthropic: "Harness design for long-running application development"
- Mitchell Hashimoto: "每次发现 Agent 犯错，就花时间设计一个解决方案"
