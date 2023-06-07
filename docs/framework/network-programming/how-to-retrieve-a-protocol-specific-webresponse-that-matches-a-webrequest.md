---
title: 如何：检索与 WebRequest 匹配的特定于协议的 WebResponse
description: 了解如何检索与 .NET Framework 中的 WebRequest 匹配的特定于协议的 WebResponse。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d8c90785-f16b-42a5-8439-ed2f731b2ba8
ms.openlocfilehash: fd163c115dcd19c05f93f4c202b043440834ba9d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96266735"
---
# <a name="how-to-retrieve-a-protocol-specific-webresponse-that-matches-a-webrequest"></a>如何：检索与 WebRequest 匹配的特定于协议的 WebResponse

此示例演示如何检索与 WebRequest 匹配的特定于协议的 WebResponse。  
  
## <a name="example"></a>示例  
  
```csharp  
WebRequest req = WebRequest.Create("http://www.contoso.com/");  
WebResponse resp = req.GetResponse();  
```  
  
```vb  
Dim req As WebRequest = WebRequest.Create("http://www.contoso.com")  
Dim resp As WebResponse = req.GetResponse()  
```  
  
## <a name="compiling-the-code"></a>编译代码  

 此示例需要：  
  
- 引用 System.Net 命名空间。  
  
## <a name="see-also"></a>请参阅

- [请求数据](requesting-data.md)
