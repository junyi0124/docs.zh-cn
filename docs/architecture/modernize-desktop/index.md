---
title: 使用 .NET Core 3.1 在 Windows 10 上对桌面应用进行现代化改造
description: 了解如何使用 .NET Core 3.1 对现有桌面应用进行现代化改造
ms.date: 05/12/2020
ms.openlocfilehash: 5861f806a9158ef761c47bc23e51327d4e2d0480
ms.sourcegitcommit: 9a4488a3625866335e83a20da5e9c5286b1f034c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83422661"
---
# <a name="modernizing-desktop-apps-on-windows-10-with-net-core-31"></a>使用 .NET Core 3.1 在 Windows 10 上对桌面应用进行现代化改造

![显示“对桌面应用进行现代化改造”电子书封面的屏幕截图。](./media/modernizing-existing-desktop-apps-ebook-cover.png)

发布者

Microsoft 开发人员部门、.NET 和 Visual Studio 产品团队

Microsoft Corporation 的一个部门

One Microsoft Way

Redmond, Washington 98052-6399

版权所有 © 2020 Microsoft Corporation

保留所有权利。 未经发布者书面许可，不得以任何形式或任何方式复制或传播本书中的任何内容。

本书“按原样”提供，表达作者的观点和看法。 本书中表达的观点、看法和信息（包括 URL 和其他 Internet 网站引用）如有更改，恕不另行通知。

本书中提及的一些示例仅用于说明，纯属虚构。 不存在任何实际关联或联系，请勿妄加推断。

Microsoft 和 <https://www.microsoft.com> 上“商标”网页列出的商标是 Microsoft 集团公司的商标。

Mac 和 macOS 是 Apple Inc. 的商标

所有其他标记和徽标均为其各自所有者的财产。

合著者：

> Microsoft .NET 团队项目经理 Olia Gavrysh 

> Kabel 创新架构师 Miguel Angel Castejón Dominguez 

参与者和审阅者：

> Microsoft .NET 团队高级项目经理 Maira Wenzel 

> Microsoft .NET 文档团队高级内容开发人员 Andy De Gorge 

> Microsoft Windows 开发人员平台团队高级项目经理 Miguel Ramos 

> Microsoft Windows 开发人员平台团队首席项目经理 Adam Braden 

> Microsoft Azure IoT 团队高级项目经理 Ricardo Minguez Pablos 

> Nish Anil，Microsoft .NET 团队高级项目经理 

> Microsoft 高级产品营销经理 Beth Massi 

> Microsoft .NET 团队合作伙伴总监项目经理 Scott Hunter 

> Marta Fuentes Lara，Kabel 

> Raúl Fernández de Córdoba，Kabel 

> Antonio Manuel Fernández Cantos，Kabel 

## <a name="introduction"></a>介绍

本书介绍了可用于通过现代化路径移动现有桌面应用程序并合并最新的运行时、语言和平台功能的策略。 你会发现，由于每个应用程序不同，并且你的要求和偏好也不同，因此没有独特的方法。 好消息是有一些常用的方法可用于向应用程序添加新特性和功能。 其中一些方法甚至不需要对代码进行重大修改。 本书介绍了所有这些功能在后台的工作原理，并说明了其实现机制。 此外，本书还详细介绍了一些对现有桌面应用程序进行现代化改造的常见场景，以便你可以找到发展项目的灵感。

Microsoft 对现有应用程序进行现代化改造的方法是让你能够灵活地创建自己的自定义路径。 本书中所述的所有现代化策略多是独立的。 你可以选择与你的应用程序相关的策略，跳过对你不重要的其他策略。 换句话说，你可以混合搭配策略以最好地满足你的应用程序需求。

## <a name="who-should-use-the-book"></a>本书的目标读者

我们为希望通过对现有 Windows Forms 和 WPF 桌面应用程序进行现代化改造以利用 .NET Core 和 Windows 10 好处的开发人员和解决方案架构师编写本书。

如果你是一位技术决策制定者（例如需要大致了解更新现有桌面应用程序的好处的企业架构师、开发主管或主管），你也可能会发现本书非常有用。

## <a name="how-to-use-the-book"></a>如何使用本书

本书介绍了需要对现有应用程序进行现代化改造的原因，以及使用 NET Core 3.1 和 MSIX 对桌面应用进行现代化改造的具体好处。 本书的内容适用于架构师和技术决策者，他们只需大概了解，不需要实现和技术分步细节。

不同的章节还提供了示例实现代码片段和屏幕截图，第 5 章专门介绍了示例应用程序的完整迁移过程。

## <a name="what-this-book-doesnt-cover"></a>本书未涵盖的内容

本书介绍了一些特定的场景（侧重于直接迁移场景），其中概述了利用现代化改造的好处且不需要重写代码的方法。

本书并未介绍如何使用 .NET Core 从头开始开发新式应用程序，也未介绍 Windows 窗体和 WPF 使用入门。 它重点介绍如何通过最新的桌面开发技术来更新现有桌面应用程序。

## <a name="samples-used-in-this-book"></a>本书中使用的示例

为了强调执行现代化改造所需的步骤，我们将使用名为 `eShopModernizing` 的示例应用程序。 此应用程序有两种样式（Windows 窗体和 WPF），我们将分步介绍如何使用 .NET Core 针对这两种风格进行现代化改造。

此外，在本书的 GitHub 存储库中，你会发现该过程的结果，如果你决定按照分步教程操作，则可以将其用作参考。

## <a name="send-your-feedback"></a>提供你的反馈

我们正在不断完善本书和相关示例，欢迎你提供反馈！ 如果对如何改进本书有任何建议，请使用 [GitHub 问题](https://github.com/dotnet/docs/issues)上任何页面底部的反馈部分进行反馈。

>[!div class="step-by-step"]
>[下一页](why-modern-applications.md)
