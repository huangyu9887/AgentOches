

# 一、统一模型：四工程 = Agent产品四层设计

你可以把一个 AI Agent 产品拆成一台机器：

> Prompt = 大脑指令
> Context = 输入材料
> Harness = 操作系统
> Loop = 自动执行机制

---

## 🧠 AI Agent 产品四层架构

```
┌────────────────────────────┐
│ Loop Engineering           │ ← 行为闭环（持续执行）
├────────────────────────────┤
│ Harness Engineering        │ ← 系统能力（工具/权限/记忆/控制）
├────────────────────────────┤
│ Context Engineering        │ ← 信息供给（RAG/记忆/任务状态）
├────────────────────────────┤
│ Prompt Engineering         │ ← 指令定义（目标/角色/格式）
└────────────────────────────┘
```

---

# 二、AI Agent PRD设计模板

---

## 📌 1. 产品定义（Prompt层）

### 1.1 产品一句话定义

* 这个 Agent 是谁？
* 做什么？

> 示例：
> 一个自动完成“竞品分析报告”的 AI Research Agent

---

### 1.2 角色 Prompt（核心指令）

```
你是一个专业的 {角色}

你的目标是：
- 完成 {任务}

你的输出必须满足：
- 结构清晰
- 可执行
- 不输出无关信息

禁止行为：
- 不确定内容必须标注
- 不编造数据
```

---

## 📌 2. Context 设计（信息层）

### 2.1 输入信息结构

Agent 每次运行会收到：

* 用户输入（User Goal）
* 历史对话（Memory）
* 外部资料（RAG）
* 工具返回（Tool Output）

---

### 2.2 Context结构设计（关键）

```
Context = {
  goal: 用户目标,
  constraints: 限制条件,
  memory: 历史经验,
  retrieved_knowledge: 外部知识,
  tool_results: 当前执行结果,
  state: 当前进度
}
```

---

### 2.3 Context更新规则（很重要）

* 每一步只追加“新信息”
* 不覆盖历史关键状态
* 状态必须结构化（不是自然语言）

---

## 📌 3. Harness 设计（系统层）

这是 Agent 是否“能用”的关键。

---

### 3.1 工具系统（Tools）

你的 Agent 可以用什么能力？

* 搜索（Search）
* 写代码（Code）
* 调用API（API）
* 数据分析（SQL / Python）
* 文件操作（File system）

---

### 3.2 权限系统（Guardrails）

定义：

* 可以做什么
* 不可以做什么

```
允许：
- 读取资料
- 生成报告

禁止：
- 删除文件
- 访问用户隐私
```

---

### 3.3 状态系统（State Machine）

```
state = {
  step: 1,
  status: "thinking / acting / verifying",
  errors: [],
  retries: 0
}
```

---

### 3.4 观察系统（Observability）

记录：

* 每一步输出
* 工具调用
* 错误原因
* 是否收敛

---

## 📌 4. Loop 设计（执行层）

### 4.1 Agent标准循环

```
while not done:

    1. observe(context)
    2. plan(goal, context)
    3. act(tool call)
    4. observe result
    5. update state
```

---

### 4.2 Loop关键设计点

#### ① Stop Condition（停止条件）

必须明确：

* 什么时候结束？
* 谁判断结束？

```
if report_quality >= threshold:
    stop
```

---

#### ② Fail Recovery（失败恢复）

```
if tool_failed:
    retry(2)
    or switch_tool
```

---

#### ③ Loop防爆机制

* 最大步数 max_steps
* token budget
* timeout

---

# 三、完整 Agent PRD模板（可复制）

---

## 📄 AI Agent PRD（四工程版本）

### 1. 产品目标（Prompt层）

* 产品名称：
* 一句话描述：
* 用户目标：
* 成功标准：

---

### 2. Prompt设计

* Agent角色：
* 任务定义：
* 输出格式：
* 行为约束：

---

### 3. Context设计

* 输入字段：

  * goal
  * constraints
  * memory
  * tools_result
* context更新规则：

---

### 4. Harness设计

#### Tools：

* tool A
* tool B

#### Permissions：

* allowed:
* forbidden:

#### State：

```
step:
status:
error_log:
```

#### Observability：

* log strategy
* trace strategy

---

### 5. Loop设计

#### loop结构：

* observe → plan → act → verify

#### stop condition：

* 完成条件
* 超时条件
* 失败条件

---

### 6. 风险设计

* hallucination风险
* context drift
* tool failure
* infinite loop

---

### 7. Evaluation指标

* success rate
* steps to completion
* cost per task
* error recovery rate

---

# 四、完整示例：做一个「竞品分析Agent」

---

## 🧠 1. Prompt层

```
你是一个AI产品分析师

目标：
生成结构化竞品分析报告

输出：
- 表格 + bullet point
- 不允许编造数据
```

---

## 📦 2. Context层

```
goal: 分析Notion vs Obsidian
memory: 用户偏好简洁报告
tools:
  - web search
  - document reader
state:
  step = 1
```

---

## ⚙️ 3. Harness层

* tools：

  * search()
  * summarize()
* guardrails：

  * 禁止虚构价格
* state：

  * track competitor info
* observability：

  * log每次搜索结果

---

## 🔁 4. Loop层

```
Step 1: 搜集数据
Step 2: 提取维度
Step 3: 对比分析
Step 4: 生成报告
Step 5: 自检
```

---

## 🧪 Stop Condition

```
if report.sections == 5
and no missing data:
    finish
```

---

# 五、一句话进阶理解（非常重要）


> Prompt defines what to do
> Context defines what it knows
> Harness defines what it can do
> Loop defines how it behaves over time


