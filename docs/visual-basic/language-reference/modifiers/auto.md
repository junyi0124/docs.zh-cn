---
title: 自动
ms.date: 07/20/2015
f1_keywords:
- vb.Auto
helpviewer_keywords:
- Auto keyword [Visual Basic], external references
- Declare statement [Visual Basic], marshaling strings
- Auto keyword [Visual Basic]
- Auto keyword [Visual Basic], marshaling strings
ms.assetid: bf79ba95-a62c-48a5-916f-0ac7a52c13ec
ms.openlocfilehash: 799a7320b701384dc5f4b4b46fef8544f6b15b02
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90875509"
---
# <a name="auto-visual-basic"></a>Auto (Visual Basic)

指定 Visual Basic 应根据正在声明的外部过程的外部名称按 .NET Framework 规则封送字符串。  
  
 调用在项目外部定义的过程时，Visual Basic 编译器不能访问正确调用过程所必须具有的信息。 此信息包括过程的位置、标识方法、调用序列和返回类型，以及它使用的字符串字符集。 [Declare 语句](../statements/declare-statement.md)创建对外部过程的引用并提供此必要信息。  
  
 `charsetmodifier`语句中的部分 `Declare` 提供在调用外部过程期间封送处理字符串的字符集信息。 它还会影响 Visual Basic 搜索外部文件的外部过程名称的方式。 `Auto`修饰符指定 Visual Basic 应根据 .NET Framework 规则封送字符串，并且应确定运行时平台的基本字符集，并可能在初始搜索失败时修改外部过程名称。 有关详细信息，请参阅 [Declare 语句](../statements/declare-statement.md)中的 "字符集"。  
  
 如果未指定字符集修饰符， `Ansi` 则默认为。  
  
## <a name="remarks"></a>备注  

 `Auto`可以在此上下文中使用修饰符：  
  
 [Declare Statement](../statements/declare-statement.md)  
  
## <a name="smart-device-developer-notes"></a>智能设备开发人员说明  

 不支持此关键字。  
  
## <a name="see-also"></a>另请参阅

- [适应](ansi.md)
- [Unicode](unicode.md)
- [关键字](../keywords/index.md)
