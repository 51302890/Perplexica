# Perplexica 的架构

Perplexica 的架构由以下关键组件组成：

1. **用户界面**: 一个基于 Web 的界面，允许用户与 Perplexica 进行交互，用于搜索图片、视频等等。
2. **智能体/链**: 这些组件预测 Perplexica 的下一个动作，理解用户的查询，并判断是否需要进行网络搜索。
3. **SearXNG**: Perplexica 使用的一个元数据搜索引擎，用于在网络中搜索资源。
4. **LLM (大型语言模型)**: 由智能体和链使用，用于理解内容、编写响应和引用来源等任务。示例包括 Claude、GPT 等。
5. **嵌入模型**: 为了提高搜索结果的准确性，嵌入模型使用相似性搜索算法（如余弦相似度和点积距离）重新排序搜索结果。

有关这些组件如何协同工作的更详细说明，请参阅 [WORKING.md](https://github.com/ItzCrazyKns/Perplexica/tree/master/docs/architecture/WORKING.md)。
