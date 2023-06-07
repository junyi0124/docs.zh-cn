---
title: 缓解：XML 架构验证
description: 如果在 .NET Framework 4.6 中使用复合键且有一个键为空，则 XSD 架构验证将检测是否违反唯一约束。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: b73dd4f4-f2dc-47a2-9425-3896e92321fb
ms.openlocfilehash: 0744a703c1e9dc35a8accc448d34f1a736e77e4d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96253526"
---
# <a name="mitigation-xml-schema-validation"></a>缓解：XML 架构验证

在 .NET Framework 4.6 中，如果使用复合键且有一个键为空，则 XSD 架构验证将检测是否违反唯一约束。  
  
## <a name="impact"></a>影响  

 此更改的影响应该很小：根据架构规范，如果因使用具有空键的复合键违反了 `xsd:unique`，则会出现架构验证错误。  
  
## <a name="mitigation"></a>缓解  

 是否在复合键具有一个空键的情况下检测架构验证错误是一项可配置的功能：  
  
- 从面向 .NET Framework 4.6 的应用开始，默认情况下会启用架构验证错误的检测；但可以选择退出此行为，这样将不会检测架构验证错误。  
  
- 在 .NET Framework 4.6 下运行但面向 .NET Framework 4.5.2 及更早版本的应用中，默认情况下不会检测架构验证错误；但可以选择加入此行为，这样将检测架构验证错误。  
  
 此行为可以通过使用 <xref:System.AppContext> 类定义 `System.Xml.IgnoreEmptyKeySequences` 开关的值来配置。 由于开关的默认值为 `false`（不忽略空键顺序），面向 .NET Framework 4.6 的应用可以通过使用下面的代码将开关的值设置为 `true` 以选择退出该行为：  
  
 [!code-csharp[AppCompat.IgnoreEmptyKeySequences#1](../../../samples/snippets/csharp/VS_Snippets_CLR/appcompat.ignoreemptykeysequences/cs/program.cs#1)]
 [!code-vb[AppCompat.IgnoreEmptyKeySequences#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/appcompat.ignoreemptykeysequences/vb/module1.vb#1)]  
  
 对于面向 .NET Framework 4.5.2 和更早版本的应用，由于开关的默认值为 `true`（忽略空键顺序），可以通过使用下面的代码将开关的值设置为 `false` 来确保具有空键的复合键生成架构验证错误。  
  
 [!code-csharp[AppCompat.IgnoreEmptyKeySequences#2](../../../samples/snippets/csharp/VS_Snippets_CLR/appcompat.ignoreemptykeysequences/cs/program.cs#2)]
 [!code-vb[AppCompat.IgnoreEmptyKeySequences#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/appcompat.ignoreemptykeysequences/vb/module1.vb#2)]  
  
## <a name="see-also"></a>请参阅

- [应用程序兼容性](application-compatibility.md)
