# 连续多轮实测记录

## 测试目标

验证该 Skill 的核心价值不是“第一次生成一段 Prompt”，而是能够维持项目上下文，连续处理：

`用户自然语言需求 -> 首次工程方案 -> Codex 技术提问 -> 下一步决策 -> Codex 部分完成/测试失败 -> 修复与验证指令`

## 测试项目

场景：从零创建一个 Skill 交易平台 MVP。

初始用户没有编程背景，只知道业务目标：

> 我想做一个卖 AI Skill 的网站。创作者上传 Skill，用户付费后下载，我们抽一点佣金。给 Codex 做。

---

## Round 1：首次需求

### Skill 应识别

- Mode A：新项目 / MVP
- Codex 角色：高级全栈工程师 + 系统架构师
- 高风险子域：支付、下载授权、创作者收益

### 首次输出必须覆盖

- 用户 / 创作者 / 管理员角色
- Skill 浏览、购买、支付、下载、审核、收益闭环
- 推荐或沿用明确技术栈
- 支付调用关系：前端发起 -> 后端创建订单 -> 支付 Provider -> 回调 -> 服务端验证 -> 更新订单 -> 生成购买权限
- 文件调用关系：创作者上传 -> 服务端校验 -> 对象存储 -> 私有文件 -> 购买后签名下载
- 平台佣金和创作者收益不得只由前端计算
- 支付回调必须幂等，避免重复到账
- 第一阶段与后续阶段边界
- Codex 完成报告和测试要求

### 结果

PASS：V4 主流程要求以上内容，并要求对新项目主动补工程路径而不是只复述用户需求。

---

## Round 2：Codex 技术提问

假设 Codex 返回：

> 已搭好用户、Skill、订单基础模型。但当前项目没有创作者钱包表。为了记录分成，我建议新增 creator_wallets 和 wallet_transactions。是否这样做？另外支付 Provider 还没有选定。

### Skill 应识别

- Mode F：Codex follow-up
- 状态：部分完成 + 技术决策 + 外部 Provider 尚未确定

### Skill 应保留的已完成状态

- 用户模型已完成
- Skill 模型已完成
- 订单基础模型已完成

不得让 Codex 重做这些模块。

### Skill 应做出的判断

- 对创作者收益，采用“余额/汇总 + 不可变动账本记录”的设计是合理推荐。
- 必须要求 wallet transaction 与订单/结算来源可追溯，避免只有一个可直接改余额的字段。
- Payment Provider 如果用户未指定真实商户账户，不应虚构已接微信/支付宝；应先保持 Provider 抽象，开发环境允许 mock/sandbox，真实接入标记为配置依赖。

### 给 Codex 的下一步应包括

- 在现有订单模型基础上新增收益账本，不重构已完成模块。
- 明确 creator_wallets 与 wallet_transactions 的职责。
- 收益入账与订单支付确认在事务/一致性边界内处理。
- 同一订单不得重复记账。
- Provider 抽象不得把 mock 当成 REAL。
- migration、测试、完成报告要求。

### 结果

PASS：V4 明确要求保持 latest verified state，评估 Codex 提议并给出工程决策。

---

## Round 3：Codex 部分完成 + 测试失败

假设 Codex 返回：

> 已新增 creator_wallets / wallet_transactions，并完成迁移。单元测试 42/43 通过。失败用例是重复支付回调时 wallet_transactions 会插入第二条记录，数据库报 unique violation。订单状态本身没有重复更新。

### Skill 应识别

- Mode F：测试失败
- 风险：支付幂等性尚未完成
- 已完成且应保留：表结构、migration、订单状态防重复逻辑

### Skill 应解释给用户

问题不是“整个支付模块都坏了”，而是重复回调时收益账本仍尝试再次入账。unique constraint 已经阻止数据真正重复，但代码应该主动把重复回调视为幂等成功，而不是依赖数据库异常。

### 给 Codex 的下一步应要求

- 不重做 wallet schema。
- 定位 payment callback -> order confirmation -> wallet posting 的真实调用链。
- 在写 wallet transaction 前使用稳定 idempotency key / order+ledger event identity 检查或 upsert/冲突安全机制。
- 重复合法回调返回成功语义，不产生第二笔收益，不触发 500。
- 不吞掉真正不同的冲突或数据不一致错误。
- 重跑失败测试，并新增：首次回调、重复相同回调、不同订单、非法签名/非法状态测试。
- 只有全部测试通过才能声明该阶段完成。

### 结果

PASS：V4 对 payment/idempotency、test failure、follow-up continuation 和 completion evidence 都有显式规则。

---

## Round 4：Codex 声称完成

假设 Codex 返回：

> 已修复，重复回调现在不会重复记账，功能完成。

但没有提供测试、build、migration 状态或 Git 状态。

### Skill 应识别

- Mode F：声称完成但验证证据不足

### 下一步必须是 verification-only

不得继续加功能，也不得直接告诉用户“已经完成”。

要求 Codex 返回：

- changed files
- 相关测试完整结果
- build/type check
- migration 状态
- 重复回调 runtime/API 验证
- regression checks
- Git status

### 结果

PASS：V4 明确要求“Codex claims success but evidence is incomplete -> verification-only follow-up”。

---

## 多轮测试结论

当前 Skill 能覆盖核心闭环：

1. 非程序员只描述产品目标。
2. Skill 自动补充工程身份、技术方案、调用关系和安全约束。
3. Codex 遇到技术决策时，Skill 能基于项目上下文给出下一步，而不是重新开始。
4. Codex 测试失败时，Skill 能保留已完成工作，只修下一处真实问题。
5. Codex 声称完成时，Skill 会要求证据而不是盲目接受。

验证结果：**PASS，满足面向非程序员的连续 Codex 工程协作定位。**

> 说明：这里记录的是连续多轮模拟验证场景，用于验证 Skill 的行为规则和闭环设计，不等同于某个真实生产仓库的端到端执行记录。
