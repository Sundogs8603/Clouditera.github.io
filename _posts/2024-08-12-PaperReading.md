---
layout:     post
title:      "【论文速读】| 涟漪下的漩涡：对启用RAG的应用程序的实证研究"
date:       2024-08-12 12:00:00
author:     "安全极客"
header-img: "img/post-bg-unix-linux.jpg"
tags:
    - Security
    - AIGC
    - 论文速读
---


![这是一张图片](https://www.gptsecurity.info/img/in-post/0807/01.jpg)


本次分享论文：Vortex under Ripplet: An Empirical Study of RAG-enabled Applications

## 基本信息

**原文作者**：Yuchen Shao, Yuheng Huang, Jiawei Shen, Lei Ma, Ting Su, Chengcheng Wan

**作者单位**：East China Normal University, The University of Tokyo, University of Alberta

**关键词**：RAG, LLM, Integration Defects, Software Development, Empirical Study

**原文链接**：https://arxiv.org/pdf/2407.05138

**开源代码**：暂无

## 论文要点

**论文简介**：本文研究了检索增强生成（RAG）技术支持的大语言模型（LLMs）在各种应用场景中的有效解决方案。然而，开发者在将RAG增强的LLMs集成到软件系统时面临许多挑战，包括接口规范的缺失、软件上下文的需求以及复杂的系统管理。通过对100个开源应用程序及其问题报告的手动研究，发现超过98%的应用程序存在多个集成缺陷，影响了软件功能、效率和安全性。

本文总结了19种缺陷模式，并提出了相应的解决方案指南，以帮助开发者更好地开发基于LLM的软件并激励未来的研究。

![这是一张图片](https://www.gptsecurity.info/img/in-post/0812/01.png)

**研究目的**：本研究旨在揭示RAG增强的LLM在实际应用中的系统集成问题，探讨开发者在集成过程中面临的主要挑战。通过对100个开源应用程序的实证分析，识别和总结常见的集成缺陷模式，提出系统性的解决方案和指导原则。研究的最终目标是帮助开发者更有效地构建和维护基于LLM的智能软件，提高软件的可靠性、效率和安全性，同时为未来的相关研究提供基础和方向。

**研究贡献**：本文首次深入研究了RAG增强的LLM在实际应用中的系统集成问题，揭示了在100个开源应用程序中广泛存在的集成缺陷。通过分析超过3000个问题报告，本文总结了19种常见的缺陷模式，并提出了系统性的解决方案。这些缺陷模式涵盖了功能、效率和安全等多个方面，导致了软件的意外停止、不正确行为、执行缓慢和安全漏洞。

本文的研究不仅为开发者提供了实用的指导，帮助他们识别和解决集成中的常见问题，还为未来研究提供了宝贵的参考和新的研究方向。通过这些贡献，本文旨在提高LLM增强软件的开发质量，促进更广泛和可靠的实际应用。

## 引言

大语言模型（LLMs）在各种语言处理任务中表现出色，通过检索增强生成（RAG）技术，这些模型在具体应用场景中的能力得到了进一步提升。RAG通过从外部数据源提供相关信息，使LLMs能够解决更为复杂和知识密集型的任务。云服务和各种框架，如LangChain和LlamaIndex，减轻了开发者实现和托管LLM和RAG解决方案的负担，推动了智能软件的迅速发展。

然而，尽管RAG技术大大提升了LLMs的应用潜力，开发者在集成这些技术时仍面临重大挑战，包括缺乏明确的接口规范、满足软件上下文需求的难度以及复杂的系统管理问题。此外，由于测试不充分和对LLM及RAG知识的缺乏，非专业开发者可能无法察觉这些集成问题。

本文通过对100个开源应用程序及其问题报告的实证研究，揭示了这些应用程序中广泛存在的集成缺陷，总结了19种缺陷模式，并提出了相应的解决方案，以帮助开发者更好地应对这些挑战，提高软件质量，并为未来研究提供参考。

## 研究背景

随着大语言模型（LLMs）在对话、文档理解和问答等认知功能中的广泛应用，检索增强生成（RAG）技术通过提供外部数据源的相关信息，进一步提升了LLMs在具体应用场景中的能力。通过云服务和各种框架，如LangChain和LlamaIndex，开发者可以更轻松地集成LLMs和向量数据库，开发出功能强大的智能软件。然而，这些应用在集成过程中仍面临诸多挑战，包括接口规范的缺失、软件上下文的需求和复杂的系统管理。

尽管已有大量研究致力于改进LLM和RAG算法，但关于其系统集成的研究却较为缺乏。本研究通过实证分析，揭示了RAG增强的LLMs在实际应用中的广泛集成缺陷，旨在为开发者提供实用指导，并为未来的研究提供参考。

![这是一张图片](https://www.gptsecurity.info/img/in-post/0812/02.png)

## 相关工作

先前的研究主要集中在改进LLM和RAG算法，但对LLM增强软件系统的集成问题关注较少。一些研究探索了通用AI组件的集成和LLM、RAG算法的提升，但这些研究通常侧重于算法本身，而非其在软件系统中的实际应用。此外，已有的研究多关注传统AI模型在特定任务中的使用，而LLMs作为通用语言模型，其在软件开发中的独特挑战尚未得到充分研究。

本文填补了这一空白，通过实证分析揭示了实际应用中的集成缺陷，提出了系统性的解决方案，为开发者提供了实用指导，并为未来研究提供了宝贵的参考和新方向。

## 研究方法

本文采用实证研究的方法，对 100 个涵盖 RAG 增强 LLM 的开源应用程序展开了分析，旨在揭示其系统集成方面的常见问题。

首先，研究者于 GitHub 上随机选取了 500 个开源项目，而后经过手动筛选，以保证每个项目均是针对具体的实际问题，并且实现了 LLM 与向量数据库的紧密集成。

其次，研究者对这些应用程序的 3000 多条问题报告予以了详细分析，确定了 320 个由软件缺陷所引发的问题。经由多轮的迭代，研究者对这些问题进行了总结和聚类，从而识别出 19 种常见的缺陷模式。

最后，研究者针对这些缺陷模式展开深入剖析，并提出了对应的解决方案与指导原则，其目的在于助力开发者更高效地集成和优化 LLM 增强的软件系统，提升其可靠性、效率以及安全性。

## 集成故障

通过实证研究，研究者在100个LLM增强的应用程序中识别出495个缺陷，归纳总结了19种常见的缺陷模式。这些缺陷主要由开发者不系统的提示/查询构建、对接口规范的误解、对软件上下文的忽视以及缺乏系统管理导致。

![这是一张图片](https://www.gptsecurity.info/img/in-post/0812/03.png)

它们广泛存在于四个主要组件中，对软件质量的各个方面产生了重大影响：

LLM代理：构建提示并生成LLM响应的组件。常见缺陷包括提示中缺乏上下文、缺乏限制、不当的历史管理、缺少输入格式验证、输出格式不兼容、输出过多、超出上下文限制和不当管理。

1. 向量数据库：支持RAG算法并增强LLM代理的组件。常见问题有知识错位、冲突的知识条目、不当的文本嵌入和不当的相似性搜索。
   
2. 软件组件：与LLM代理和向量数据库交互的其余软件部分。常见缺陷包括缺少最终输出、不当的错误处理、低频交互和隐私泄露。
   
3. 系统：管理资源和执行任务的组件。常见问题有资源争用、低效的内存管理以及LLM和下游任务之间的速度不匹配。
   
这些缺陷导致了功能、效率和安全等方面的问题，如意外停止、不正确行为、执行缓慢、不友好的用户界面、增加的令牌成本和安全漏洞。研究者提出了针对每种缺陷模式的系统性解决方案，帮助开发者提高LLM增强软件的质量。

## 论文结论

通过本文的实证研究，揭示了广泛存在于实际应用中的集成缺陷，并提出了相应的解决方案。研究者的研究表明，超过98%的应用程序存在多个类型的缺陷，导致了各种问题，包括意外的故障停止、不正确的软件行为、缓慢的执行、不友好的用户界面、增加的令牌成本和安全漏洞。这些缺陷分布在LLM增强软件的四个主要组件中：构建提示并生成LLM响应的LLM代理、支持RAG算法的向量数据库、与LLM代理和向量数据库交互的软件组件以及执行的系统。研究者希望本文的研究能够帮助LLM增强软件的开发，并激励未来的研究。


原作者：论文解读智能体

校对：小椰风






