# Codex Engineering Copilot

> **面向非程序员的 Codex 工程协作助手**

如果你会用自然语言描述“我想让 Codex 做什么”，但不会写代码、不会整理完整开发任务，也不知道 Codex 提问后该怎么回答，这个项目就是为这种场景设计的。

**正式名称：Codex Engineering Copilot**  
**当前版本：V5**

---

## 它解决什么问题

Codex 真正执行开发时，经常需要明确：

- 这次到底要改什么
- 现有项目哪些东西不能动
- 前端、后端、数据库分别怎么处理
- 权限、状态、数据安全怎么设计
- 哪些地方要先检查再改
- 怎么验证是否真的完成

Codex Engineering Copilot 的作用是：

```text
用户说想要什么
        ↓
Codex Engineering Copilot
判断任务类型和复杂度
        ↓
给出合适大小的工程说明 / Codex 指令
        ↓
Codex 执行
        ↓
Codex 返回问题、报错、测试或完成结果
        ↓
Codex Engineering Copilot
判断下一步
```

它不是普通 ChatGPT 的替代品，也不是“所有软件问题都生成长 Prompt”的工具。

---

# 一、怎么触发

这个 Skill 只应该在用户明确：

`@Codex Engineering Copilot`

或者明确说“使用 Codex Engineering Copilot”时启用。

普通问题，例如：

- Docker 是什么？
- React 和 Vue 有什么区别？
- 这段代码什么意思？
- 这个报错代表什么？

默认应该由普通 ChatGPT 回答，而不是自动进入 Codex 工程模式。

---

# 二、最适合的用途

1. **让 Codex 执行开发**  
   把自然语言需求整理成可直接执行的工程指令。

2. **分析 Codex 回复**  
   看懂 Codex 的阻塞、问题、测试失败和建议。

3. **替用户做技术决策**  
   当 Codex 问“要不要加表 / 换方案 / 重构”时给出工程判断。

4. **验证 Codex 是否真的完成**  
   根据测试、build、migration、runtime、Git 等证据判断。

---

# 三、输出不再一律很长

V5 增加了输出分级。

### 1. 简短解释

用于理解 Codex 回复、技术选项、警告或一个窄问题。

只回答真正需要的内容，不生成完整工程 Brief。

### 2. 精简 Codex 指令

用于小 Bug、局部 UI、单个接口、一个明确功能。

输出：
- 简短说明
- 一段可直接复制给 Codex 的指令

### 3. 完整工程 Brief

只用于：
- 新项目 / MVP
- 多模块功能
- 数据库 / 权限 / 支付 / 迁移等高风险修改
- 用户明确要求完整开发指令

---

# 四、核心工作模式

支持：

1. 新项目 / MVP
2. 已有项目新增功能
3. Bug / 失败行为
4. UI / 交互改造
5. 数据库 / 权限 / 支付 / 迁移等高风险修改
6. Codex 后续回复继续处理

已有项目统一遵循：

`inspect -> plan -> implement -> verify`

先检查真实实现，再计划，再修改，最后验证。

---

# 五、明确不负责什么

- 不替代普通 ChatGPT 做一般技术问答。
- 不把每一个软件问题都转成 Codex Prompt。
- 不替代 Codex 实际修改代码。
- 不在没有证据时假装知道真实文件名、函数名、数据库字段或根因。
- 不为了“显得高级”强行引入微服务、Redis、Kafka、Kubernetes 等技术。
- 不把 Codex 一句“已完成”当作真正完成。
- 不把无关项目上下文混入当前项目。
- 不让已验证完成的模块反复重做。

---

# 六、使用方式

第一次：

```text
@Codex Engineering Copilot
我想让 Codex 给客户详情页增加消费趋势图。
```

Skill 会先判断这是小改动还是大任务，再给出对应大小的指令。

Codex 回复后：

```text
@Codex Engineering Copilot
Codex 回复如下：
……
```

Skill 会继续判断：

- 是否真的完成
- 是否部分完成
- 是否被配置阻塞
- 是否需要技术决策
- 是否测试失败
- 是否存在架构或迁移风险
- 是否只是缺少验证证据

然后只给下一步需要的内容。

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

---

# 八、核心原则

> **用户负责描述想要的结果，Copilot 负责补齐工程实现路径，Codex 负责执行代码修改。**

V5 额外强调：

> **只有明确调用时才工作；简单问题简单回答；只有真正需要开发时才生成 Codex 工程指令；只有复杂或高风险任务才使用完整 Brief。**
