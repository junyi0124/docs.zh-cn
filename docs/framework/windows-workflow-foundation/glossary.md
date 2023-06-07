---
title: 针对 .NET Framework 4.5 的 Windows Workflow Foundation 词汇表
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Workflow Foundation [WF], glossary
- WF [WF], glossary
ms.assetid: ab682b2f-3779-45ca-b831-b7c03d7dbb3a
ms.openlocfilehash: 61d9acab1161302937e240eb8ebb506ca9f1585c
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "61945621"
---
# <a name="windows-workflow-foundation-glossary-for-net-framework-45"></a>针对 .NET Framework 4.5 的 Windows Workflow Foundation 词汇表

Windows Workflow Foundation 文档中使用了以下术语。

## <a name="terms"></a>术语

|术语|定义|
|----------|----------------|
|活动|Windows Workflow Foundation 中的程序行为单元。 可将单个活动组合在一起，形成更复杂的活动。|
|activity action（活动操作）|用于公开工作流和活动执行的回调的数据结构。|
|argument（参数）|定义流入和流出活动的数据。 每个参数都有特定的方向：in、out 或 in/out。这些表示活动的输入参数、输出参数或输入/输出参数。|
|bookmark（书签）|活动可以暂停并等待继续的时间点。|
|compensation（补偿）|一组旨在撤消或减轻先前所完成工作的效果的操作。|
|关联|将消息路由至工作流或服务实例的机制。|
|表达式|一个构造，它采用一个或多个参数，对参数执行运算，并返回一个值。 可以在可使用活动的任意位置使用表达式。|
|flowchart（流程图）|一个众所周知的建模范例，它将程序组件表示为用方向箭头链接在一起的符号。  在 .NET Framework 4 中，可以使用 Flowchart 活动将工作流作为流程图来建模。|
|long-running process（长时间运行的进程）|不立即返回并且可跨系统重新启动的程序执行单元。|
|持久性|将工作流或服务的状态保存到持久性介质中，以便其可以从内存中卸载，或在系统出现故障后恢复。|
|状态机|一个众所周知的建模范例，它将程序组件表示为用事件驱动状态转换链接在一起的单个状态。  可以使用 StateMachine 活动将工作流作为状态机来建模。|
|substance（实体）|表示公共标识符下的一组相关书签，允许运行时做出有关特定书签恢复是有效还是会变为有效的决定。|
|type converter（类型转换器）|CLR 类型可以与一个或多个 System.ComponentModel.TypeConverter 派生类型关联，这些派生类型使 CLR 类型的实例与其他类型的实例之间能够互相转换。 类型转换器使用 System.ComponentModel.TypeConverterAttribute 特性的 CLR 类型的与相关联。  可以在 CLR 类型或属性上直接指定 TypeConverterAttribute。 在属性上指定的类型转换器始终优先于在属性的 CLR 类型上指定的类型转换器。|
|变量|表示必须保存并稍后访问的某些数据的存储。|
|工作流|由宿主进程调用的一个活动或活动树。|
|XAML|可扩展应用程序标记语言|
