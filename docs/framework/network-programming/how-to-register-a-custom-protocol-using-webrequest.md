---
title: 如何：使用 WebRequest 注册自定义协议
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 98ddbdb9-66b1-4080-92ad-51f5c447fcf8
ms.openlocfilehash: 86ad862dbff9f4a982eec27a6806bcbb6c16f3ae
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96255801"
---
# <a name="how-to-register-a-custom-protocol-using-webrequest"></a>如何：使用 WebRequest 注册自定义协议

此示例演示如何注册在其他位置定义的特定于协议的类。 在此示例中，`CustomWebRequestCreator` 是用户实现对象，它实现返回 `CustomWebRequest` 对象的“Create”方法。 此代码示例假定已编写了实现自定义协议的 `CustomWebRequest` 代码。  
  
## <a name="example"></a>示例  
  
```csharp  
WebRequest.RegisterPrefix("custom", new CustomWebRequestCreator());  
WebRequest req = WebRequest.Create("custom://customHost.contoso.com/");  
```  
  
```vb  
WebRequest.RegisterPrefix("custom", New CustomWebRequestCreator())  
Dim req As WebRequest = WebRequest.Create("custom://customHost.contoso.com/")  
```  
  
## <a name="compiling-the-code"></a>编译代码  

 此示例需要：  
  
 对 <xref:System.Net> 命名空间的引用。  
  
## <a name="see-also"></a>另请参阅

- [对可插入协议进行编程](programming-pluggable-protocols.md)
