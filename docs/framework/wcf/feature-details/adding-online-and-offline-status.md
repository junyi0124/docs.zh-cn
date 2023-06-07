---
title: 添加联机和脱机状态
ms.date: 03/30/2017
ms.assetid: 05e5f51d-81b6-4c17-b364-9dda447a5fce
ms.openlocfilehash: 08ae4a20ced9626504c9fd2045416460e0878c5b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96292917"
---
# <a name="adding-online-and-offline-status"></a>添加联机和脱机状态

在很多情况下，应用程序必须监控有关对等通道连接状态的特定详细信息。 通过对 `GetProperty` 接口的实现调用 <xref:System.ServiceModel.IOnlineStatus> 方法可以获取此信息。 实现此接口的对象可以监视连接状态或注册事件处理程序（如 `OnOnline` 和 `OnOffline`），并对联机状态的更改立即做出反应。  
  
 在对等通道基础结构中，如果客户端连接到至少一个其他对等客户端，则将此客户端视为联机；否则，视为脱机。 在调试开发应用程序或向最终用户显示详细信息时，此功能特别有用。  
  
> [!NOTE]
> 联机事件处理程序首先应确保节点在发送消息之前处于打开状态。  
  
## <a name="see-also"></a>另请参阅

- [生成对等通道应用程序](building-a-peer-channel-application.md)
