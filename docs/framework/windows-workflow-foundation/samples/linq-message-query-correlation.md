---
title: LINQ 消息查询相关性
ms.date: 03/30/2017
ms.assetid: b746872e-57b1-4514-b337-53398a0e0deb
ms.openlocfilehash: 83ffc80bd1944681da29e9058de454636aa7405b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96291097"
---
# <a name="linq-message-query-correlation"></a>LINQ 消息查询相关性

此示例演示如何使用一个与系统提供的 <xref:System.ServiceModel.Dispatcher.MessageQuery> 相对的自定义 <xref:System.ServiceModel.XPathMessageQuery> 实现，执行基于内容的相关性。  
  
## <a name="demonstrates"></a>演示  

 自定义 <xref:System.ServiceModel.Dispatcher.MessageQuery>，基于内容的相关性。  
  
## <a name="discussion"></a>讨论 (Discussion)  

 此示例演示如何出于相关性的目的从 <xref:System.ServiceModel.Dispatcher.MessageQuery> 基类进行扩展。 自定义实现 `LinqMessageQuery` 允许用户提供一个 XName 以使用 XLinq 在消息中查找。 此查询检索的数据用于构成相关性键，以将消息调度到相应的工作流实例。  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 此示例使用 HTTP 终结点公开一个工作流服务。 若要运行此示例，必须添加正确的 URL Acl (参阅 [配置 HTTP 和 HTTPS](../../wcf/feature-details/configuring-http-and-https.md) 以获取详细信息) ，方法是以管理员身份运行 Visual Studio，或在提升的提示符下执行以下命令来添加相应的 acl。 确保替换了域和用户名。  
  
    ```console  
    netsh http add urlacl url=http://+:8000/ user=%DOMAIN%\%UserName%  
    ```  
  
2. 在添加 URL ACL 后，请使用下列步骤。  
  
    1. 生成解决方案。  
  
    2. 通过右键单击解决方案并选择 " **设置启动项目**" 来设置多个启动项目。 将 **服务** 和 **客户端** (按顺序添加) 为多个启动项目。  
  
    3. 运行该应用程序。 客户端控制台演示工作流如何发送订单、接收订购单 ID 以及随后确认订单。 服务窗口将显示正在处理的请求。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Scenario\Services\LinqMessageQueryCorrelation`
