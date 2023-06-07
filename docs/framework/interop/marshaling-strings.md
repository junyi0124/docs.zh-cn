---
title: 封送处理字符串
description: 查看如何封送字符串。 请参阅按值或引用封送处理字符串的选项、作为结果封送的选项、按值或引用对结构或类进行封送处理的选项等等。
ms.date: 03/30/2017
helpviewer_keywords:
- marshaling, samples
- platform invoke, marshaling data
- data marshaling, strings
- data marshaling, samples
- data marshaling, platform invoke
- marshaling. strings
- marshaling, platform invoke
- sample applications [.NET Framework], marshaling strings
ms.assetid: e21b078b-70fb-4905-be26-c097ab2433ff
ms.openlocfilehash: 2b7038a57ee0a8186288c7388fcd000daad70ed0
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96238036"
---
# <a name="marshaling-strings"></a>封送处理字符串

平台调用复制字符串参数，根据需要将其从 .NET Framework 格式 (Unicode) 转换为非托管格式 (ANSI)。 由于托管的字符串不可变，在函数返回时，平台调用并不将它们从非托管内存复制回托管内存。  
  
 下表列出了字符串的封送处理选项，描述了它们的用法，并提供了指向相应 .NET Framework 示例的链接。  
  
|String|描述|示例|  
|------------|-----------------|------------|  
|按值。|将字符串作为 In 参数传递。|[MsgBox](msgbox-sample.md)|  
|作为结果。|从非托管代码返回字符串。|[字符串](/previous-versions/dotnet/netframework-4.0/e765dyyy(v=vs.100))|  
|按引用。|使用 <xref:System.Text.StringBuilder> 将字符串作为 In/Out 参数传递。|[缓冲区](/previous-versions/dotnet/netframework-4.0/x3txb6xc(v=vs.100))|  
|结构中按值。|在作为 In 参数的结构中传递字符串。|[结构](/previous-versions/dotnet/netframework-4.0/eadtsekz(v=vs.100))|  
|结构中按引用 (char\*)。|在作为 In/Out 参数的结构中传递字符串。 非托管函数需要指向字符缓冲区的指针，并且该缓冲区大小是结构的成员。|[字符串](/previous-versions/dotnet/netframework-4.0/e765dyyy(v=vs.100))|  
|结构中按引用 (char[])。|在作为 In/Out 参数的结构中传递字符串。 非托管函数需要嵌入字符缓冲区。|[OSInfo](/previous-versions/dotnet/netframework-4.0/795sy883(v=vs.100))|  
|类中按值 (char\*)。|在类（类是 In/Out 参数）中传递字符串。 非托管函数需要指向字符缓冲区的指针。|[OpenFileDlg](/previous-versions/dotnet/netframework-4.0/w5tyztk9(v=vs.100))|  
|类中按值 (char[])。|在类（类是 In/Out 参数）中传递字符串。 非托管函数需要嵌入字符缓冲区。|[OSInfo](/previous-versions/dotnet/netframework-4.0/795sy883(v=vs.100))|  
|作为字符串数组按值。|创建按值传递的字符串数组。|[数组](marshaling-different-types-of-arrays.md)|  
|作为包含字符串的结构数组按值。|创建包含字符串的结构数组，且该数组是按值传递的。|[数组](marshaling-different-types-of-arrays.md)|  
  
## <a name="see-also"></a>请参阅

- [字符串的默认封送处理](default-marshaling-for-strings.md)
- [用平台调用封送数据](marshaling-data-with-platform-invoke.md)
- [封送类、结构和联合](marshaling-classes-structures-and-unions.md)
- [封送处理不同类型的数组](marshaling-different-types-of-arrays.md)
- [其他封送处理示例](/previous-versions/dotnet/netframework-4.0/ss9sb93t(v=vs.100))
