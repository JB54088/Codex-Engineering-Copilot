# Codex Engineering Copilot Output Patterns V5

Choose the smallest pattern that safely satisfies the request. Do not force a full engineering brief onto every invocation.

## Pattern S: Simple explanation

Use when the user explicitly invoked the Skill but only wants to understand a Codex message, technical option, warning, or narrow concept.

Output:
1. Direct explanation in plain language.
2. Recommended next action only if one is needed.

Do not generate a Codex prompt unless the user wants Codex to act.

---

## Pattern F: Focused implementation instruction

Use for one narrow feature, bug, UI adjustment, or localized backend change with no major data/money/migration risk.

### Part 1 — 给用户看的简短说明

Explain:
- what will change
- the recommended approach
- any material risk or constraint

Keep this short.

### Part 2 — 可直接复制给 Codex 的执行指令

Include only applicable items:
- Codex role
- exact outcome
- current known context
- Codex must inspect before changing
- affected module/layer
- required behavior
- what must remain unchanged
- acceptance criteria
- relevant tests/checks
- concise completion report

Do not include empty sections merely to make the prompt look comprehensive.

---

## Pattern A: Full engineering brief

Use only for a new product, multi-module workflow, high-risk data/permission/payment/migration change, or when the user explicitly asks for a complete engineering brief.

### Part 1 — 给用户看的方案说明

Keep this understandable to a non-programmer.

Include only applicable points:
1. 这次要做什么
2. 我建议怎么做
3. 关键技术/调用关系
4. 需要注意的风险
5. 是否需要分阶段

### Part 2 — 可直接复制给 Codex 的完整工程指令

Use applicable sections from this structure:

1. Codex 身份与工作方式
2. 项目背景与业务目标
3. 已确认事实 / 工程建议 / Codex 必须核查
4. 技术栈与架构约束
5. 本次范围与明确不做的内容
6. 系统/数据流与调用关系
7. 前端要求
8. 后端与业务逻辑要求
9. 数据库与数据安全
10. API 与外部服务
11. 权限与角色边界
12. 状态流转
13. 异常和边界情况
14. 禁止事项
15. 验收标准
16. 测试和验证
17. 完成报告

Omit sections that are genuinely irrelevant instead of filling them with boilerplate.

For existing projects always require:
`inspect -> plan -> implement -> verify`

For new projects require a phase/module plan before broad implementation.

---

## Pattern C: Codex follow-up / continuation

Use when the user pastes Codex's latest response.

### 1. 当前状态
State what Codex actually verified, changed, failed, or asked.

### 2. 这意味着什么
Explain the blocker, decision, failure, conflict, risk, or missing evidence in plain language.

### 3. 下一步判断
Decide what should happen next based on established project constraints.

Preserve completed work. Do not restart verified modules.

### 4. 给 Codex 的下一条指令
Provide one copy-ready continuation instruction only when Codex needs to act.

If Codex claims completion but evidence is incomplete, produce a verification-only instruction.

---

## Evidence-sizing rule

Require evidence proportionate to the task.

Tiny/localized change:
- changed file(s)
- relevant check/test
- build/typecheck only if applicable

Normal feature/bug:
- changed files
- key logic
- relevant tests
- build/typecheck/lint as applicable
- regression result
- Git status if relevant

High-risk/multi-module:
- changed files
- schema/migrations
- external config/providers
- tests
- build/typecheck/lint
- runtime/integration checks
- regression checks
- remaining risks
- Git status

Do not require migration, provider, or database reporting when those layers are not involved.
