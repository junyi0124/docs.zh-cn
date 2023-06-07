---
title: BinaryFormatter 安全指南
description: 本文介绍了 BinaryFormatter 类型中固有的安全风险，以及针对要使用的不同序列化程序的建议。
ms.date: 07/11/2020
ms.author: levib
author: GrabYourPitchforks
ms.openlocfilehash: 2c76a81650e5b83677f6c4df64770bd1ef5f775e
ms.sourcegitcommit: cbb19e56d48cf88375d35d0c27554d4722761e0d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/19/2020
ms.locfileid: "88607927"
---
# <a name="binaryformatter-security-guide"></a>BinaryFormatter 安全指南

本文适用于以下 .NET 实现：

* 所有版本的 .NET Framework
* .NET Core 2.1 - 3.1
* .NET 5.0 及更高版本

## <a name="background"></a>背景

> [!WARNING]
> <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 类型会带来风险，不建议将其用于数据处理。 即使应用程序认为自己正在处理的数据是可信的，也应尽快停止使用 `BinaryFormatter`。 `BinaryFormatter` 不安全，无法确保安全。

本文适用于以下类型：

* <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter>
* <xref:System.Runtime.Serialization.NetDataContractSerializer>
* <xref:System.Web.UI.LosFormatter>
* <xref:System.Web.UI.ObjectStateFormatter>

反序列化漏洞是指不安全地处理请求有效负载的威胁类别。 成功利用这些漏洞攻击应用的攻击者可导致目标应用内出现拒绝服务 (DoS)、信息泄露或远程代码执行。 此风险类别始终是 [10 项最严重的 OWASP 风险](https://owasp.org/www-project-top-ten/)之一。 攻击目标包括使用[多种语言](https://owasp.org/www-community/vulnerabilities/Deserialization_of_untrusted_data)（包括 C/C++、Java 和 C#）编写的应用。

在 .NET 中，风险最大的目标是使用 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 类型来反序列化数据的应用。 `BinaryFormatter` 因为其强大的功能和易用性而广泛用于整个 .NET 生态系统。 但是，其强大的功能也让攻击者能够影响目标应用内的控制流。 成功的攻击可能导致攻击者能够在目标进程的上下文中运行代码。

更简单的比喻是，假设在有效负载上调用 `BinaryFormatter.Deserialize` 相当于将该有效负载解释为独立的可执行文件并启动它。

## <a name="binaryformatter-security-vulnerabilities"></a>BinaryFormatter 安全漏洞

> [!WARNING]
> 将 `BinaryFormatter.Deserialize` 方法用于不受信任的输入时，该方法永远都不安全。 强烈建议使用者改为考虑使用本文后面概述的替代方法之一。

`BinaryFormatter` 是在反序列化漏洞成为一个众所周知的威胁类别之前实现的。 因此，代码不遵循现代最佳做法。 `Deserialize` 方法可用作攻击者对使用中的应用执行 DoS 攻击的载体。 这些攻击可能导致应用无响应或进程意外终止。 使用 `SerializationBinder` 或任何其他 `BinaryFormatter` 配置开关都无法缓解此类攻击。 .NET 认为此行为是设计使然，因此不会发布代码更新来修改此行为。

使用 `BinaryFormatter.Deserialize` 可能容易遭受其他攻击类别的攻击，如信息泄露或远程代码执行。 利用自定义 <xref:System.Runtime.Serialization.SerializationBinder> 等功能可能不足以适当缓解这些风险。 存在发现新漏洞的可能性，而 .NET 实际上无法为此发布安全更新。 使用者应该评估其各个应用场景，并考虑他们遇到这些风险的可能性。

我们建议 `BinaryFormatter` 使用者对其应用执行单独的风险评估。 由使用者完全负责确定是否利用 `BinaryFormatter`。 使用者应该对使用 `BinaryFormatter` 的安全性、技术、声誉、法律和监管要求进行风险评估。

## <a name="preferred-alternatives"></a>首选替代方法

.NET 提供了多个随附的序列化程序，可用于安全处理不受信任的数据：

* <xref:System.Xml.Serialization.XmlSerializer> 和 <xref:System.Runtime.Serialization.DataContractSerializer>，用于将对象图序列化为 XML 或从 XML 序列化对象图。 不要将 `DataContractSerializer` 与 <xref:System.Runtime.Serialization.NetDataContractSerializer> 混淆。
* <xref:System.IO.BinaryReader> 和 <xref:System.IO.BinaryWriter>，适用于 XML 和 JSON。
* <xref:System.Text.Json> API，用于将对象图序列化为 JSON。

## <a name="dangerous-alternatives"></a>危险的替代方法

避免使用以下序列化程序：

* <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter>
* <xref:System.Web.UI.LosFormatter>
* <xref:System.Runtime.Serialization.NetDataContractSerializer>
* <xref:System.Web.UI.ObjectStateFormatter>

上述序列化程序都执行不受限制的多态反序列化，并且会带来风险，就像 `BinaryFormatter` 一样。

## <a name="the-risks-of-assuming-data-to-be-trustworthy"></a>假设数据值得信任的风险

通常，应用开发人员可能会认为他们只是在处理受信任的输入。 在一些罕见的情况下，可实现真正的安全输入。 但更常见的情况是，有效负载跨越了信任边界，而开发人员却没有意识到这一点。

考虑本地服务器，员工在其中使用其工作站的桌面客户端与服务进行交互。 这个场景可能被天真地视为可以接受使用 `BinaryFormatter` 的“安全”设置。 但是，这个场景为恶意软件提供了一个载体，使恶意软件能够访问单个员工的计算机，从而能够在整个企业中传播。 该恶意软件可以利用企业使用 `BinaryFormatter` 造成的漏洞，从员工的工作站横向移动到后端服务器。 然后，它可以泄露公司的敏感数据。 此类数据可能包括商业机密或客户数据。

还考虑使用借助 `BinaryFormatter` 来保持保存状态的应用。 最初看来这似乎是一个安全的方案，因为在你自己的硬盘驱动器上读写数据威胁较低。 但是，通过电子邮件或 Internet 共享文档是很常见的，并且大多数最终用户不会认为打开这些下载的文件属于危险行为。

攻击者可以利用此场景来制造恶意结果。 如果应用是一款游戏，则共享保存文件的用户会在不知情的情况下面临风险。 开发者自身也可能成为目标。 攻击者可能会通过电子邮件向开发者的技术支持人员发送电子邮件，并添加恶意数据文件作为附件，然后要求支持人员打开该文件。 这种攻击可以为攻击者提供一个在企业中的据点。

另一种场景是数据文件存储在云存储空间中，并在用户的计算机之间自动同步。 能够访问云存储帐户的攻击者可以对数据文件进行病毒攻击。 此数据文件将自动同步到用户的计算机。 用户下一次打开数据文件时，攻击者的有效负载就会运行。 因此，攻击者可以利用云存储帐户泄露来获得完整的代码执行权限。

考虑从桌面安装模型迁移到云优先模型的应用。 此场景包括从桌面应用或丰富客户端模型迁移到基于 Web 的模型的应用。 任何为桌面应用绘制的威胁模型都不一定适用于基于云的服务。 桌面应用的威胁模型可能会消除某个给定的威胁，因为“客户端对攻击自己不感兴趣”。 但是，当考虑到远程用户（客户端）攻击云服务本身时，同样的威胁可能会变得有意义。

> [!NOTE]
> 通常，序列化的目的是将对象传入或传出应用。 威胁建模练习几乎始终将此类数据传输标记为跨越信任边界。

## <a name="further-resources"></a>其他资源

* [YSoSerial.Net](https://github.com/pwntester/ysoserial.net)，提供有关攻击者如何使用 `BinaryFormatter` 来攻击应用的研究。
* 反序列化漏洞的一般背景：
  * [10 项最严重的 OWASP 风险 - A8:2017 不安全的反序列化](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A8-Insecure_Deserialization)
  * [CWE-502：不受信任的数据的反序列化](https://cwe.mitre.org/data/definitions/502.html)
