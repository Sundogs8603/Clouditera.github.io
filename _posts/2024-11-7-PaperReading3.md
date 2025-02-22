---
layout:     post
title:      "【论文速读】| APILOT：通过避开过时API陷阱，引导大语言模型生成安全代码"
date:       2024-11-7 11:30:00
author:     "安全极客"
header-img: "img/post-bg-unix-linux.jpg"
tags:
    - Security
    - AIGC
    - 论文速读
---


![这是一张图片](https://www.gptsecurity.info/img/in-post/0807/01.jpg)

## 基本信息

**原文标题**：APILOT: Navigating Large Language Models to Generate Secure Code by Sidestepping Outdated API Pitfalls

**原文作者**：Weiheng Bai, Keyang Xuan, Pengxiang Huang, Qiushi Wu, Jianing Wen, Jingjing Wu, Kangjie Lu

**作者单位**：University of Minnesota - Twin Cities, University of Illinois Urbana-Champaign, Northwestern University, IBM Research

**关键词**：大语言模型，安全代码生成，API推荐，过时API，代码安全

**原文链接**：https://arxiv.org/pdf/2409.16526

**开源代码**：暂无

## 论文要点

**论文简介**：这篇论文关注于随着大语言模型（LLMs）在代码生成中的广泛应用所带来的安全风险，尤其是由于LLMs的训练数据集过时导致推荐过时或有漏洞的API问题。论文提出了一种名为APILOT的系统，它通过实时更新过时API的数据集，并结合增强生成方法，引导LLMs生成版本感知的安全代码。在7个不同的LLMs上进行的实验表明，APILOT能够显著减少过时API推荐，平均减少率达到89.42%，同时提升了代码的可用性，平均提升了27.54%。

**研究目的**：随着大语言模型在代码生成领域的快速发展，代码助手（如GitHub Copilot和Codex）已成为开发者日常工作的重要工具。然而，这些AI驱动的工具有时会生成包含安全漏洞的代码，尤其是推荐使用过时的API，这些API往往在较新的版本中被修复或弃用。由于重新训练大模型的高昂成本和时间需求，模型无法及时更新其知识，导致其生成的代码无法反映最新的安全标准。本研究的目的是解决这一问题，开发一个能够实时更新API信息，并引导大模型生成安全代码的系统。

**研究贡献**：

在这篇论文中，作者提出了以下几个关键贡献：

1. 进行了一项全面的研究，分析了大语言模型推荐过时API的普遍性及其安全影
   
2.  提出了APILOT系统，通过GitHub提交差异分析和抽象语法树（AST）分析等技术，增强了大语言模型生成代码的安全性。
   
3. 引入了一种新的衡量代码安全性的指标和数据集，用于评估由LLMs生成的代码的安全性。

## 引言

随着大语言模型（LLMs）的迅速发展，AI驱动的代码生成工具，如GitHub Copilot和Codex，已经成为软件开发的重要组成部分。然而，当前这些工具面临一个严重的问题，即生成代码时可能会调用过时的API，而这些API可能在新的版本中存在安全漏洞。由于LLMs的训练数据集通常不会频繁更新，模型往往无法识别最新的API更改，从而导致推荐过时或不安全的API。

例如，一些API在旧版本中存在安全漏洞，但通过补丁在新版本中已修复。如果LLMs没有及时更新其知识库，可能仍会推荐这些旧版本的API，进而导致生成不安全的代码。此外，某些API的使用方式在新版本中发生了变化，但旧版本的用法仍可能被模型推荐。这种版本不敏感的API推荐问题在当前LLMs中普遍存在。

现有的方法，如提示工程（prompt engineering）和模型微调，虽然可以在一定程度上改善代码逻辑的安全性，但无法有效解决API版本更新的问题。提示工程更多关注于代码的逻辑结构，而微调模型则需要大量的时间和资源，导致模型在实际使用中依然基于过时的数据集进行推理。

为了解决这一问题，作者提出了APILOT，一个能够帮助LLMs生成版本感知、安全代码的系统。APILOT的核心思想是构建一个实时可更新的系统，持续收集和利用最新的API版本信息和漏洞数据，以引导LLMs避免推荐不安全的API。

## 研究背景

当前AI驱动的代码生成工具在生成代码时，面临着代码安全性不足的问题。特别是大语言模型（LLMs）推荐的API可能是过时的API，这些API往往在较新的版本中被修复或替换。随着软件开发依赖于第三方库和API的日益增加，推荐使用这些过时API会导致严重的安全风险。此外，由于LLMs的训练数据集通常并不包含最新的API信息，导致模型推荐的API版本滞后于实际开发中的最新版本。

API的过时问题主要可以分为三类：弃用API（deprecated APIs）、已修补的API（patched APIs）和使用方式修改的API（usage-modified APIs）。弃用API通常会在即将发布的新版本中被移除，而已修补的API则在新版本中修复了安全漏洞。使用方式修改的API则意味着其参数或返回值发生了变化，这些变化通常是为了修复某些安全问题。如果LLMs无法识别这些变化，可能会推荐不安全或不再适用的API，导致代码生成的安全性大幅降低。

## API研究

软件开发中，第三方API的使用极其普遍，尤其是在通过AI生成代码时，API推荐的准确性直接影响代码的安全性和性能。然而，大语言模型（LLMs）由于使用静态、过时的数据集，常常会推荐使用存在安全风险或已经不再适用的API。这些过时API包括三种主要类型：

**1. 弃用API（Deprecated APIs）**：这些API已被标记为不再推荐使用，开发者应尽快迁移到替代API。这类API虽然在当前版本仍可使用，但将会在未来版本中完全移除，开发者如果继续依赖这些API，可能会遭遇功能失效或安全漏洞。
   
![这是一张图片](https://www.gptsecurity.info/img/in-post/1107/06.png)

**2. 已修补API（Patched APIs）**：这类API由于早期版本中的安全漏洞已经通过后续的补丁修复。如果开发者仍使用旧版本的API而未能及时更新，就可能暴露在安全风险中。这些漏洞可能涉及数据泄露、权限提升等严重问题，而通过及时更新可以避免这些风险。

**3. 使用方式修改API（Usage-Modified APIs）**：有些API在后续版本中改变了其使用方法，比如参数的调整或返回值的改变。这些修改通常是为了提高安全性或修复漏洞，开发者若按照旧的使用方式调用API，可能会导致代码无法正确运行，甚至引发安全隐患。

![这是一张图片](https://www.gptsecurity.info/img/in-post/1107/07.png)

大语言模型的训练往往使用静态的、长期未更新的数据集，导致其在代码生成中推荐这些过时API。这不仅影响代码的安全性，还可能导致开发者的代码与最新的API版本不兼容，进一步增加了软件维护的难度。尤其是在开发者不频繁更新其项目依赖包的情况下，模型推荐的旧版本API更容易被使用，这加剧了潜在的安全风险。

为解决这一问题，APILOT系统通过以下方式来降低这种风险：

**1. 动态API信息收集**：APILOT通过对GitHub上开源项目的提交记录进行差异分析，实时获取API的版本变化信息，特别是那些被弃用、修补或者使用方式发生变化的API。与静态训练数据不同，这种实时的API数据更新机制确保APILOT能够及时获取最新的安全修补信息。
   
**2. 自动检测与过滤**：在大语言模型生成代码的过程中，APILOT通过分析模型输出的代码，利用抽象语法树（AST）技术来精准识别使用的API。如果检测到代码中调用了过时的API，APILOT会立即进行拦截，并根据其“禁止列表”（Ban List）将这些API替换为最新、安全的API调用方式。

**3. 增强的代码生成**：APILOT不仅是一个检测和过滤系统，它还能够在模型生成的代码中动态加入安全提示，确保代码调用的是最新的API版本。通过这一机制，APILOT不仅提高了代码的安全性，还能在保持代码功能性的同时，提升其兼容性和可用性。

通过这种系统化的检测和更新机制，APILOT不仅有效避免了大语言模型生成过时、不安全的代码，还为开发者提供了更安全、更高效的编码助手。特别是在如今软件开发速度加快、版本迭代频繁的背景下，APILOT为开发者提供了一个自动化的、实时安全保障的工具，使得他们能够专注于核心开发工作，而不必担心安全漏洞问题。

## 研究概述

APILOT系统的工作流程包括三个主要阶段：首先是API的收集，系统通过分析GitHub提交记录，构建一个包含过时API信息的数据库；其次是LLMs生成代码时对这些API的检测和过滤；最后，通过增强生成过程，引导LLMs生成不包含过时API的代码。

![这是一张图片](https://www.gptsecurity.info/img/in-post/1107/08.png)

## APILOT设计

APILOT采用了一种新的API信息收集技术，通过GitHub提交差异分析来识别API的更新和修改。同时，系统使用了抽象语法树（AST）分析技术来检测代码中使用的API，并根据API的过时情况对LLMs的代码生成进行过滤和引导。APILOT的设计目标是帮助LLMs生成版本感知的安全代码，并提高生成代码的可用性。

## 研究评估

为了评估APILOT系统在解决大语言模型（LLMs）推荐过时API问题上的有效性，研究团队对其进行了广泛的实验。实验覆盖了七个最先进的LLM，包括GPT-3.5、CodeLlama等，通过多维度评估APILOT在安全性、性能和可用性方面的表现。这些实验旨在验证APILOT能否大幅减少过时API的推荐，提升生成代码的安全性，同时确保代码的功能性和实际可用性不受影响。以下是主要的评估结果：

**1. 安全性评估**

在安全性方面，评估的核心指标是过时API推荐率（FAPI），即LLM在未使用APILOT时推荐过时API的频率。实验表明，未经过优化的LLMs往往会推荐各种过时API，尤其是在处理已弃用或使用方式改变的API时。这些API可能包含严重的安全漏洞，继续使用会导致代码面临安全风险。APILOT通过其API检测和过滤机制，将这些不安全的API拦截并替换为安全版本。实验结果显示，APILOT成功将过时API的推荐率减少了89.42%，有效阻止了LLM生成不安全代码的倾向。

此外，针对三类过时API（弃用API、已修补API和使用方式修改API），APILOT的表现非常突出。例如，针对已修补API（如存在已知CVE漏洞的API），APILOT在多次测试中将推荐率从25.38%降低到仅2.1%。而对于使用方式修改的API，APILOT的推荐准确性更为显著，将推荐率从53.36%降低到5.69%。这些结果表明，APILOT能够显著减少LLM推荐使用不安全API的概率，并为开发者提供更加安全的代码建议。

**2. 性能评估**

APILOT系统的一个显著优势是其对大语言模型生成速度的影响非常小。评估过程中使用了执行时间（ExecTime）作为衡量标准，记录了APILOT在检测、过滤过时API以及增强生成过程中的时间开销。结果显示，APILOT在模型生成代码的基础上增加的时间开销非常有限，平均延迟不到1.97倍。这意味着APILOT的安全功能几乎不会显著影响代码生成的效率。

具体而言，APILOT的检测时间（SanTime）在0.5秒以下，极大程度上优化了检测和过滤的流程。即便在面对复杂的代码生成任务时，APILOT依然保持了极高的效率，确保在安全性与性能之间达到了最佳平衡。相比现有的过时API检测工具（如Purple-Llama），APILOT不仅检测更加精准，而且性能开销更低。

**3. 可用性和功能性评估**

除了安全性和性能外，APILOT还提升了生成代码的实际可用性。评估过程中，研究团队引入了可执行代码比例（ExecRate）这一指标，来衡量APILOT在不同版本的API环境下生成的代码是否能够直接编译和运行。结果显示，APILOT生成的代码在最新版本的API环境下可用性提升了31.12%，在旧版本环境下也提升了23.96%。

此外，功能性评分（ICE-Score）用来评估APILOT是否影响代码的功能性。实验结果表明，虽然APILOT的安全增强机制有时会略微降低生成代码的简洁性（平均功能评分下降约6.84%），但这些代码依然能有效完成指定任务。这种功能评分的下降主要是由于APILOT在替换过时API时引入了一些额外的安全逻辑，使得代码结构略显复杂，但其安全性和兼容性得到了极大提升。对于大多数开发者而言，APILOT生成的代码依然是实用且高效的。

**4. 迭代生成的效果**

APILOT不仅仅是一个一次性过滤系统，它还具备迭代增强生成的能力。当APILOT检测到大语言模型生成的代码中含有过时API时，它会通过多次迭代与模型进行交互，引导其在不使用过时API的前提下生成安全代码。实验显示，APILOT在3次迭代内成功引导模型生成了符合安全标准的代码，而性能开销也得到了很好的控制。

![这是一张图片](https://www.gptsecurity.info/img/in-post/1107/09.png)

这种迭代生成的策略有效地解决了大语言模型因训练数据过时而无法推荐最新安全API的问题，确保即便是模型初次生成的代码中存在问题，APILOT也能通过不断修正来生成安全的代码。这一能力在实际开发环境中尤为重要，因为开发者不必手动筛查每个API的安全性，而是可以完全依赖APILOT系统来自动完成这一过程。

**5. 用户透明度与可控性**

APILOT系统设计的一个重要特点是它的用户透明度。在实际使用中，APILOT会将所有检测到的过时API标记并反馈给用户，附带详细的安全警告和替代建议。开发者可以清晰地了解生成代码中存在的潜在问题，并根据APILOT的建议调整自己的开发流程。

![这是一张图片](https://www.gptsecurity.info/img/in-post/1107/10.png)

此外，APILOT允许开发者自定义最大迭代次数，以平衡代码安全性与生成时间。例如，在高安全需求的环境下，开发者可以将迭代次数设定为更高，以确保生成的代码无任何已知漏洞；而在性能优先的场景中，可以选择较少的迭代次数以提高效率。这种灵活性使得APILOT能够适应不同类型的开发需求，进一步增强了它的实用性和可控性。

## 论文结论

APILOT通过实时收集过时API的信息，并结合增强生成方法，引导LLMs生成安全、版本感知的代码。实验表明，APILOT可以显著减少过时API推荐，提高生成代码的安全性和可用性。未来的工作将继续优化APILOT的性能，并扩展其适用范围，以支持更多的编程语言和API库。

原作者：论文解读智能体

校对：小椰风

![这是一张图片](https://www.gptsecurity.info/img/in-post/0813/08.webp)







