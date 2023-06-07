---
title: 本地化评审
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- world-ready applications, localizability
- application development [.NET], localization
- localizability [.NET]
- international applications [.NET], localizability
- globalization [.NET], localizability
- culture, localizability
- localization [.NET], localizability
- global applications, localizability
- localizing resources
ms.assetid: 3aee2fbb-de47-4e37-8fe4-ddebb9719247
ms.openlocfilehash: ecfba7b6b5908a16bb23860704a35035f58e3ed4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95686014"
---
# <a name="localizability-review"></a>本地化评审

本地化分析检查是全球通用应用程序开发中的一个中间步骤。 它验证全球化应用程序是否已准备好进行本地化，以及是否能够识别需要特别处理的所有代码或所有用户界面元素。 此步骤还有助于确保本地化过程不会将任何功能缺陷引入应用程序。 一旦本地化分析检查提出的所有问题都得到解决，就意味着可以对应用程序进行本地化了。 如果本地化分析评审详尽彻底，则在本地化过程中应该不需要修改任何源代码。

本地化分析检查包括以下三项检查：

- [是否已实现全球化建议？](#global)

- [是否已正确处理区域性敏感型功能？](#culture)

- [是否已使用国际数据测试应用？](#test)

<a name="global"></a>

## <a name="implement-globalization-recommendations"></a>实现全球化建议

如果在设计和开发应用时考虑了本地化因素，并且遵循了[全球化](globalization.md)一文中给出的建议，那么可本地化评审在很大程度上就会成为质量保证关口。 否则，请在此阶段评审并实现[全球化](globalization.md)建议，修复源代码中妨碍本地化的错误。

<a name="culture"></a>

## <a name="handle-culture-sensitive-features"></a>处理区分区域性的功能

.NET 在许多方面都不提供编程支持，而且各区域性之间差别很大。 大多数情况下，你必须编写自定义代码来处理诸如以下方面的功能特性：

- 地址

- 电话号码

- 纸张大小

- 用于长度、重量、面积、体积和温度的度量单位

   虽然 .NET 不对度量单位之间的转换提供内置支持，但可以使用 <xref:System.Globalization.RegionInfo.IsMetric%2A?displayProperty=nameWithType> 属性确定特定国家或地区是否使用公制，如下面的示例所示。

   [!code-csharp[Conceptual.Localizability#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.localizability/cs/ismetric1.cs#1)]
   [!code-vb[Conceptual.Localizability#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.localizability/vb/ismetric1.vb#1)]

<a name="test"></a>

## <a name="test-your-application"></a>测试应用程序

在本地化应用程序之前，应当使用国际数据在操作系统的国际版本上对其进行测试。 虽然此时不会对大部分用户界面进行本地化，但可以检测到如下问题：

- 在操作系统版本之间无法正确执行反序列化的序列化数据。

- 不反映当前区域性的约定的数值数据。 例如，显示的数字可能带有错误的组分隔符、小数分隔符或货币符号。

- 不反映当前区域性的约定的日期和时间数据。 例如，表示月和日的数字可能会以错误的顺序出现，日期分隔符可能不正确，或者时区信息可能不正确。

- 找不到的资源，因为尚未确定应用程序的默认区域性。

- 以特定区域性中的异常顺序显示的字符串。

- 返回意外结果的字符串比较或相等比较。

如果在开发应用时遵循了全球化建议，并正确处理了区域性敏感型功能，同时还发现并解决了测试期间出现的本地化问题，就可以执行下一步[本地化](localization.md)。

## <a name="see-also"></a>另请参阅

- [全球化和本地化](index.md)
- [本地化](localization.md)
- [全球化](globalization.md)
- [桌面应用中的资源](../../framework/resources/index.md)
