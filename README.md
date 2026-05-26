# SpecX

SpecX 是一个轻量级 Codex 工作区，用于把不确定的产品输入转化为可落地的后端架构 Spec。

这个仓库不是应用代码仓库。它主要沉淀代理约束、可复用 skills、架构澄清参考、评审清单和模板，让 Codex 以“后端设计伙伴”的方式工作，而不是只做文档排版。

## 仓库内容

```text
specx/
├── AGENTS.md
├── README.md
└── .agents/
    └── skills/
        ├── backend-architecture-refiner/
        │   ├── SKILL.md
        │   ├── references/
        │   │   ├── architecture-clarification-bank.md
        │   │   └── architecture-review-checklist.md
        │   └── templates/
        │       ├── ownership_matrix_template.md
        │       ├── phase_0_input_acquisition_template.md
        │       ├── phase_1_context_template.md
        │       ├── phase_2_runtime_template.md
        │       ├── phase_3_contract_template.md
        │       ├── runtime_risk_template.md
        │       └── state_machine_template.md
        └── lark-doc-reader/
            └── SKILL.md
```

GitHub 分支有意不包含 `specx/` 目录下生成的项目专属 Spec 产物。这里保留的是可复用的代理工作流资产。

## 核心流程

SpecX 围绕分阶段的后端设计流程组织：

1. 获取并规整输入
   - PRD、会议纪要、飞书/Lark 文档、已有契约，或者粗略的产品想法。
2. 澄清上下文与归属
   - 服务边界、数据归属、领域职责和集成关系。
3. 收敛运行时语义
   - 状态流转、并发、幂等、重试、一致性、失败行为和可观测性。
4. 冻结实现契约
   - API、RPC、事件、任务、持久化模型、兼容性要求和测试策略。

## 关键文件

- `AGENTS.md`
  - 项目级代理行为约束。
  - 要求代理进行批判性分析、明确指出问题、澄清职责边界，并输出面向工程落地的后端设计。

- `.agents/skills/backend-architecture-refiner/SKILL.md`
  - 主要的后端架构收敛 workflow。
  - 通过 Phase 0 到 Phase 3，把原始输入推进到可以冻结契约的架构设计。

- `.agents/skills/backend-architecture-refiner/references/architecture-clarification-bank.md`
  - 架构澄清问题库。
  - 用于识别缺失决策、薄弱假设和会影响架构的歧义。

- `.agents/skills/backend-architecture-refiner/references/architecture-review-checklist.md`
  - 架构评审清单。
  - 用于在把 Spec 视为可实施之前，检查边界、契约、运行时语义和风险是否足够清晰。

- `.agents/skills/backend-architecture-refiner/templates/`
  - 分阶段 Spec、归属矩阵、运行时风险和状态机等 Markdown 模板。

- `.agents/skills/lark-doc-reader/SKILL.md`
  - 通过 `lark-cli` 读取飞书/Lark 文档的工作流。
  - 用于把外部文档转化为可继续分析的设计输入。

## 设计原则

- 把输入材料当作证据，而不是真理。
- 在润色文档之前先暴露矛盾。
- 在定义契约之前先澄清归属。
- 优先选择能满足需求的最简单设计。
- 显式记录假设、被拒绝的方案和未解决的架构风险。
- Spec 必须服务于研发实现，而不只是用于展示。

## Skills 预期产物

架构收敛流程应产出实用的后端设计材料，例如：

- 职责边界和数据归属
- 领域模型和持久化假设
- API、RPC、事件和任务契约
- 存在持久化数据时的 ER 图
- 存在生命周期状态时的状态图
- 主流程和失败流程的时序图
- 幂等、重试、并发和事务边界决策
- 迁移、兼容、回滚、审计和可观测性说明
- 单元测试、集成测试、契约测试和验收测试策略

## 仓库范围

这个 GitHub 分支只用于维护可复用的 SpecX 工作流资产：

- 代理行为说明
- 架构收敛 skills
- Lark 文档读取工作流
- 可复用参考资料和模板

项目专属的生成 Spec 默认不放在这个分支中，除非它们被明确作为公开示例发布。
