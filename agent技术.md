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

![image-20260228100113514](http://image.huawei.com/tiny-lts/v1/images/hi3ms/3755dbb5e716963b1646ce6d7d82f8b5_1043x562.png)

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

![image-20260228110020921](http://image.huawei.com/tiny-lts/v1/images/hi3ms/8d7431b3097be9fc7de757133439bb6b_1535x819.png)

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

![image-20260228155534200](http://image.huawei.com/tiny-lts/v1/images/hi3ms/3f3c5130a26db3f3437e4a89be673bad_1500x758.png)

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


