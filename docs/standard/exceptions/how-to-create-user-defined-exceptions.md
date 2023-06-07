---
title: 如何：创建用户定义异常
description: 了解如何创建用户定义的异常，这是从 .NET 中异常基类派生的异常类层次结构的替代项。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- user-defined exceptions
- exceptions, examples
- exceptions, user-defined
ms.assetid: 25819a5a-f915-4fc8-b924-a76915674e04
ms.openlocfilehash: b0a8549c9bacf322a0685c7b505185ab1d1101f6
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94828122"
---
# <a name="how-to-create-user-defined-exceptions"></a>如何创建用户定义的异常

.NET 可提供由基类 <xref:System.Exception> 最终派生的异常类层次结构。 然而，如果预定义的异常都不符合需求，可通过从 <xref:System.Exception> 类派生来创建自己的异常类。

创建自己的异常时，用户定义的异常类的名称需要以“Exception”一词结尾，并实现三个常见的构造函数，如以下示例所示。 该示例定义名为 `EmployeeListNotFoundException` 的新异常类。 该类从 <xref:System.Exception> 派生，且包含三个构造函数。

[!code-cpp[dg_exceptionDesign#14](../../../samples/snippets/cpp/VS_Snippets_CLR/dg_exceptionDesign/cpp/example2.cpp#14)]
[!code-csharp[dg_exceptionDesign#14](../../../samples/snippets/csharp/VS_Snippets_CLR/dg_exceptionDesign/cs/example2.cs#14)]
[!code-vb[dg_exceptionDesign#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/dg_exceptionDesign/vb/example2.vb#14)]  

> [!NOTE]
> 使用远程处理时，必须确保所有用户定义的异常的元数据在服务器（被调用方）可用，在客户端（代理对象或调用方）也可用。 有关详细信息，请参阅[异常的最佳做法](best-practices-for-exceptions.md)。

## <a name="see-also"></a>请参阅

- [异常](index.md)
