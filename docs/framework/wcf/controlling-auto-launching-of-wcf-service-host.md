---
title: 控制 WCF 服务主机的自动启动
ms.date: 03/30/2017
f1_keywords:
- WcfOptions
ms.assetid: 6abe5d34-519b-4cef-8f02-3c0a7f125585
ms.openlocfilehash: 2033e693003d0b50bcdada428e4a5f451b3ad67e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96255073"
---
# <a name="controlling-auto-launching-of-wcf-service-host"></a>控制 WCF 服务主机的自动启动

当你在包含多个项目的同一 Visual Studio 解决方案中调试另一个项目时，可以控制 WCF 服务库项目的 Windows Communication Foundation (WCF) 服务)  ( 主机的自动启动功能。  
  
 为此，请在 **解决方案资源管理器** 中右键单击 "wcf 服务" 项目，选择 " **属性**"，然后单击 " **WCF 选项** " 选项卡。默认情况下，启用 " **调试同一解决方案中的另一个项目时启动 WCF 服务主机** " 复选框。 您可以清除该复选框，以便在同一个解决方案中调试另一个项目时，此特定项目的 WCF 服务主机不会启动。  
  
 此行为不会影响 F5 调试或此项目的“添加服务引用”功能。  
  
 该选项可用于以下项目：  
  
- WCF 服务库项目。  
  
- 顺序工作流服务库项目。  
  
- 状态机工作流服务库项目。  
  
- 联合服务库项目。  
  
## <a name="see-also"></a>另请参阅

- [WCF 服务主机 (WcfSvcHost.exe)](wcf-service-host-wcfsvchost-exe.md)
