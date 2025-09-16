# Perplexica 搜索 API 文档

## 概述

Perplexica 的搜索 API 使您能够轻松使用我们的 AI 驱动的搜索引擎。您可以运行不同类型的搜索，选择您想使用的模型，并获取最新信息。请按照以下标题了解更多关于 Perplexica 搜索 API 的信息。

## 端点

### **POST** `http://localhost:3000/api/search`

**注意**：如果您更改了默认端口，请将 `3000` 替换为任何其他端口

### 请求

API 接受请求体中的 JSON 对象，您可以在其中定义焦点模式、聊天模型、嵌入模型和您的查询。

#### 请求体结构

```json
{
  "chatModel": {
    "provider": "openai",
    "name": "gpt-4o-mini"
  },
  "embeddingModel": {
    "provider": "openai",
    "name": "text-embedding-3-large"
  },
  "optimizationMode": "speed",
  "focusMode": "webSearch",
  "query": "What is Perplexica",
  "history": [
    ["human", "Hi, how are you?"],
    ["assistant", "I am doing well, how can I help you today?"]
  ],
  "systemInstructions": "Focus on providing technical details about Perplexica's architecture.",
  "stream": false
}
```

### 请求参数

- **`chatModel`** (object, optional): 定义用于查询的聊天模型。有关模型详细信息，您可以向 `http://localhost:3000/api/models` 发送 GET 请求。请确保使用键值（例如使用 "gpt-4o-mini" 而不是显示名称 "GPT 4 omni mini"）。

  - `provider`: 指定聊天模型的提供商（例如 `openai`、`ollama`）。
  - `name`: 来自所选提供商的特定模型（例如 `gpt-4o-mini`）。
  - 自定义 OpenAI 配置的可选字段：
    - `customOpenAIBaseURL`: 如果您使用自定义 OpenAI 实例，请提供基础 URL。
    - `customOpenAIKey`: 自定义 OpenAI 实例的 API 密钥。

- **`embeddingModel`** (object, optional): 定义用于基于相似性搜索的嵌入模型。有关模型详细信息，您可以向 `http://localhost:3000/api/models` 发送 GET 请求。请确保使用键值（例如使用 "text-embedding-3-large" 而不是显示名称 "Text Embedding 3 Large"）。

  - `provider`: 嵌入模型的提供商（例如 `openai`）。
  - `name`: 特定的嵌入模型（例如 `text-embedding-3-large`）。

- **`focusMode`** (string, required): 指定要使用的焦点模式。可用模式：

  - `webSearch`, `academicSearch`, `writingAssistant`, `wolframAlphaSearch`, `youtubeSearch`, `redditSearch`.

- **`optimizationMode`** (string, optional): 指定优化模式以控制性能和质量的平衡。可用模式：

  - `speed`: 优先考虑速度并返回最快答案。
  - `balanced`: 提供速度和质量平衡良好的答案。

- **`query`** (string, required): 搜索查询或问题。

- **`systemInstructions`** (string, optional): 用户提供的自定义指令，用于引导 AI 的响应。这些指令被视为用户偏好，优先级低于系统的核心指令。例如，您可以指定特定的写作风格、格式或重点领域。

- **`history`** (array, optional): 表示对话历史的消息对数组。每对包含一个角色（'human' 或 'assistant'）和消息内容。这使系统能够使用对话的上下文来优化结果。示例：

  ```json
  [
    ["human", "What is Perplexica?"],
    ["assistant", "Perplexica is an AI-powered search engine..."]
  ]
  ```

- **`stream`** (boolean, optional): 设置为 `true` 时启用流式响应。默认为 `false`。

### 响应

API 的响应包括最终消息和用于生成该消息的来源。

#### 标准响应（stream: false）

```json
{
  "message": "Perplexica is an innovative, open-source AI-powered search engine designed to enhance the way users search for information online. Here are some key features and characteristics of Perplexica:\n\n- **AI-Powered Technology**: It utilizes advanced machine learning algorithms to not only retrieve information but also to understand the context and intent behind user queries, providing more relevant results [1][5].\n\n- **Open-Source**: Being open-source, Perplexica offers flexibility and transparency, allowing users to explore its functionalities without the constraints of proprietary software [3][10].",
  "sources": [
    {
      "pageContent": "Perplexica is an innovative, open-source AI-powered search engine designed to enhance the way users search for information online.",
      "metadata": {
        "title": "What is Perplexica, and how does it function as an AI-powered search ...",
        "url": "https://askai.glarity.app/search/What-is-Perplexica--and-how-does-it-function-as-an-AI-powered-search-engine"
      }
    },
    {
      "pageContent": "Perplexica is an open-source AI-powered search tool that dives deep into the internet to find precise answers.",
      "metadata": {
        "title": "Sahar Mor's Post",
        "url": "https://www.linkedin.com/posts/sahar-mor_a-new-open-source-project-called-perplexica-activity-7204489745668694016-ncja"
      }
    }
        ....
  ]
}
```

#### 流式响应（stream: true）

启用流式传输时，API 返回换行符分隔的 JSON 对象流。每行包含一个完整、有效的 JSON 对象。响应的 Content-Type 为 application/json。

流式响应对象的示例：

```
{"type":"init","data":"Stream connected"}
{"type":"sources","data":[{"pageContent":"...","metadata":{"title":"...","url":"..."}},...]}
{"type":"response","data":"Perplexica is an "}
{"type":"response","data":"innovative, open-source "}
{"type":"response","data":"AI-powered search engine..."}
{"type":"done"}
```

客户端应将每行作为单独的 JSON 对象处理。不同的消息类型包括：

- **`init`**: 初始连接消息
- **`sources`**: 用于响应的所有来源
- **`response`**: 生成的答案文本块
- **`done`**: 表示流式传输完成

### 响应中的字段

- **`message`** (string): 基于查询和焦点模式生成的搜索结果。
- **`sources`** (array): 用于生成搜索结果的来源列表。每个来源包括：
  - `pageContent`: 来自来源的相关内容片段。
  - `metadata`: 关于来源的元数据，包括：
    - `title`: 网页标题。
    - `url`: 网页 URL。

### 错误处理

如果在搜索过程中发生错误，API 将返回适当的错误消息和 HTTP 状态码。

- **400**: 如果请求格式错误或缺少必需字段（例如没有焦点模式或查询）。
- **500**: 如果在搜索过程中发生内部服务器错误。
