---
title: UriTemplate 表调度程序示例
ms.date: 03/30/2017
ms.assetid: 3b32975d-ba90-4c5c-83bc-2fbb48f11c0c
ms.openlocfilehash: 57489264de62b6adbca1c98230a0f90735b3918a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96294945"
---
# <a name="uritemplate-table-dispatcher-sample"></a>UriTemplate 表调度程序示例

<xref:System.UriTemplateTable> 类提供了一个类似字典的关联表结构，该结构可用来处理一组 <xref:System.UriTemplate> 实例。 此示例演示使用 `UriTemplateTable` 生成的基本调度引擎，这是 `UriTemplateTable` 类的常见使用方案。  
  
 此示例演示 `UriTemplateTable` 类的以下关键概念：  
  
- 将委托与 `UriTemplates` 中的 `UriTemplateTable` 关联。  
  
- 使用 <xref:System.UriTemplateTable.MatchSingle%2A> 获取特定 URI 的正确处理程序委托。  
  
- 调用处理程序委托以处理请求。  
  
### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。  
  
2. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Web\UriTemplateDispatcher`  
  
## <a name="see-also"></a>另请参阅

- [UriTemplate 表](uritemplate-table-sample.md)
- [UriTemplate](uritemplate-sample.md)
