---
title: System.Convert 方法
ms.date: 03/30/2017
ms.assetid: 3ca6c5b6-ea5d-4ab0-b675-f082135b342c
ms.openlocfilehash: d0aa0b11223e23b874471962d727d8e16e152ceb
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/07/2019
ms.locfileid: "70781057"
---
# <a name="systemconvert-methods"></a>System.Convert 方法

[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 不支持以下 <xref:System.Convert> 方法。

- 带有 <xref:System.IFormatProvider> 参数的版本。

- 涉及字符数组或字节数组的方法：

  - <xref:System.Convert.FromBase64CharArray%2A>

  - <xref:System.Convert.ToBase64CharArray%2A>

  - <xref:System.Convert.FromBase64String%2A>

  - <xref:System.Convert.ToBase64String%2A>

- 以下方法：

  - `public static <Type2> To<Type2>(<Type1> value);`，其中

    `Type1` 和 `Type2` 各自是 `sbyte`、`uint`、`ulong` 或 `ushort` 之一。

  - C#：

    `int To<int type>(string value, int fromBase),`

    `ToString(... value, int toBase)`

  - Visual Basic：

    `Function To(Of [Numeric])(value as String, fromBase As Integer)`

    `As [Numeric], ToString( value As …, toBase As Integer)`

  - <xref:System.Convert.IsDBNull%2A>

  - <xref:System.Convert.GetTypeCode%2A>

  - <xref:System.Convert.ChangeType%2A>

## <a name="see-also"></a>请参阅

- [数据类型和函数](data-types-and-functions.md)
