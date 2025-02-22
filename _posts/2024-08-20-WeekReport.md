---
layout:     post
title:      "第64期|GPTSecurity周报"
date:       2024-08-20 16:00:00
author:     "安全极客"
header-img: "img/post-bg-unix-linux.jpg"
catalog: true
tags:
    - Security
    - AIGC
    - GPTSecurity周报
---


![这是一张图片](https://www.gptsecurity.info/img/in-post/0807/01.jpg)

GPTSecurity是一个涵盖了前沿学术研究和实践经验分享的社区，集成了生成预训练Transformer（GPT）、人工智能生成内容（AIGC）以及大语言模型（LLM）等安全领域应用的知识。在这里，您可以找到关于GPT/AIGC/LLM最新的研究论文、博客文章、实用的工具和预设指令（Prompts）。现为了更好地知悉近一周的贡献内容，现总结如下。


## Security Papers



#### CodeMirage: 由大语言模型生成的代码幻觉

简介：大语言模型（LLMs）在程序生成和自动化编码领域展现出巨大潜力，但同时也存在生成错误代码的风险。这些错误可能包括语法、逻辑错误，甚至安全漏洞和内存泄漏。鉴于LLMs在提升编码效率方面的广泛应用，深入研究其在代码生成中的错误显得尤为重要。本研究首次系统性地探讨了LLMs生成的代码幻觉问题，定义了代码幻觉，并分类了其类型。研究者创建了首个基准数据集CodeMirage，包含1137个由GPT-3.5生成的Python代码片段。通过对比开源模型CodeLLaMA、GPT-3.5和GPT-4的检测方法，发现GPT-4在HumanEval数据集上表现优异，与CodeBERT在MBPP数据集上的结果相当。最后，研究者讨论了减少代码幻觉的策略，为未来研究提供了方向。

*链接：https://arxiv.org/abs/2408.08333*

#### Transformers 和大语言模型在高效入侵检测系统中的应用：一项全面调查

简介：随着 Transformers 和大语言模型（LLMs）在自然语言处理（NLP）领域迅速发展，它们在网络安全领域的应用不断增加。网络安全中的众多关键参数以文本和表格形式呈现，使得 NLP 技术成为强化通信安全的重要工具。本综述论文深入剖析了 Transformers 和 LLMs 在网络威胁检测系统中的应用，构建了评估现有研究的严格框架。论文论述了 Transformers 的基础知识，涵盖网络攻击背景及常用数据集。着重分析了基于注意力的模型、BERT、GPT 等 LLMs，还有 CNN/LSTM-Transformer 混合体、ViTs 等不同架构在入侵检测系统中的运用。同时，探讨了这些技术在计算机网络、物联网、关键基础设施、云计算、SDN 和自动驾驶车辆等领域的实施状况。文章还指明了研究面临的挑战，例如可解释性、可扩展性和适应性，并提出了未来的研究方向，强调了 Transformers 和 LLMs 在提升网络威胁检测能力方面的关键作用。

*链接：https://arxiv.org/abs/2408.07583*

#### 评估基于大语言模型的个人信息提取及其对策

简介：研究者进行了一项有关基于大语言模型（LLM）的个人信息提取及对策的系统测量研究。传统方法在从公开个人资料中提取个人信息（如姓名、电话、邮箱等）方面成效有限。研究者为此提出基于 LLM 的提取攻击框架，收集了三个数据集（含 GPT-4 生成的合成数据集和两个真实世界数据集），引入基于“提示注入”的新缓解策略，并使用 10 个 LLM 和 3 个数据集进行基准测试。关键发现有：攻击者可能滥用 LLM 准确提取个人信息，LLM 在此方面优于传统方法，提示注入能很大程度减轻风险且优于传统对策。

链接：https://arxiv.org/abs/2408.07291

#### 使用高级大语言模型增强较小大语言模型：一种可解释的知识蒸馏方法

简介：研究者指出，像 GPT-4 或 LlaMa 3 这类先进大语言模型在复杂的类人交互中性能优越。但它们成本高、规模大，不适合边缘设备且自行托管难度大，存在安全与隐私问题。为此，研究者引入一种新颖的可解释知识蒸馏方法，以提升公司可自行托管的更小型、更经济的语言模型的性能。他们在构建以目标导向对话实现高客户满意度的客户服务代理情境中进行研究。与传统知识蒸馏不同，此可解释的“策略”教学法让教师提供策略以改善学生在各种场景中的表现，方法在“场景生成”和“改进策略”步骤间交替，仅需黑箱访问模型，无需操作参数。在客户服务应用中，该方法提升了性能，所学策略可转移，其可解释性还能通过人工审核防范潜在危害。

*链接：https://arxiv.org/abs/2408.07238*

#### 用于安全代码评估的大语言模型：一项多语言实证研究
简介：研究者指出，多数漏洞检测研究聚焦于 C/C++ 代码的漏洞数据集，语言多样性受限，包括大语言模型在内的深度学习方法在检测其他语言软件漏洞的有效性仍待探索。为此，研究者使用不同提示和角色策略，针对六种先进预训练的 LLM（如 GPT-3.5-Turbo 等）及五种编程语言（Python、C、C++、Java、JavaScript），评估其在检测和分类常见弱点枚举方面的效果。他们从不同来源编译多语言漏洞数据集以确保代表性，结果显示 GPT-4o 在少样本设置下，漏洞检测和分类得分最高。此外，研究者还开发了与 VSCode 集成的 CODEGUARDIAN 库，通过涉及 22 名行业开发人员的用户研究评估发现，使用该库能让开发人员更准确快速地检测漏洞。

*链接：https://arxiv.org/abs/2408.06428*

#### 基于RAG的网络攻击调查和归因问题的问答解决方案

简介：研究者在这项工作中，首次引入了基于检索增强生成（RAG）技术和大语言模型（LLM）的问答（QA）模型，旨在为网络安全专家提供有关网络攻击调查和归因的信息。该 QA 模型依据包含网络攻击调查和归因精选信息的知识库（KB），或者用户提供的外部资源来提供答案。研究者用各类问题对 QA 模型进行了测试和评估，包括基于 KB 的、基于元数据的、来自 KB 的具体文档以及基于外部资源的问题。他们还将基于 KB 问题的答案与 OpenAI 的 GPT-3.5 和最新的 GPT-4o LLMs 的答案作比较。研究者提出的 QA 模型因能提供答案来源并克服 GPT 模型的幻觉限制，优于 OpenAI 的 GPT 模型，这在网络攻击的调查和归因中至关重要。此外，分析表明，RAG QA 模型给出少量样本示例时生成的答案比零样本指令更好。

*链接：https://arxiv.org/abs/2408.06272*




![secgeek-foot](https://www.gptsecurity.info/img/secgeek-foot.png)
