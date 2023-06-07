---
title: 如何：请求网页并以数据流的形式检索结果
description: 此示例演示如何请求网页并在 .NET Framework 的流中检索结果。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d32b7f35-29d8-4fb7-ad71-d219edc5e359
ms.openlocfilehash: bd57f9af6be29c783d044e785ebb36aaa8592df2
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2020
ms.locfileid: "84502478"
---
# <a name="how-to-request-a-web-page-and-retrieve-the-results-as-a-stream"></a>如何：请求网页并以数据流的形式检索结果

此示例演示如何请求 Web 页并在流中检索结果。
  
## <a name="example"></a>示例

```csharp
var myClient = new WebClient();
Stream response = myClient.OpenRead("https://docs.microsoft.com/dotnet/");
// The stream data is used here.
response.Close();
```

```vb
Dim myClient As New WebClient()
Dim response As Stream = myClient.OpenRead("https://docs.microsoft.com/dotnet/")
' The stream data is used here.
response.Close()
```

## <a name="compiling-the-code"></a>编译代码

 此示例需要：

- 对 <xref:System.IO> 和 <xref:System.Net> 命名空间的引用。

## <a name="see-also"></a>请参阅

- [请求数据](requesting-data.md)
