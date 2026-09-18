# Codex Engineering Copilot

> **面向非程序员的 Codex 工程协作助手**

如果你会用自然语言描述“我想做什么”，但不会写代码、不会整理完整开发需求、不会选技术，也不知道 Codex 提问后该怎么回答，这个项目就是为这种场景设计的。

**正式名称：Codex Engineering Copilot**  
**定位：面向非程序员的 Codex 工程协作助手**

---

## 它解决什么问题

普通用户往往只会说：

> “我想做一个 Skill 商城，创作者上传，用户购买，我们抽佣。”

但 Codex 真正需要的通常还包括：

- 以什么工程身份执行
- 项目应该如何拆分
- 前端、后端、数据库分别怎么做
- 用什么技术，以及为什么
- 哪个环节调用什么服务
- API、权限、状态流转怎么设计
- 数据怎么保存、去重和回滚
- 哪些功能不能被破坏
- 怎么测试、验收、证明真的完成

Codex Engineering Copilot 的作用就是把这两者连接起来：

```text
用户说想要什么
        ↓
Codex Engineering Copilot
补齐工程方案、技术路径和约束
        ↓
Codex 执行开发
        ↓
Codex 返回问题 / 报错 / 测试结果
        ↓
Codex Engineering Copilot
判断下一步并生成继续执行指令
        ↓
Codex 继续开发
```

它不是一次性的 Prompt 润色器，而是一个持续的工程协作闭环。

---

# 一、什么时候使用

适合以下场景：

- 你不会写代码，但想让 Codex 做网站、后台、AI 产品或软件。
- 你已经有一个项目，需要新增功能。
- 你只有一句自然语言需求，不知道该怎么拆成开发任务。
- 你有页面截图，想让 Codex 按目标效果修改。
- 按钮、接口、导入、权限、数据库出现 Bug。
- 你拿到了报错、日志或测试失败结果，但看不懂。
- Codex 问你“要不要新增表 / 换技术 / 重构”，你不知道怎么决定。
- Codex 说“已完成”，但你不知道它是否真的完成。
- 你需要在多轮开发中持续保留前面的项目上下文。

支持六种主要工作模式：

1. 新项目 / MVP
2. 已有项目新增功能
3. Bug / 报错修复
4. UI / 交互改造
5. 数据库 / 权限 / 支付 / 迁移等高风险修改
6. Codex 后续回复继续处理

---

# 二、输入什么

不需要写专业 Prompt，直接说人话即可。

例如：

```text
我想做一个 Skill 商城，创作者可以上传 Skill，
用户付费后下载，我们抽取一点佣金，给 Codex 做。
```

或者：

```text
这个页面我想改成第二张图这样，
中间弹出来更大、更清晰，手机上也要好看。
```

或者：

```text
同一份 Excel 上传两次以后金额翻倍了，
历史数据不能删，后面新的消费还要继续追加。
```

也可以直接提供：

- 截图
- 报错
- 日志
- 代码片段
- 数据库结构
- API 信息
- 当前技术栈
- 项目文件
- Codex 的完整回复

---

# 三、会输出什么

默认输出两层内容。

## 第一层：给非程序员看的方案说明

用普通语言告诉你：

- 这次真正要解决什么
- 建议怎么实现
- 为什么这样实现
- 应该沿用或使用哪些技术
- 哪个环节调用什么服务
- 有没有数据、权限、支付、迁移等风险
- 是否应该拆成多个开发阶段

## 第二层：可以直接复制给 Codex 的完整工程指令

其中会明确：

- Codex 应该扮演什么工程角色
- 项目背景和业务目标
- 已确认事实
- Codex 必须先检查什么
- 技术栈和架构约束
- 前端 / 后端 / 数据库 / API 要求
- 数据流和第三方调用关系
- 权限和角色边界
- 状态流转
- 异常处理
- 幂等、事务、并发等安全要求
- 禁止修改的范围
- 可验证的验收标准
- 测试 / TypeScript / Build / Migration / 回归要求
- Codex 完成后必须返回的证据

---

# 四、怎么继续交给 Codex

第一次：

```text
自然语言需求
  ↓
Codex Engineering Copilot
  ↓
复制“给 Codex 的完整工程指令”
  ↓
发送给 Codex
```

Codex 回复以后，不要自己猜怎么回答。

把 Codex 的完整回复继续发给 Codex Engineering Copilot，例如：

```text
Codex 说：
当前项目没有 creator_wallet 表，
我建议新增 creator_wallets 和 wallet_transactions，
是否继续？
```

Copilot 会判断：

- 已完成
- 部分完成
- 被配置阻塞
- 需要技术决策
- 测试失败
- 架构冲突
- 数据迁移风险
- Codex 想做不必要的重构
- 声称完成但验证证据不足

然后生成**下一段可以直接回复 Codex 的指令**。

核心循环：

```text
用户需求
→ Copilot
→ Codex
→ Codex 回复
→ Copilot
→ Codex
→ 验证完成
```

---

# 五、5 个典型案例

完整案例见 [references/case-studies.md](references/case-studies.md)。

### 1. 新项目

用户只说：

> 我想做一个 Skill 商城。

Copilot 会补齐角色、交易闭环、技术栈、支付、对象存储、订单、下载权限、创作者收益和开发阶段。

### 2. UI 修改

用户提供当前页面和目标截图。

Copilot 会转换成明确的前端工程任务，包括桌面端、移动端、弹窗、滚动、加载状态和需要保持不变的后台行为。

### 3. Bug 修复

用户说：

> 按钮点了没反应。

Copilot 不会直接猜根因，而是要求 Codex 追踪：

```text
按钮事件
→ 前端状态
→ API 请求
→ 后端处理
→ 数据更新
```

再修真实根因并补回归测试。

### 4. 数据库 / 导入问题

用户说：

> 同一个 Excel 导入两次金额翻倍了。

Copilot 会识别这是幂等、去重、历史数据安全问题，要求 Codex 处理唯一身份、重复导入、事务、旧数据修复和回滚，而不是简单删除数据。

### 5. Codex 技术提问后的继续协作

Codex 问：

> 要不要新增 creator_wallets 和 wallet_transactions？

Copilot 会根据前文业务目标做工程判断，并直接给出下一步数据库职责、约束、迁移和测试方案，而不是让不会写代码的用户自己选。

---

# 六、连续多轮实测

完整记录见 [references/multi-turn-validation.md](references/multi-turn-validation.md)。

当前已完成一套**连续多轮模拟验证**：

```text
用户自然语言需求
        ↓
首次工程方案
        ↓
Codex 技术提问
        ↓
Copilot 给出技术决策
        ↓
Codex 部分完成 / 测试失败
        ↓
Copilot 保留已完成内容，只修真实问题
        ↓
Codex 声称完成但证据不足
        ↓
verification-only 验证
```

测试场景：从零创建一个 Skill 交易平台 MVP。

覆盖：

- 新项目技术方案
- 创作者钱包 / 收益账本
- Payment Provider 未确定
- 重复支付回调
- 幂等性
- 单元测试失败
- Codex “已完成”但缺少测试证据
- 最终 verification-only 检查

**模拟验证结果：PASS。**

这项测试验证的不是“能不能第一次生成 Prompt”，而是能否在多轮 Codex 开发过程中持续保留项目状态并给出下一步。

---

# 七、项目文件

```text
Codex-Engineering-Copilot/
├── README.md
├── SKILL.md
├── agents/
│   └── openai.yaml
├── assets/
│   └── icon.svg
└── references/
    ├── user-guide.md
    ├── case-studies.md
    ├── multi-turn-validation.md
    ├── output-template.md
    └── usage-examples.md
```

- [使用说明](references/user-guide.md)
- [5 个典型案例](references/case-studies.md)
- [连续多轮验证](references/multi-turn-validation.md)
- [输出模板](references/output-template.md)
- [行为示例](references/usage-examples.md)

---

# 八、核心原则

> **用户负责描述想要的结果，Copilot 负责补齐工程实现路径，Codex 负责执行代码修改。**

同时遵循：

- 新项目：主动补齐工程方案。
- 已有项目：优先沿用现有技术栈。
- 不把猜测当成项目事实。
- 不为了“高级”而过度设计。
- 不让非程序员承担不必要的技术选择。
- 高风险数据 / 支付 / 权限操作必须有保护措施。
- 大任务拆阶段。
- Codex 的一句“已完成”不等于真正完成。
- 最终完成必须有测试、构建、迁移、运行或回归证据。

---

## 当前版本

**Codex Engineering Copilot V4**

正式定位：

> **面向非程序员的 Codex 工程协作助手**
