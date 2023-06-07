---
title: 本地化
ms.date: 03/30/2017
helpviewer_keywords:
- culture, localization
- application development [.NET], localization
- globalization [.NET], localization
- code blocks
- international applications [.NET], localization
- global applications, localization
- world-ready applications, localization
- user interface blocks
- localization [.NET], about localization
- localizing resources
ms.assetid: 49d520d7-92d7-44ee-bb24-8b615db1d41b
ms.openlocfilehash: 5d47d002c714ea80b6f94c810f2dca726c273134
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94829825"
---
# <a name="localization"></a>本地化

本地化是针对应用支持的每个区域性，将应用资源转换为本地化版本的过程。 只有在完成[可本地化评审](localizability-review.md)步骤，以验证全球化应用是否做好本地化准备后，才应继续执行本地化步骤。

可以开始进行本地化的应用程序分为两个概念块：一个是包含所有用户界面元素的块，另一个是包含可执行代码的块。 用户界面块仅包含可本地化的用户界面元素，如字符串、错误消息、对话框、菜单、嵌入的对象资源等区域性中性元素。 代码块仅包含所有支持的区域性要使用的应用代码。 公共语言运行时支持附属程序集资源模型，用于将应用的可执行代码与资源分隔开来。 若要详细了解如何实现此模型，请参阅 [.NET 中的资源](../../framework/resources/index.md)。

对于应用的每个本地化版本，请添加新的附属程序集，其中包含转换为目标区域性的相应语言的本地化用户界面块。 所有区域性的代码块应保持不变。 用户界面块的本地化版本和代码块组合生成了应用的本地化版本。

Windows 软件开发工具包 (SDK) 提供了 Windows 窗体资源编辑器 (Winres.exe)，以便于针对目标区域性快速本地化 Windows 窗体。 若要了解如何使用此工具，请参阅 [Winres.exe（Windows 窗体资源编辑器）](../../framework/tools/winres-exe-windows-forms-resource-editor.md)。

## <a name="see-also"></a>请参阅

- [全球化和本地化](index.md)
- [可本地化性审核](localizability-review.md)
- [全球化](globalization.md)
- [桌面应用中的资源](../../framework/resources/index.md)
