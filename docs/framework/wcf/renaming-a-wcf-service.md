---
title: 重命名 WCF 服务
ms.date: 03/30/2017
ms.assetid: 14235a65-b1c5-409d-b6cc-a979acd54bbd
ms.openlocfilehash: 25f9201253f02f368ccf95ddf1f7a7d78d2e1b2f
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96249717"
---
# <a name="renaming-a-wcf-service"></a>重命名 WCF 服务

本主题介绍如何重命名 WCF) 服务 (Windows Communication Foundation。  
  
## <a name="renaming-a-wcf-service"></a>重命名 WCF 服务  

 执行以下步骤以重命名 Windows Communication Foundation (WCF) 模板中的服务。  
  
- 更改实现该服务的类的名称。  
  
- 在该服务的配置文件中，将服务的名称更改为选定的新名称，如以下示例中所示。 配置文件可以是 app.config 或 web.config 文件，具体取决于宿主模型。  
  
```xml  
<system.servicemodel>  
   <services>  
      <service name="WcfService.NewName">  
      </service>  
   </services>  
</system.servicemodel>  
```  
  
- 如果服务已 webhosted，则它将使用 *\* .svc* 文件。 打开 svc 文件并修改服务的名称，如以下示例中所示。 由于没有 svc 文件，因此对于自承载的应用程序来说，此步骤不是必需的。  
  
```aspx-csharp
<%@ ServiceHost Service="WcfService.NewName">  
```  
  
## <a name="renaming-a-wcf-service-contract"></a>重命名 WCF 服务协定  
  
- 若要重命名服务协定，请执行下列步骤：  
  
- 更改服务协定的名称。  
  
- 在该服务的配置文件中，将服务协定的名称更改为选定的新名称，如以下示例中所示。 配置文件可以是 app.config 或 web.config 文件，具体取决于宿主模型。  
  
```xml  
<system.servicemodel>  
   <services>  
      <service>  
         <endpoint contract="WcfService.NewContractName" />  
      </service>  
   </services>  
</system.servicemodel>  
```
