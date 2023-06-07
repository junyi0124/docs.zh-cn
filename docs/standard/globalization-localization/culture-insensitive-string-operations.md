---
title: 不区分区域性的字符串操作
ms.date: 03/30/2017
helpviewer_keywords:
- culture, culture-insensitive string operations
- case-sensitive comparisons
- globalization [.NET], culture-insensitive string operations
- strings [.NET], culture-insensitive string operations
- localization [.NET], culture-insensitive string operations
- world-ready applications, culture-insensitive string operations
- culture-sensitive string operations
- culture-insensitive string operations
ms.assetid: e6e2bb94-a95d-44e2-b68c-cfdd1db77784
ms.openlocfilehash: 2cfd4bf3428832c204124637fbbe3de2edd554f9
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94827121"
---
# <a name="culture-insensitive-string-operations"></a>不区分区域性的字符串操作

如果要创建旨在按区域性向用户显示结果的应用程序，则区分区域性的字符串操作无疑是一个有利条件。 默认情况下，区分区域性的方法从当前线程的 <xref:System.Globalization.CultureInfo.CurrentCulture%2A> 属性中获得要使用的区域性。

有时候，并不需要执行区分区域性的字符串操作。 当结果不应依赖于区域性时，使用区分区域性的操作可能会导致应用程序代码在遇到自定义事例映射和排序规则的区域性时失败。 相关示例，请参阅[使用字符串的最佳做法](../base-types/best-practices-strings.md)一文中的[“使用当前区域性的字符串比较”](../base-types/best-practices-strings.md#string-comparisons-that-use-the-current-culture)部分。

字符串操作是否应该区分区域性取决于应用程序使用结果的方式。 向用户显示结果的字符串操作通常应是区分区域性的。 例如，如果应用程序在列表框中显示本地化字符串的排序列表，则应用程序应执行区分区域性的排序操作。

内部使用的字符串操作的结果通常应该是不区分区域性的。 一般而言，如果应用程序使用的是不向用户显示的文件名、持久性格式或符号信息，则字符串操作的结果不应因区域性而异。 例如，如果应用程序比较字符串以确定它是否是可识别的 XML 标记，则这种比较不应是区分区域性的。 此外，如果安全决策基于字符串比较或大小写更改操作的结果，则操作应该不区分区域性，以确保结果不受 <xref:System.Globalization.CultureInfo.CurrentCulture%2A> 值的影响。

无论是否要开发包含用来处理本地化和全球化问题的代码的应用程序，你都应该对默认情况下检索区分区域性的结果的 .NET 方法有所了解。

## <a name="see-also"></a>请参阅

- [全球化和本地化](index.md)
