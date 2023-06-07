---
title: 后期绑定解决方案；可能会发生运行时错误
ms.date: 07/20/2015
f1_keywords:
- vbc42017
- BC42017
helpviewer_keywords:
- BC42017
ms.assetid: 45f552c8-57c6-44c0-97d3-e510119b257a
ms.openlocfilehash: ef0fa295cadaaa0550be4809ec97c6da13b5e2db
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92160426"
---
# <a name="bc42017-late-bound-resolution-runtime-errors-could-occur"></a>BC42017：后期绑定解析;可能出现运行时错误

对象被分配给声明为 [Object 数据类型](../data-types/object-data-type.md)的变量。

 将变量声明为时 `Object` ，编译器必须执行 *后期绑定*，这会导致在运行时产生额外的操作。 它还使应用程序易于发生潜在的运行时错误。 例如，如果将分配 <xref:System.Windows.Forms.Form> 到 `Object` 变量，然后尝试访问 <xref:System.Xml.XmlDocument.NameTable%2A?displayProperty=nameWithType> 属性，则运行时将引发， <xref:System.MemberAccessException> 因为该类不 <xref:System.Windows.Forms.Form> 公开 `NameTable` 属性。

 如果将变量声明为特定类型，编译器可以在编译时执行 *早期绑定* 。 这会提高性能、控制对特定类型的成员的访问，以及更好的代码可读性。

 默认情况下，此消息是一个警告。 有关隐藏警告或将警告视为错误的信息，请参见 [Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic)。

 **错误 ID：** BC42017

## <a name="to-correct-this-error"></a>更正此错误

- 如果可能，请将变量声明为特定类型。

## <a name="see-also"></a>另请参阅

- [早期绑定和后期绑定](../../programming-guide/language-features/early-late-binding/index.md)
- [对象变量声明](../../programming-guide/language-features/variables/object-variable-declaration.md)
