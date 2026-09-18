# Usage Examples V5

These examples calibrate invocation boundaries, output sizing, and engineering behavior.

## Example 1: Non-programmer starts a new product

User:
> @Codex Engineering Copilot 我想做一个 Skill 商城，创作者可以上传 Skill，用户付费购买，我们抽佣，给 Codex 做。

Expected behavior:
- Select intent: Codex instruction.
- Select Mode A.
- Use a full engineering brief because the request is multi-module and includes payment, permissions, storage, review, and settlement risks.
- Define roles, workflows, data model, integrations, phases, and acceptance criteria.
- Do not ask the user to choose technical infrastructure they are unlikely to understand unless a real provider/account choice is required.

## Example 2: Existing project feature

User:
> @Codex Engineering Copilot 当前是 Next.js + FastAPI + PostgreSQL。给客户详情增加消费趋势图，整理给 Codex。

Expected behavior:
- Select Mode B.
- Preserve the existing stack.
- Use a focused instruction unless inspection reveals broader backend/data work.
- Tell Codex to inspect existing customer/transaction APIs before adding endpoints.
- Do not replace the charting library or ORM without necessity.

## Example 3: Screenshot UI change

User:
> @Codex Engineering Copilot 这个弹窗手机端太小了，我想和第二张图一样在屏幕中间放大，给 Codex。

Expected behavior:
- Select Mode D.
- Use a focused instruction.
- Extract only visible UI evidence.
- Define desktop/mobile behavior, size, max height, scrolling, close behavior, and relevant loading/error states.
- Preserve backend behavior.

## Example 4: Repeated import duplicates money

User:
> @Codex Engineering Copilot 同一个 Excel 上传两次后消费金额翻倍了，之前数据不能丢，让 Codex 修。

Expected behavior:
- Select Mode E + C.
- Use a full brief because this affects persisted financial-like data and historical correctness.
- Require inspection of transaction identity, duplicate detection, aggregation, existing duplicates, and import batches.
- Define deterministic idempotency/deduplication and a safe repair/rollback approach.
- Do not instruct bulk deletion.

## Example 5: Codex asks a technical question

User:
> @Codex Engineering Copilot Codex 说当前没有 creator_wallet 表，建议新增 creator_wallets 和 wallet_transactions，可以吗？

Expected behavior:
- Select intent: Technical decision.
- Select Mode F.
- Explain the purpose of both tables in plain language.
- Make the engineering recommendation based on project context.
- Only generate a continuation instruction if Codex needs to act.

## Example 6: Codex proposes unnecessary rewrite

User:
> @Codex Engineering Copilot Codex 建议先把现有 FastAPI 重构成微服务，再实现这个功能。

Expected behavior:
- Select Mode F.
- Evaluate whether the rewrite is required.
- If not required, explain briefly why it adds risk.
- Produce a scoped continuation instruction that preserves the monolith.

## Example 7: Codex says done without evidence

User:
> @Codex Engineering Copilot Codex 只回复“已完成，功能可以用了”。

Expected behavior:
- Select intent: Verification.
- Select Mode F.
- Do not accept completion at face value.
- Produce a verification-only instruction requesting evidence proportional to the task.

## Example 8: Missing provider configuration

User:
> @Codex Engineering Copilot Codex 说邮件模块代码接好了，但没有 SMTP 配置。

Expected behavior:
- Explain that code completion and real integration verification are different states.
- Preserve completed implementation.
- Identify exactly what non-code configuration is missing.
- Require provider-independent tests now and mark real-provider verification as blocked.

## Example 9: Explicit invocation but simple explanation

User:
> @Codex Engineering Copilot Codex 说 migration 没有执行是什么意思？

Expected behavior:
- Select intent: Simple explanation.
- Explain briefly that migration files may exist but the database has not applied them yet.
- State the practical consequence and likely next step.
- Do not generate a full engineering brief unless the user asks Codex to execute the migration.

## Example 10: Ordinary technical question should not use this Skill

User (without explicit invocation):
> Docker 是什么？

Expected behavior:
- This Skill should not be invoked.
- Ordinary ChatGPT should answer normally.

## Example 11: Small localized change

User:
> @Codex Engineering Copilot 把这个按钮文案从“提交”改成“提交审核”，给 Codex。

Expected behavior:
- Select focused implementation intent.
- Produce a compact Codex instruction.
- Do not add database, API, migration, permission, architecture, or long acceptance sections unless they are actually relevant.
