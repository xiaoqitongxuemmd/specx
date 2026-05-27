# SpecX

SpecX 是一个面向后端设计协作的轻量工作区，用于让 Codex 在不完整 PRD、会议纪要、Lark 文档等输入材料基础上，输出可落地的后端工程 Spec。

这里不是应用代码仓库，也不是文档堆放目录。它的核心目标是约束代理行为、沉淀 Spec 模板，并把“需求分析 -> 设计澄清 -> 工程化 Spec”这条链路收敛成可复用流程。

## 适用场景

- 根据 PRD、会议纪要或临时想法，整理后端实现方案
- 对不完整或相互冲突的需求做澄清和收敛
- 输出面向研发实现的 feature / module / microservice Spec
- 在读取 Lark/Feishu 文档后，将内容转化为后端设计输入材料

## 目录结构

```text
specx/
├── AGENTS.md
├── README.md
├── specx/
│   └── vms/
│       └── vms-control-plane/
│           ├── phase_1_context.md
│           ├── phase_2_runtime_semantics.md
│           └── phase_3_contracts.md
└── .agents/
    └── skills/
        ├── backend-architecture-refiner/
        │   ├── SKILL.md
        │   ├── references/
        │   └── templates/
        └── lark-doc-reader/
            └── SKILL.md
```

## 关键文件说明

- [AGENTS.md](/home/mini/Project/PDCL4/specx/AGENTS.md)
  - 项目的主代理约束。
  - 明确要求代理以“后端设计伙伴”而不是“文档打字员”的方式工作。
  - 约束内容包括：批判性分析、冲突识别、职责边界、澄清节奏、Spec 完成标准等。

- [.agents/skills/backend-architecture-refiner/SKILL.md](/home/mini/Project/PDCL4/specx/.agents/skills/backend-architecture-refiner/SKILL.md)
  - 分阶段（Phase 0~3）的后端架构收敛技能。
  - 强调先澄清再设计，聚焦运行时语义、归属边界、一致性与契约冻结。

- [.agents/skills/lark-doc-reader/SKILL.md](/home/mini/Project/PDCL4/specx/.agents/skills/lark-doc-reader/SKILL.md)
  - 用于通过 `lark-cli` 读取飞书/Lark 文档的技能说明。
  - 包含 CLI 安装、初始化、用户授权、Wiki/Doc 读取，以及首次配置完成后同步部署到用户态 Codex skill 目录的约束。

## 工作方式

建议按下面的方式使用这个仓库：

1. 提供输入材料
   - 可以是 PRD、飞书文档链接、会议纪要、已有接口说明，或者口头需求。

2. 让代理先做分析而不是直接润色
   - 先提取后端相关事实、识别冲突和边界问题，再进入设计。

3. 基于模板输出 Spec
   - 按 `feature`、`module` 或 `microservice` 选择合适粒度。

4. 在 Spec 中明确记录不确定项
   - 包括：已确认决策、开放问题、默认假设、拒绝方案、主要风险。

## 设计原则

本仓库默认遵循以下原则：

- 输入材料是证据，不是真理
- 优先澄清系统边界，而不是先补格式
- 能简化的设计不要复杂化
- 高影响决策必须显式确认
- Spec 面向工程落地，不能停留在抽象表述

## 对代理的要求

如果你在这个仓库中扩展新的 skill、模板或说明，建议保持以下约束一致：

- 不替用户掩盖需求冲突
- 不把架构级问题伪装成“默认值”
- 不在职责边界未清晰时直接给出最终方案
- 不把 Spec 写成纯产品文档
- 有状态流就补状态机，有持久化就补 ER 图，有主流程就补时序图

## Lark 文档读取说明

当前仓库内置了 `lark-doc-reader` skill，用于辅助读取飞书/Lark 文档：

- 优先尝试本地 `lark-cli`
- 缺失时由代理按步骤安装和初始化
- 用户授权失效时引导重新登录
- 首次本地配置成功后，可将 skill 同步部署到用户态 Codex skill 目录

注意：
部署到用户态的 skill 应保持为运行时版本，不应带有项目内用于“自部署”的维护说明。

## 当前状态

当前仓库更接近“Spec 工具仓”而不是“完整产品仓”：

- 已有主代理约束
- 已有架构收敛 skill（`backend-architecture-refiner`）
- 已有 Lark 文档读取 skill
- 已有 VMS 控制面分阶段 Spec 产物（Phase 1/2/3）
- 尚未引入更多自动化脚本

## 已产出示例

- [phase_1_context.md](/home/mini/Project/PDCL4/specx/specx/vms/vms-control-plane/phase_1_context.md)
  - 上下文、职责边界、通信模式与运行约束。
- [phase_2_runtime_semantics.md](/home/mini/Project/PDCL4/specx/specx/vms/vms-control-plane/phase_2_runtime_semantics.md)
  - 运行时语义、状态机、重试/回放/失败语义、ER 图与实体字段草案。
- [phase_3_contracts.md](/home/mini/Project/PDCL4/specx/specx/vms/vms-control-plane/phase_3_contracts.md)
  - HTTP 与 Kafka 契约、幂等规则、错误码与可观测性约定。

## 后续可扩展方向

- 增加示例 Spec，覆盖 feature / module / microservice 三类场景
- 增加面向事件驱动、批处理、数据同步等专项模板
- 补充将输入材料自动整理为 Spec 初稿的脚本或命令约定
- 把 skill 的“文档说明”进一步固化为可重复执行的自动化流程
