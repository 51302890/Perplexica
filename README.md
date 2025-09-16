# 🚀 Perplexica - 一款AI驱动的搜索引擎 🔎 <!-- omit in toc -->

<div align="center" markdown="1">
   <sup>特别感谢：</sup>
   <br>
   <br>
   <a href="https://www.warp.dev/perplexica">
      <img alt="Warp赞助" width="400" src="https://github.com/user-attachments/assets/775dd593-9b5f-40f1-bf48-479faff4c27b">
   </a>

### [Warp, 您终端中的AI开发工具](https://www.warp.dev/perplexica)

[适用于MacOS、Linux和Windows](https://www.warp.dev/perplexica)

</div>

<hr/>

[![Discord](https://dcbadge.limes.pink/api/server/26aArMy8tT?style=flat)](https://discord.gg/26aArMy8tT)

![预览](.assets/perplexica-screenshot.png?)

## 目录 <!-- omit in toc -->

- [概述](#overview)
- [预览](#preview)
- [功能特性](#features)
- [安装指南](#installation)
  - [使用Docker开始（推荐）](#getting-started-with-docker-recommended)
  - [非Docker安装](#non-docker-installation)
  - [Ollama连接错误](#ollama-connection-errors)
- [作为搜索引擎使用](#using-as-a-search-engine)
- [使用Perplexica的API](#using-perplexicas-api)
- [将Perplexica暴露到网络](#expose-perplexica-to-network)
- [一键部署](#one-click-deployment)
- [即将推出的功能](#upcoming-features)
- [支持我们](#support-us)
  - [捐赠](#donations)
- [贡献](#contribution)
- [帮助与支持](#help-and-support)

## 概述

Perplexica 是一款开源的AI驱动搜索工具或AI搜索引擎，它能深入互联网寻找答案。灵感来源于Perplexity AI，它是一个开源选项，不仅搜索网页，还能理解您的问题。它使用相似性搜索和嵌入等先进的机器学习算法来优化结果，并提供带有引用来源的清晰答案。

使用SearxNG保持最新且完全开源，Perplexica确保您始终获得最新信息，而不会损害您的隐私。

想了解更多关于其架构和工作原理的信息？您可以在[这里](https://github.com/ItzCrazyKns/Perplexica/tree/master/docs/architecture/README.md)阅读。

## 预览

![视频预览](.assets/perplexica-preview.gif)

## 功能特性

- **本地大语言模型**：您可以使用Qwen、DeepSeek、Llama和Mistral等本地LLM。
- **两种主要模式**：
  - **Copilot模式：**（开发中）通过生成不同的查询来增强搜索，以找到更相关的互联网资源。与正常搜索类似，它不仅仅是使用SearxNG的上下文，而是访问顶级匹配项并尝试直接从页面找到与用户查询相关的资源。
  - **普通模式：**处理您的查询并执行网络搜索。
- **专注模式**：专门用于更好地回答特定类型问题的模式。Perplexica目前有6种专注模式：
  - **全模式：**搜索整个网络以找到最佳结果。
  - **写作助手模式：**对不需要搜索网络的写作任务有帮助。
  - **学术搜索模式：**查找文章和论文，适合学术研究。
  - **YouTube搜索模式：**根据搜索查询查找YouTube视频。
  - **Wolfram Alpha搜索模式：**使用Wolfram Alpha回答需要计算或数据分析的查询。
  - **Reddit搜索模式：**搜索Reddit上与查询相关的讨论和意见。
- **最新信息：**一些搜索工具可能会提供过时的信息，因为它们使用爬虫机器人抓取的数据，并将其转换为嵌入并存储在索引中。与它们不同，Perplexica使用SearxNG（元搜索引擎）获取结果，重新排序并从中获取最相关的资源，确保您始终获得最新信息，而无需每日数据更新的开销。
- **API：**将Perplexica集成到您现有的应用程序中并利用其功能。

它还有许多其他功能，如图像和视频搜索。一些计划的功能在[即将推出的功能](#upcoming-features)中提及。

## 安装指南

主要有两种安装Perplexica的方式 - 使用Docker和不使用Docker。强烈推荐使用Docker。

### 使用Docker开始（推荐）

1. 确保您的系统上安装并运行了Docker。
2. 克隆Perplexica仓库：

   ```bash
   git clone https://github.com/ItzCrazyKns/Perplexica.git
   ```

3. 克隆后，导航到包含项目文件的目录。

4. 将`sample.config.toml`文件重命名为`config.toml`。对于Docker设置，您只需要填写以下字段：

   - `OPENAI`：您的OpenAI API密钥。**只有当您希望使用OpenAI的模型时才需要填写此字段**。
   - `CUSTOM_OPENAI`：您的OpenAI API兼容的本地服务器URL、模型名称和API密钥。您应该将本地服务器主机设置为`0.0.0.0`，记下它运行的端口号，然后使用该端口号设置`API_URL = http://host.docker.internal:PORT_NUMBER`。您必须指定模型名称，如`MODEL_NAME = "unsloth/DeepSeek-R1-0528-Qwen3-8B-GGUF:Q4_K_XL"`。最后，将`API_KEY`设置为适当的值。如果您没有定义API密钥，只需在引号之间放置任何您想要的内容：`API_KEY = "whatever-you-want-but-not-blank"` **只有当您想使用本地OpenAI兼容服务器（如Llama.cpp的[`llama-server`](https://github.com/ggml-org/llama.cpp/blob/master/tools/server/README.md)）时才需要配置这些设置**。
   - `OLLAMA`：您的Ollama API URL。您应该输入为`http://host.docker.internal:PORT_NUMBER`。如果您在端口11434上安装了Ollama，请使用`http://host.docker.internal:11434`。对于其他端口，相应调整。**只有当您希望使用Ollama的模型而不是OpenAI的模型时才需要填写此字段**。
   - `GROQ`：您的Groq API密钥。**只有当您希望使用Groq的托管模型时才需要填写此字段**。
   - `ANTHROPIC`：您的Anthropic API密钥。**只有当您希望使用Anthropic模型时才需要填写此字段**。
   - `Gemini`：您的Gemini API密钥。**只有当您希望使用Google的模型时才需要填写此字段**。
   - `DEEPSEEK`：您的Deepseek API密钥。**只有当您想要Deepseek模型时才需要**。
   - `AIMLAPI`：您的AI/ML API密钥。**只有当您想要使用AI/ML API模型和嵌入时才需要**。

      **注意**：您可以从设置对话框启动Perplexica后更改这些设置。

   - `SIMILARITY_MEASURE`：要使用的相似性度量（默认已填写；如果您不确定，可以保持不变）。

5. 确保您位于包含`docker-compose.yaml`文件的目录中，然后执行：

   ```bash
   docker compose up -d
   ```

6. 等待几分钟设置完成。您可以在网络浏览器中通过 http://localhost:3000 访问Perplexica。

**注意**：构建容器后，您可以直接从Docker启动Perplexica，无需打开终端。

### 非Docker安装

1. 安装SearXNG并在SearXNG设置中允许`JSON`格式。
2. 克隆仓库并将`sample.config.toml`文件重命名为根目录中的`config.toml`。确保在此文件中完成所有必需字段。
3. 填充配置后运行`npm i`。
4. 安装依赖项，然后执行`npm run build`。
5. 最后，通过运行`npm run start`启动应用程序。

**注意**：推荐使用Docker，因为它可以简化设置过程，特别是在管理环境变量和依赖项方面。

有关更新等更多信息，请参阅[安装文档](https://github.com/ItzCrazyKns/Perplexica/tree/master/docs/installation)。

### 故障排除

#### 本地OpenAI API兼容服务器

如果Perplexica告诉您您没有配置任何聊天模型提供商，请确保：

1. 您的服务器运行在`0.0.0.0`（而不是`127.0.0.1`）上，并且与您在API URL中输入的端口相同。
2. 您已指定本地LLM服务器加载的正确模型名称。
3. 您已指定正确的API密钥，或者如果未定义，您在API密钥字段中放置了*某些内容*，而不是留空。

#### Ollama连接错误

如果您遇到Ollama连接错误，可能是由于后端无法连接到Ollama的API所致。要解决此问题，您可以：

1. **检查您的Ollama API URL：**确保API URL在设置菜单中正确设置。
2. **根据操作系统更新API URL：**

   - **Windows：**使用`http://host.docker.internal:11434`
   - **Mac：**使用`http://host.docker.internal:11434`
   - **Linux：**使用`http://<private_ip_of_host>:11434`

   如果您使用不同的端口，请相应调整。

3. **Linux用户 - 将Ollama暴露到网络：**

   - 在`/etc/systemd/system/ollama.service`中，您需要添加`Environment="OLLAMA_HOST=0.0.0.0:11434"`。（如果您使用不同的端口，请更改端口号。）然后使用`systemctl daemon-reload`重新加载systemd管理器配置，并通过`systemctl restart ollama`重启Ollama。有关更多信息，请参见[Ollama文档](https://github.com/ollama/ollama/blob/main/docs/faq.md#setting-environment-variables-on-linux)

   - 确保端口（默认为11434）没有被防火墙阻止。

## 作为搜索引擎使用

如果您希望将Perplexica用作Google或Bing等传统搜索引擎的替代品，或者想要从浏览器搜索栏添加快速访问的快捷方式，请按照以下步骤操作：

1. 打开浏览器设置。
2. 导航到"搜索引擎"部分。
3. 使用以下URL添加新站点搜索：`http://localhost:3000/?q=%s`。如果Perplexica不是本地托管，请将`localhost`替换为您的IP地址或域名，并将`3000`替换为端口号。
4. 单击添加按钮。现在您可以直接从浏览器搜索栏使用Perplexica。

## 使用Perplexica的API

Perplexica还为寻求将其强大的搜索引擎集成到自己的应用程序中的开发人员提供了API。您可以运行搜索、使用多种模型并获取查询的答案。

有关更多详细信息，请在此处查看完整文档[here](https://github.com/ItzCrazyKns/Perplexica/tree/master/docs/API/SEARCH.md)。

## 将Perplexica暴露到网络

Perplexica在Next.js上运行并处理所有API请求。它立即在同一个网络上工作，即使在端口转发后也保持可访问性。

## 一键部署

[![部署到Sealos](https://raw.githubusercontent.com/labring-actions/templates/main/Deploy-on-Sealos.svg)](https://usw.sealos.io/?openapp=system-template%3FtemplateName%3Dperplexica)
[![部署到RepoCloud](https://d16t0pc4846x52.cloudfront.net/deploylobe.svg)](https://repocloud.io/details/?app_id=267)
[![在ClawCloud上运行](https://raw.githubusercontent.com/ClawCloud/Run-Template/refs/heads/main/Run-on-ClawCloud.svg)](https://template.run.claw.cloud/?referralCode=U11MRQ8U9RM4&openapp=system-fastdeploy%3FtemplateName%3Dperplexica)

## 即将推出的功能

- [x] 添加设置页面
- [x] 添加对本地LLM的支持
- [x] 历史记录保存功能
- [x] 引入各种专注模式
- [x] 添加API支持
- [x] 添加发现功能
- [ ] 完成Copilot模式

## 支持我们

如果您发现Perplexica有用，请考虑在GitHub上给我们一个星标。这有助于更多人发现Perplexica并支持新功能的开发。我们非常感谢您的支持！

### 捐赠

我们也接受捐赠以帮助维持我们的项目。如果您想做出贡献，可以使用以下选项进行捐赠。感谢您的支持！

| 以太坊                                              |
| --------------------------------------------------- |
| 地址：`0xB025a84b2F269570Eb8D4b05DEdaA41D8525B6DD` |

## 贡献

Perplexica基于AI和大语言模型应该每个人都能轻松使用的理念构建。如果您发现错误或有想法，请通过GitHub Issues分享。有关为Perplexica做出贡献的更多信息，您可以阅读[CONTRIBUTING.md](CONTRIBUTING.md)文件，以了解更多关于Perplexica以及如何为其做出贡献的信息。

## 帮助与支持

如果您有任何问题或反馈，请随时联系我们。您可以在GitHub上创建问题或加入我们的Discord服务器。在那里，您可以与其他用户联系，分享您的经验和评价，并获得更个性化的帮助。[点击这里](https://discord.gg/EFwsmQDgAu)加入Discord服务器。要讨论常规支持之外的问题，请随时在Discord上联系我，用户名为`itzcrazykns`。

感谢您探索Perplexica，这款旨在增强您搜索体验的AI搜索引擎。我们正在不断努力改进Perplexica并扩展其功能。我们重视您的反馈和贡献，这些帮助我们使Perplexica变得更好。别忘了回来查看更新和新功能！
