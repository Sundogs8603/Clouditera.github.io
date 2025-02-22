---
layout:     post
title:      "第72期|GPTSecurity周报"
date:       2024-11-20 10:00:00
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

#### 从孤立指令到互动鼓励！通过自然语言提示实现大语言模型的安全代码生成

简介：本研究旨在介绍一个名为 SecCode 的创新框架，其致力于安全代码生成，运用了独特的互动鼓励提示（EP）技术，且仅借助自然语言（NL）提示来实现相关功能。这种方式的优势在于能够确保所使用的提示易于被一般用户分享和理解。SecCode 的运行主要通过三个阶段来完成。首先是利用自然语言提示进行代码生成；其次是运用研究者提出的鼓励提示对代码漏洞进行检测与修复；最后是进行漏洞交叉检查以及代码安全优化。值得注意的是，这些阶段并非一次性完成，而是通过多次互动迭代执行，从而逐步提升代码的安全性。

在实验评估环节，研究者采用了专有 LLMs（如 GPT - 3.5 Turbo、GPT - 4 和 GPT - 4o）以及开源 LLMs（如 Llama 3.1 8B Instruct、DeepSeek Coder V2 Lite Instruct），并在三个基准数据集上展开评估。广泛的实验结果有力地表明，研究者所提出的 SecCode 在生成安全代码方面展现出了显著优势，相较于对比基线表现更为出色，具备较高的漏洞修复率。例如，SecCode 在经过 5 次自动化 EP 互动迭代后，其修复成功率超过 76%，而经过 10 次自动化 EP 互动迭代后，这一比例更是超过了 89%。

据研究者所知，此项工作开创了仅通过自然语言提示构建安全代码生成的先河。目前，研究者已将相关代码开源，并积极鼓励社区对安全代码生成予以关注。

*链接：https://arxiv.org/abs/2410.14321*

#### 注意力是实现基于大语言模型的代码漏洞定位的关键

简介：本文着重介绍了 LOVA 这一全新框架，其致力于利用 LLMs 所固有的自注意力机制来强化漏洞定位功能。研究者的关键发现在于，自注意力机制能够对输入的不同部分赋予各异的重要性，这就使得追踪模型对特定代码行的关注程度变为可能。在漏洞定位的情境之下，存在这样一种假设，即脆弱的代码行自然而然会吸引更高的注意力权重，原因在于它们对模型输出具有更大的影响。通过系统地追踪注意力权重的变化情况，并聚焦于特定的代码行，LOVA 显著提高了在各类编程语言中识别脆弱行的精准度。经由严格的实验与评估过程，研究者有力地证明了 LOVA 在性能方面相较于现有的基于 LLM 的方法具有显著优势，其 F1 得分最高可提高 5.3 倍。LOVA 还展现出了出色的可扩展性，在 C、Python、Java 以及 Solidity 等语言的智能合约漏洞定位中，其准确性最高可提升 14.6 倍。它的鲁棒性通过在不同 LLM 架构下的稳定表现得到了有力证实。

*链接：https://arxiv.org/abs/2410.15288*

#### 迈向自动化渗透测试：引入大语言模型基准、分析与改进

简介：黑客攻击对网络安全形成了重大威胁，每年致使数十亿美元的损失。为降低此类风险，常采用道德黑客或渗透测试来识别系统与网络中的漏洞。近来，大语言模型（LLMs）的发展在包括网络安全在内的各个领域展现出潜力。然而，当前尚缺乏全面、开放且端到端的自动化渗透测试基准，以推动相关进展并评估这些模型在安全情境下的能力。本文介绍了一种新颖的开放基准，针对基于 LLM 的自动化渗透测试，填补了这一关键空缺。

研究者首先运用最先进的 PentestGPT 工具评估了包括 GPT - 4o 和 Llama 3.1 - 405B 在内的 LLM 的性能。研究发现，尽管 Llama 3.1 在某些方面优于 GPT - 4o，但这两种模型目前均未能实现完全自动化的端到端渗透测试。随后，研究者推动技术进步，提出消融研究，为改进 PentestGPT 工具提供了见解。研究者的研究揭示了 LLM 在渗透测试各个方面（如枚举、利用和特权提升）所面临的挑战。此项工作为 AI 辅助网络安全的知识体系做出了贡献，并且为未来基于大语言模型的自动化渗透测试研究奠定了基础。

*链接：https://arxiv.org/abs/2410.17141*

#### AdvWeb：对基于视觉语言模型（VLM）的网络智能体的可控黑箱攻击

简介：视觉语言模型（VLMs）已然彻底变革了通用网络智能体的创建，使其能够在真实网站上自主完成各类任务，进而提升人类的效率与生产力。然而，尽管这些智能体具备显著的能力，但其在恶意攻击下的安全性却严重被忽视，这引发了对其安全部署的重大忧虑。

为了揭示并利用网络智能体中的此类漏洞，研究者提供了 AdvWeb，这是一种专为网络智能体设计的新型黑箱攻击框架。AdvWeb 训练了一个对抗性提示生成模型，该模型能够生成对抗性提示并注入到网页中，从而误导网络智能体执行具有针对性的对抗性操作，例如不当的股票购买或错误的银行交易等，这些操作可能引发严重的现实后果。仅凭借对网络智能体的黑箱访问，研究者运用 DPO 训练并优化对抗性提示生成模型，利用针对目标智能体的成功与失败攻击字符串。与以往方法不同的是，研究者的对抗性字符串注入保持隐蔽且可控：其一，攻击前后网站的外观维持不变，用户几乎无法检测到篡改；其二，攻击者能够修改生成的对抗性字符串中的特定子字符串，轻松改变攻击目标（例如从不同公司购买股票），增强了攻击的灵活性与效率。

研究者进行了广泛的评估，展示了 AdvWeb 在针对基于 SOTA GPT - 4V 的 VLM 智能体执行各种网络任务时的高成功率。研究者的研究揭示了当前基于 LLM/VLM 的智能体存在的关键漏洞，强调了开发更可靠的网络智能体以及有效防御措施的紧迫需求。研究者的代码和数据可在相应链接获取。

*链接：https://arxiv.org/abs/2410.17401*

#### ProveRAG：基于来源驱动的漏洞分析与自动化检索增强的大语言模型

简介：在网络安全领域，安全分析师面临着实时缓解新发现漏洞的艰巨挑战，自 1999 年起，已识别出超过 30 万个常见漏洞和暴露（CVE）。已知漏洞的庞大数量致使检测未知威胁的模式变得错综复杂。尽管大语言模型（LLMs）能够提供一定助力，但其往往会产生幻觉，并且与近期的威胁缺乏一致性。截至 2024 年，已识别出超过 25000 个漏洞，这些漏洞是在流行的 LLM（如 GPT - 4）的训练数据截止后引入的。这给网络安全中利用 LLM 带来了一项主要挑战，因为准确性和最新信息至关重要。

在这项工作中，研究者旨在通过模仿分析师执行漏洞分析的方式，提升 LLMs 在漏洞分析中的适应性。研究者提出了 ProveRAG，这是一个基于 LLM 的系统，旨在通过自动化检索增强网络数据，快速分析 CVE，同时利用可验证的证据自我评估其响应。ProveRAG 包含自我批判机制，以助力缓解在网络安全应用中常见的输出遗漏和幻觉问题。该系统交叉引用可验证来源的数据（如 NVD 和 CWE），使分析师对所提供的可操作见解充满信心。

研究者的结果表明，ProveRAG 在提供可验证证据方面表现卓越，漏洞利用和缓解策略的准确率分别超过 99% 和 97%。该系统在漏洞分析中优于直接提示和分块检索，克服了时间和上下文窗口的限制。ProveRAG 帮助分析师更有效地保护系统，同时记录过程以供未来审计使用。

*链接：https://arxiv.org/abs/2410.17406*

#### SafeBench：多模态大语言模型的安全评估框架

简介：多模态大语言模型（MLLMs）在安全性方面引发了强烈关注（例如为用户生成有害输出），这促使了安全评估基准的开发。然而，研究者观察到现有的针对 MLLMs 的安全基准在查询质量和评估可靠性方面存在局限性，限制了对模型安全性影响的检测，毕竟 MLLMs 仍处于不断发展之中。在本文中，研究者提出了 \toolns，这是一个旨在进行 MLLMs 安全评估的综合框架。研究者的框架涵盖了一个全面的有害查询数据集以及一个自动化评估协议，旨在分别解决上述局限性。

研究者首先设计了一个自动化的安全数据集生成管道，在此过程中，研究者运用一组 LLM 评审员来识别并分类对 MLLMs 最具危害性且多样化的风险场景；基于分类法，研究者进一步要求这些评审员相应地生成高质量的有害查询，由此形成了 23 个风险场景以及 2300 对多模态有害查询。在安全评估过程中，研究者受司法程序中陪审团制度的启发，开创了陪审团审议评估协议，采用协作 LLMs 来评估目标模型是否呈现出特定的有害行为，从而提供可靠且无偏见的内容安全风险评估。此外，研究者的基准还能够扩展到音频模态，展现出了很高的可扩展性和潜力。

基于研究者的框架，研究者对 15 个广泛使用的开源 MLLMs 以及 6 个商业 MLLMs（如 GPT - 4o、Gemini）进行了大规模实验，揭示了现有 MLLMs 中普遍存在的安全问题，并举例说明了关于 MLLM 安全性能的若干见解，例如图像质量和参数大小等方面。

*链接：https://arxiv.org/abs/2410.18927*


![secgeek-foot](https://www.gptsecurity.info/img/secgeek-foot.png)
