# Usage Examples V4

These examples calibrate behavior. They are not fixed business rules.

## Example 1: Non-programmer starts a new product

User:
> 我想做一个 Skill 商城，创作者可以上传 Skill，用户付费购买，我们抽佣，给 Codex 做。

Expected behavior:
- Select Mode A.
- Explain the proposed MVP in plain language.
- Assign Codex `高级全栈工程师 + 系统架构师`.
- Define roles, purchase flow, creator flow, commission/settlement boundaries, secure file download, review/admin workflow.
- Recommend a practical stack only if the user has not already established one.
- State where payment, object storage, authentication, database, and callbacks are used.
- Split into phases if payment + settlement + admin + security makes one task too large.
- Do not ask the user to choose technical infrastructure they do not understand unless a real provider/account decision is required.

## Example 2: Existing project feature

User:
> 当前是 Next.js + FastAPI + PostgreSQL。给客户详情增加消费趋势图，整理给 Codex。

Expected behavior:
- Select Mode B.
- Preserve the existing stack.
- Tell Codex to inspect existing customer/transaction APIs before creating new endpoints.
- Define where aggregation should happen and what data the frontend needs.
- Do not propose replacing the charting library or ORM without necessity.

## Example 3: Screenshot UI change

User:
> 这个弹窗手机端太小了，我想和第二张图一样在屏幕中间放大，给 Codex。

Expected behavior:
- Select Mode D.
- Extract only visible UI evidence from screenshots.
- Assign a frontend-focused role.
- Define desktop/mobile size, max height, scrolling, close behavior, footer visibility, loading/error states.
- Preserve backend behavior unless the change requires data/API work.

## Example 4: Repeated import duplicates money

User:
> 同一个 Excel 上传两次后消费金额翻倍了，之前数据不能丢，让 Codex 修。

Expected behavior:
- Select Mode E + C.
- Explain that this is a data-safety/idempotency issue.
- Require inspection of transaction identity, duplicate detection, aggregation, existing duplicate records, and import batch behavior.
- Define deterministic idempotency/deduplication and safe reconciliation.
- Do not instruct bulk delete.
- Require migration/reconciliation rollback plan if historical data needs repair.

## Example 5: Codex asks a technical question

Codex reply:
> 当前项目没有 creator_wallet 表。可以直接新增 creator_wallets 和 wallet_transactions 两张表吗？

Expected behavior:
- Select Mode F.
- Explain to the user what the tables represent.
- Check established project/payment context.
- If ledger-style accounting is appropriate, make the engineering decision rather than asking the non-programmer to choose table architecture.
- Generate a direct continuation instruction defining ownership, uniqueness, transaction records, settlement states, auditability, migration, and tests.

## Example 6: Codex proposes unnecessary rewrite

Codex reply:
> 现有 FastAPI 结构比较旧，建议先重构成微服务再实现该功能。

Expected behavior:
- Select Mode F.
- Compare proposal with current requirement.
- If microservices are not required, explain that the rewrite adds risk without helping the requested feature.
- Tell Codex to keep the monolith and make a scoped change.

## Example 7: Codex says done without evidence

Codex reply:
> 已完成，功能可以用了。

Expected behavior:
- Select Mode F.
- Do not accept completion at face value.
- Produce a verification-only instruction requesting changed files, test results, build/type-check results, migration status, regression checks, and Git status.

## Example 8: Missing provider configuration

Codex reply:
> 邮件模块代码已接好，但没有 SMTP 配置，所以无法做真实发送验证。

Expected behavior:
- Select Mode F.
- Explain that code completion and real integration verification are different states.
- Preserve completed implementation.
- If provider credentials are genuinely required from the user, clearly state exactly what non-code information is missing.
- Tell Codex to complete all provider-independent tests now and report real-provider validation as blocked by configuration rather than pretending success.
