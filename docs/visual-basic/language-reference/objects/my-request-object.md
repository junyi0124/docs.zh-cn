---
title: My.Request 对象
ms.date: 07/20/2015
f1_keywords:
- My.MyWebExtension.Request
- My.Request
helpviewer_keywords:
- My.Request object
ms.assetid: 93d5f0e2-6b60-4a2c-8652-d90216f6ad10
ms.openlocfilehash: d0459506994fb69ebfaa4186ba137044b6fe9464
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90870071"
---
# <a name="myrequest-object"></a>My.Request 对象

获取所请求的页面的 <xref:System.Web.HttpRequest> 对象。  
  
## <a name="remarks"></a>备注  

 `My.Request` 对象包含当前 HTTP 请求的相关信息。  
  
 `My.Request` 对象仅适用于 ASP.NET 应用程序。  
  
## <a name="example"></a>示例  

 下面的示例从对象获取标头集合 `My.Request` ，并使用 `My.Response` 对象将其写入到 ASP.NET 页。  
  
 [!code-aspx-vb[VbVbalrMyWeb#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyWeb/VB/Default.aspx#1)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Web.HttpRequest>
- [My.Response 对象](my-response-object.md)
