# agent原理

## agent技术在LLM应用范式的分级



| LLM应用范式分级 | 人工参与程度                                         | AI参与程度                               | 示例                       |
| --------------- | ---------------------------------------------------- | ---------------------------------------- | -------------------------- |
| L1 无LLM        | 95%                                                  | 5% 非LLM的传统AI辅助参与                 |                            |
| L2 ChatBot      | 80%~90% 提问                                         | 10%~20% 根据提问给出信息                 | ChatGPT/AskAl/GenieAl      |
| L3 Copilot      | 50% 给出Prompt/修改Prompt                            | 50% 根据Prompt给出初稿并优化             | GitHub/Copilot/MidJourney  |
| L4 Agent        | 20% 给出初始目标，监督过程、评估结果，提供接口等资源 | 80% 自主完成任务分解，选择工具，自我迭代 | HuggingGPT/AutoGPT/BabyGPT |
| L5 AGI          | 给出目标                                             | 根据目标自主完成所有工作                 | 冯诺依曼机器人             |

##　agent技术的发展

![image-20260228093659391](http://image.huawei.com/tiny-lts/v1/images/hi3ms/43be05d10c3f9efed6fdf6029d48d267_1051x603.png)

##　AI Agent定义

```mermaid
graph LR
    Agent[Agent] --> Memory[Memory]
    Agent --> Planning[Planning]
    Agent --> Action[Action]
    Agent --> Tools[Tools]

    Memory --> ShortTerm[Short-term memory]
    Memory --> LongTerm[Long-term memory]
    Memory -.-> Reflection
    Memory -.-> Planning
    
    Planning --> Reflection[Reflection]
    Planning --> SelfCritics[Self-critics]
    Planning --> ChainOfThoughts[Chain of thoughts]
    Planning --> SubgoalDecomposition[Subgoal decomposition]
    
    Tools -.-> Action
    Tools --> Calendar[Calendar &#40&#41]
    Tools --> Calculator[Calculator &#40&#41]
    Tools --> CodeInterpreter[CodeInterpreter &#40&#41]
    Tools --> Search[Search &#40&#41]
    Tools --> More[...more]
```

- 百度的定义 以大语言模型为大脑驱动的系统，具备自主理解、感知、规划、记忆和使用工具的能力，能够自动化执行完 成复杂任务的系统 

- 学术界的定义 Agent需要拥有记忆、工具、计划能力、执行能力等，主要用于自主完成任务，定义较为广泛。 

- OpenAl的定义 不直接称呼Agent概念，在OpenAl 2023/11发布会上发布Assistant，定义为具备知识库与工具的个人助理，但 实现逻辑本质也是Agent。


智能体 = 环境感知 + LLMs + 规划 + 记忆 + 工具，即人类的 五感接收信息+大脑思考+四肢执行。

应用：场景化封装智能体

能力下限：可调用工具的多少，即有多少agent API。

能力上限：MML能力，即思考+规划。

## Agent核心能力

- 环境感知能力：

文本处理能力（分析/对话）

图片（文字/颜色/布局）chatgpt 4 vision多模态

语音（语调/语气/语速等处理）chatgpt 4 o

视频处理能力（故事情节/前因后果）

- 自主规划能力：

阶段一、COT思维链/TOT思维树

阶段二、workflow（pipline提前规划流程，可依托Coze、dify、fastgpt等进行平台 进行pipleline构建）+多智能体（分项处理，Langchain、RAGflow等框架集成支持）

阶段三、thinking模型（deepseek R1深度思考）

阶段四、ALL in LM（google gemini的deepReaserch端到端多模态）

- 工具调用能力

阶段一、Function Call（工具开发API，agent调用API）根据工具指定的接口开发，要提前适配工具

阶段二、模型学习人使用电脑（学习使用电脑屏幕 → 学习使用浏览器→分析拆解指令，逐步执行思考）manus

阶段三、接口标准化（MCP，模型上下文协议）根据标准接口开发，可自动检索API市场，自动调用，C/S模式

MCP 工作流示例

```mermaid
graph LR
    A[MCP Host] --> B[MCP Client]
    B --> C{资源发现}
    C -->|ListResources| D[MCP Server 1]
    C -->|ListResources| E[MCP Server 2]
    B --> F[LLM]
    F --> G[生成工具调用]
    G --> H[ExecuteTool]
    H --> D
    H --> E
```

Function Calling 工作流示例

```mermaid
graph LR
    A[App] --> B[定义函数 Schema]
    B --> C[LLM]
    C --> D[解析函数调用]
    D --> E[本地执行函数]
    E --> A
```

长远看MCP和FunctionCall会共存，模型资源壁垒，性能问题

- 记忆能力

短期记忆：当前对话上下文

长期记忆：历史对话结果，对于模型能力有要求，能处理的token数量

## Agent落地流程

![image-20260228112427719](http://image.huawei.com/tiny-lts/v1/images/hi3ms/19b4035b413d1cf149399ceb391ee10d_1583x791.png)

一个agent应用的使用流程：

结果评估占用1/3-1/4时间。

##　Agent应用范式

- 知识检索。如RAG检索增强生成，将知识库和问题通过embedding模型检索生成提示内容，再交给LLM总结生成回答。
- 工具调用。买机票/电影票等，通过MCP/Function Call将工具注册到LLM，通过LLM思考调用。
- NL2SQL。使用自然语言进行数据库查询检索。
- Code Interpreter。代码解释器。

Code Interpreter可以对文件进行处理，通过代码运行，降低了幻觉和迷感的概率。 

Code Interpreter让 Al 的用途更加广泛，用户不必过多"编程”。

## Agent派别

通用派：Manus/GPT-5 全能

垂直派：医疗/法律/金融等，单一领域

## Agent应用

### 通用智能体

政府公文：RAG+历史公文，

政府办事大厅：语音识别+RAG，精准定位诉求，如办出生证明

### 专业智能体

中医药知识问答/病理符合度判断

# 提示词工程

## 定义

prompt engineering：常规prompt是指app输入的界面词；这里的工程指通过API接口给LLM发出指令

token：自然语言里最小单元是汉字/单词；LLM里最小单元是token，不同模型使用不同分词器将单词分割为token。

hidden prompt：app会对用户的prompt进行一层封装，添加一些隐形提示词，达到补全/限制等作用。

prompt工程参数：

Temperature：[0-1]用于控制返回结果的确定性，越小越确定。

Top_p：用于控制返回结果的真实性，越小越真实。

max_length：控制生成的token数。

Stop Sequence：控制生成列表的项数。控制响应长度和结构。

Frequency Penalty：频率惩罚，通过token曾出现的频率来控制下一个token出现的概率，值越大惩罚越高。

Presence Penalty：存在惩罚，控制token是否出现过来控制下一个token出现的概率，值越大惩罚越高。

## 模型分类

一、

推理大模型：明确任务目标和需求即可。

非推理大模型：通用模型。显示引导推理步骤。依赖提示词

二、CoT思维链

概率预测。快速反应

链式推理。慢速思考

## 提示词架构

选择/设计适合自己/项目使用的架构，以下是一些架构举例：

RTGO

role/task/goal/objective操作要求、宗旨

COSTAR

context上下文背景/obective目标/style风格/tone语调/audience受众/response响应形式

## 提示词类型

指令型提示词：直接告诉AI需要执行的任务。 

问答型提示词：向AI提出问题，期望得到相应的答案。 

角色扮演型提示词：要求AI扮演特定角色，模拟特定场景。 

创意型提示词：引导AI进行创意写作或内容生成。 

分析型提示词：要求AI对给定信息进行分析和推理。

多模态提示词：结合文本、图像等多种形式的输入。

## 基于智能体开发平台的Prompt实操

智能体开发平台：AI技术普惠化的工具，帮助开发者/企业构建智能化服务。如coze/dify

### 课题 

用户对话，从对话中获取用户信息，确认用户权限，回答用户问题。

### 开发思路

分析核心诉求：1.从对话中获取用户信息；2.获取到信息后进行知识问答。本质是基于RAG解决方法的知识问答。

分析实施路径：1.设计RAG；2.让LLM提取用户信息。用到文本内容提取功能；3.设计逻辑。通过判断将上述步骤串联起来。

### 实操步骤

1. 创建知识库。选取索引模型/文本理解模型，导入知识，平台根据模型对知识进行拆分/向量化。
2. 创建流程编排。标准RAG流程，根据问题检索知识库，生成prompt交给LLM回答。
3. 加入定制化流程。LLM采集用户信息。
4. 加入定制化流程。用户信息不完善，LLM继续追问。
5. 加入定制化流程。采集完毕，更新标志位，进入问答。
6. 进入问答，判断标志位，标志位不在进入步骤3，存在进入步骤2。

# 多模态大模型

## 基本概念

大模型：

广义——参数量大、结构复杂的深度学习模型。也叫基础模型（FM, Foundation Model），基于广泛的训练数据，通常自监督学习，通过提示/微调广泛适应下游任务。

狭义——参数规模在一百亿10B以上，使用大规模的训练数据，具有良好的涌现能力，并在各种任务上达到较高性能水平的模型。

对比传统模型：具有涌现能力（规模提升性能突增）、参数规模庞大、多任务通用性。

## 大模型分类

| 分类方式     | 类别                                      | 适用任务                                                     |
| ------------ | ----------------------------------------- | ------------------------------------------------------------ |
| **输入类型** | NLP（自然语言处理）大模型                 | 文本生成、翻译、问答等语言任务，训练数据以文本为主，应用场景覆盖智能客服、内容创作、法律文书生成等典型架构包括：<br>1. 生成式模型：如GPT-4（文本生成）、PaLM（多语言处理）；<br>2. 理解式模型：如BERT（语义分析）、T5（文本分类） |
|              | CV（计算机视觉）大模型                    | 图像/视频处理任务，应用领域涵盖自动驾驶、安防监控、医学影像分析等。典型能力包括：<br>1. 图像分类与检测：如Vision Transformer（ViT）、YOLO系列；<br>2. 图像生成与编辑：如Stable Diffusion、DALL·E；<br>3. 视频理解：如Sora（视频生成）、SlowFast（动作识别） |
|              | 多模态大模型                              | 支持跨数据类型（文本、图像、音频等）的联合处理与生成，应用场景包括虚拟现实、智能机器人、跨媒体内容创作，关键技术包括：<br>1. 跨模态对齐：如CLIP（图文匹配）、Flamingo（多模态对话）；<br>2. 多模态生成：如Sora（视频生成）、DALL·E 3（文生图） |
| **应用领域** | L0 通用大模型                             | 跨领域多任务基座模型，通过海量通用数据预训练，具备基础认知与泛化能力（如文本生成、图像理解），代表技术如GPT、CLIP。类比“通识教育阶段” |
|              | L1 行业大模型                             | 聚焦特定领域（如医疗、金融），基于行业知识库和场景数据定向优化，实现专业化能力跃迁（如医疗问诊、工业质检）。类比“专业学科深造” |
|              | L2 场景大模型                             | 针对细分任务（如法律文书生成、生产线故障诊断），通过高精度场景数据微调，达到端到端任务最优解。类比“科研攻坚阶段” |
| **模型架构** | 密集模型（Dense Models）                  | 基于Transformer的完全参数激活模式，全连接参数结构，如GPT系列、LLaMA等 |
|              | 稀疏模型（Sparse Models）                 | 通过动态激活部分参数提升效率，通过路由机制选择特定子网络处理输入，实现参数规模与计算效率的平衡。如混合专家模型（MoE），如（DeepSeek、Kimi） |
| **训练范式** | 预训练+微调（pre-training + Fine-tuning） | 1. 在大规模通用数据集上进行初始训练<br>2. 在预训练模型基础上，使用领域数据调整参数以适配特定任务 |
|              | 提示学习（Prompt-based Learning）         | 通过自然语言指令（Prompt）驱动模型输出，无需显式参数更新，无需显式微调和更新模型参数 |
|              | 强化学习优化（RLHF）                      | 结合奖励函数或者人类反馈调整生成内容（如InstructGPT、DeepSeek） |
| **功能类型** | 通用模型                                  | 面向广泛任务设计的模型，通过大规模预训练学习通用语言规律和多模态理解能力，支持文本生成、问答、翻译等多样化需求（如DeepSeek-V3） |
|              | 推理模型                                  | 专精逻辑推导、数学计算与代码生成等复杂任务，通过神经符号融合、强化学习等技术增强深度推理能力，具备复杂逻辑推理能力（如DeepSeek-R1通过长思维链优化） |

## 多模态大模型典型架构

```mermaid
graph LR
	A[text文本]-->B[encoder编码器向量化]
	B-->|token|C[LLM处理]
	C-->|输出|D[text文本]
	E[image图片/audio音频/video视频]-->F[模态编码器提取特征]
	F --> G[连接器处理特征]
	G --> C
	C --> H[image图片/audio音频/video视频]
	
```
