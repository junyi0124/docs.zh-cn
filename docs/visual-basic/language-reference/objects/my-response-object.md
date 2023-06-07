---
title: My.Response 对象
ms.date: 07/20/2015
f1_keywords:
- My.MyWebExtension.Response
- My.Response
helpviewer_keywords:
- My.Response object
ms.assetid: 626359bc-3165-40b4-bfaf-2c610e26eb5b
ms.openlocfilehash: 53e8012e762c46e6efbd8e9d2191aa62d58aa42b
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90870037"
---
# <a name="myresponse-object"></a>My.Response 对象

获取 <xref:System.Web.HttpResponse> 与关联的对象 <xref:System.Web.UI.Page> 。 使用此对象，可以将 HTTP 响应数据发送到客户端，并包含此响应的相关信息。  
  
## <a name="remarks"></a>备注  

 `My.Response`对象包含 <xref:System.Web.HttpResponse> 与页关联的当前对象。  
  
 `My.Response`对象仅适用于 ASP.NET 应用程序。  
  
## <a name="example"></a>示例  

 下面的示例从对象获取标头集合 `My.Request` ，并使用 `My.Response` 对象将其写入到 ASP.NET 页。  
  
 [!code-aspx-vb[VbVbalrMyWeb#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyWeb/VB/Default.aspx#1)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Web.HttpResponse>
- [My.Request 对象](my-request-object.md)
