---
title: 如何：启用 Net.TCP 端口共享服务
description: 了解如何使用 MMC 配置 Net TCP 端口共享服务，以启用默认情况下禁用的 Net.tcp。
ms.date: 03/30/2017
helpviewer_keywords:
- port sharing [WCF]
- activation services [WCF]
ms.assetid: c9175af4-c27c-4765-bf45-b8f7528a7282
ms.openlocfilehash: 7200d82e4a45ce9e36b2a4cec3d0c08e1a5f00ab
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96265461"
---
# <a name="how-to-enable-the-nettcp-port-sharing-service"></a>如何：启用 Net.TCP 端口共享服务

Windows Communication Foundation (WCF) 使用称为 Net.tcp 端口共享服务的 Windows 服务，以便在多个进程之间共享 TCP 端口。 此服务作为 WCF 的一部分进行安装，但默认情况下不启用该服务作为安全预防措施，因此在首次使用之前必须手动启用。 本主题描述如何使用 Microsoft 管理控制台 (MMC) 管理单元配置 Net TCP 端口共享服务。  
  
 启用 Net.tcp 端口共享服务并手动启动后，请参阅 [如何：配置 WCF 服务以使用端口共享](how-to-configure-a-wcf-service-to-use-port-sharing.md) ，了解有关如何将服务配置为使用此服务的信息。  
  
 有关使用 net.tcp：//端口共享的示例，请参阅 [Net.tcp 端口共享示例](../samples/net-tcp-port-sharing-sample.md)。  
  
### <a name="to-enable-the-nettcp-port-sharing-service-using-mmc"></a>使用 MMC 启用 Net.TCP 端口共享服务  
  
1. 从 "开始" 菜单中，打开 "命令提示符" 窗口并键入 `services.msc` 或打开 "运行"，然后 `services.msc` 在 "打开" 框中键入来打开服务管理控制台。  
  
2. 在服务列表的 " **名称** " 列中，右键单击 **Net.tcp 端口共享服务**，然后从菜单中选择 " **属性** "。  
  
3. 若要启用该服务的手动启动，请在 " **属性** " 窗口中选择 " **常规** " 选项卡，并在 " **启动类型** " 框中选择 "手动"，然后单击 " **应用**"。  
  
4. 若要启动该服务，请在 "服务状态" 区域中，单击 " **开始** " 按钮。 现在，服务状态区域应显示为“已启动”。  
  
5. 若要返回到服务列表，请单击 **"确定**"，然后退出 MMC 控制台。  
  
## <a name="example"></a>示例  
  
## <a name="see-also"></a>另请参阅

- [Net.TCP 端口共享](net-tcp-port-sharing.md)
- [配置 Net.TCP 端口共享服务](configuring-the-net-tcp-port-sharing-service.md)
