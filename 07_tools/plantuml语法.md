# 流程图

```plantuml
@startuml
start
:运行package_skill.py;

:验证skill结构;
note right
  - 检查SKILL.md存在
  - 验证YAML frontmatter
  - 检查name和description字段
  - 验证命名规范
end note

if (验证通过?) then (否)
  :报告错误;
  :修复问题;
  stop
endif

:打包为.skill文件;
note right
  - 创建zip压缩包
  - 保持目录结构
  - 重命名为.skill
end note

:输出打包路径;
stop
@enduml
```

# 组件图

```plantuml
@startuml
[Database] as db
[Web Server] as ws
[Client] as client

client --> ws : uses
ws --> db : accesses
@enduml

```


# 类图

```plantuml
@startuml
class User {
  - name: String
  - age: int
  + getName(): String
  + setName(name: String): void
}

class Order {
  - orderId: String
  + process(): void
}

User "1" -- "many" Order : places
@enduml
```

* ​**符号说明**​：
  * `-`：私有属性/方法
  * `+`：公有属性/方法
  * `--`：关联关系
  * `: places`：关联标签
  * `"1" -- "many"`：多重性（可选）

# 状态图

```plantuml
@startuml
[*] --> Idle
Idle --> Running : start
Running --> Idle : stop
Running --> Error : exception
Error --> Idle : reset
@enduml
```

# 用例图

```plantuml
@startuml
actor User
actor Admin

User --> (Login)
User --> (View Profile)
Admin --> (Manage Users)

(User, Admin) --> (Logout)
@enduml
```

* ​**符号说明**​：
  * `actor`：参与者
  * `-->`：关联关系
  * `()`：用例（功能）

# 时序图

```plantuml
@startuml
participant Alice
participant Bob

Alice -> Bob: 你好
Bob --> Alice: 你好，收到
Alice -> Bob: 请执行操作
Bob -> Bob: 内部处理
Bob --> Alice: 操作完成
@enduml

```

* ​**符号说明**​：
  * `->`：同步消息
  * `-->`：返回消息
  * `->>`：创建对象（`new`）
  * `-->>`：销毁对象

# 高级语法

## ​注释

```plantuml
' 单行注释
package "wiki-summarizer" {
  file "SKILL.md" as skill_md
  folder "references" as refs
}

```

## 颜色与样式

```plantuml
skinparam defaultTextAlignment center
skinparam shadowing true

'定义note样式
skinparam note {
    BackgroundColor #yellow
    BorderColor #black
    BorderThickness 1
}

class User #LightBlue
class Order #FFBBBB
User --> Order : [color: red]

note right of User
colourful
end note
```

## 继承与接口

```plantuml
interface Payment {
  + pay(amount: double): void
}

class CreditCardPayment
CreditCardPayment <|-- Payment
```

## 组合/聚合

```plantuml
class Car {
  - engine: Engine
}
class Engine

Car *-- Engine : contains
```

# test1

```plantuml
@startuml
package "wiki-summarizer" {
  file "SKILL.md" as skill_md
  folder "references" as refs
  file "plantuml-templates.md" as plantuml
  file "html-styles.md" as html
  file "wiki-structure.md" as wiki
}

skill_md --> refs : 引用模板
refs --> plantuml : PlantUML图表库
refs --> html : HTML/CSS样式库
refs --> wiki : Wiki文档结构

note right of skill_md
  核心instructions
  - 核心能力说明
  - 使用指南
  - 工作流程
  - 质量标准
end note

note right of plantuml
  6种图表类型:
  - 泳道图
  - 时序图
  - 流程图
  - 状态图
  - 组件图
  - 部署图
end note

note right of html
  6种样式类型:
  - 垂直时间线
  - PROMPT输入框
  - 进度对比图表
  - 经验总结框
  - 可折叠详情
  - 表格样式
end note
@enduml
```

# test2

```plantuml
@startuml

left to right direction

rectangle "Short-term memory" as STM
rectangle "Long-term memory" as LTM
rectangle "Memory" as Memory
STM --|> Memory
LTM --|> Memory


rectangle "Tools" as Tools
rectangle "Agent" as Agent
rectangle "Planning" as Planning
rectangle "Action" as Action

Memory --|> Tools
Tools --|> Agent
Agent --|> Planning
Planning --|> Action

rectangle "Reflection" as Reflection
rectangle "Self-criticism" as SelfCriticism
rectangle "Chain of thoughts" as ChainOfThoughts
rectangle "Subgoal decomposition" as SubgoalDecomposition

Planning --|> Reflection
Planning --|> SelfCriticism
Planning --|> ChainOfThoughts
Planning --|> SubgoalDecomposition

rectangle "Calendar()" as Calendar
rectangle "Calculator()" as Calculator
rectangle "CodeInterpreter()" as CodeInterpreter
rectangle "Search()" as Search
rectangle "...more" as More

Tools --|> Calendar
Tools --|> Calculator
Tools --|> CodeInterpreter
Tools --|> Search
Tools --|> More

Agent -[dashed,#gray]-> Action

@enduml
```

# test3

```plantuml
@startuml
skinparam defaultTextAlignment center
left to right direction

' 中心节点
rectangle "Agent" as Agent #f9d5d5
rectangle "Memory" as Memory #f0f0f0
rectangle "Planning" as Planning #f0f0f0
rectangle "Action" as Action #f0f0f0
rectangle "Tools" as Tools #f0f0f0

' 记忆子系统
rectangle "Short-term memory" as ShortTerm #f0f0f0
rectangle "Long-term memory" as LongTerm #f0f0f0

' 工具列表
rectangle "Calendar ()" as Calendar #f0f0f0
rectangle "Calculator ()" as Calculator #f0f0f0
rectangle "CodeInterpreter ()" as CodeInterpreter #f0f0f0
rectangle "Search ()" as Search #f0f0f0
rectangle "...more" as MoreTools #f0f0f0

' 规划子系统
rectangle "Reflection" as Reflection #f0f0f0
rectangle "Self-critics" as SelfCritics #f0f0f0
rectangle "Chain of thoughts" as ChainOfThoughts #f0f0f0
rectangle "Subgoal decomposition" as SubgoalDecomposition #f0f0f0

' 连接关系
' Agent 与 Memory
Agent --> Memory
' Agent 与 Planning
Agent --> Planning
' Agent 与 Action
Agent --> Action
' Action 与 Tools
Agent --> Tools

' Tools 与具体工具
Tools --> Calendar
Tools --> Calculator
Tools --> CodeInterpreter
Tools --> Search
Tools --> MoreTools

' Memory 与短期/长期记忆
Memory --> ShortTerm
Memory --> LongTerm

' Planning 与规划子系统
Planning --> Reflection
Planning --> SelfCritics
Planning --> ChainOfThoughts
Planning --> SubgoalDecomposition

' 反馈循环
Memory -[dashed,#gray]-> Planning
Memory -[dashed,#gray]-> Reflection

' 工具与 Action 的反馈
Tools -[dashed,#gray]-> Action

@enduml

```

# test4

```plantuml
@startuml
skinparam graphengine twopi

[Center] as center
[Node X] as x
[Node Y] as y

center --> x
center --> y

' 禁用 y 与 center 的边对布局的影响
y -[hidden] center
@enduml

```
