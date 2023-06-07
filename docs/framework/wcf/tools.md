---
title: Windows Communication Foundation 工具
description: 了解旨在使你更轻松地创建、部署和管理 WCF 应用程序的 WCF 工具。 在命令提示符下运行这些工具。
ms.date: 03/30/2017
helpviewer_keywords:
- WCF, tools
- Windows Communication Foundation, tools
ms.assetid: 399a47b4-bfea-434b-8e83-f76b5063d79d
ms.openlocfilehash: 53791cdd7789aedf19792ca628a666c963512821
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96255138"
---
# <a name="windows-communication-foundation-tools"></a>Windows Communication Foundation 工具

Microsoft Windows Communication Foundation (WCF) 工具旨在使你更轻松地创建、部署和管理 WCF 应用程序。 本节包含有关这些工具的详细信息。 请注意，这些工具不受支持。  
  
 您可以从命令行中运行所有工具。  
  
 下表列出这些工具并提供了简要说明。  
  
|工具|说明|  
|----------|-----------------|  
|[ServiceModel 元数据实用工具 (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md)|依据元数据文档生成服务模块代码，以及依据服务模块代码生成元数据文档。|  
|[“查找私钥”工具 (FindPrivateKey.exe)](find-private-key-tool-findprivatekey-exe.md)|从指定的存储中检索私钥。|  
|[ServiceModel 注册工具 (ServiceModelReg.exe)](servicemodelreg-exe.md)|管理 ServiceModel 在单一计算机上的注册和注销。|  
|[COM+ 服务模块配置工具 (ComSvcConfig.exe)](com-service-model-configuration-tool-comsvcconfig-exe.md)|配置要作为 Web 服务公开的 COM+ 接口。|  
|[配置编辑器工具 (SvcConfigEditor.exe)](configuration-editor-tool-svcconfigeditor-exe.md)|创建和修改 WCF 服务的配置设置。|  
|[服务跟踪查看器工具 (SvcTraceViewer.exe)](service-trace-viewer-tool-svctraceviewer-exe.md)|帮助您查看、分组和筛选跟踪消息，以便能够诊断、修复和验证 WCF 服务的问题。|  
|[WS-AtomicTransaction 配置实用工具 (wsatConfig.exe)](ws-atomictransaction-configuration-utility-wsatconfig-exe.md)|使用命令行工具配置基本的 WS-AtomicTransaction 支持设置。|  
|[WS-AtomicTransaction 配置 MMC 管理单元](ws-atomictransaction-configuration-mmc-snap-in.md)|使用 MMC 管理单元配置基本的 WS-AtomicTransaction 支持设置。|  
|[工作流服务注册工具 (WFServicesReg.exe)](workflow-service-registration-tool-wfservicesreg-exe.md)|注册 Windows 工作流服务。|  
|[WCF 服务主机 (WcfSvcHost.exe)](wcf-service-host-wcfsvchost-exe.md)|承载 ( * .dll) 文件的库中包含的 WCF 服务|  
|[WCF 测试客户端 (WcfTestClient.exe)](wcf-test-client-wcftestclient-exe.md)|一个 GUI 工具，通过使用该工具，可以输入任意类型的参数、将该输入提交给服务并查看服务发回的响应。|  
|[协定优先工具](contract-first-tool.md)|一个 Visual Studio 任务，该任务从 XSD 数据协定创建代码类。|  
  
 Windows SDK 附带了上面除 ServiceModelReg.exe、WsatConfig.exe 和 ComSvcConfig.exe 之外的所有工具，这些工具可在 C:\Program Files\Microsoft SDKs\Windows\v6.0\Bin 文件夹下找到。  3 个特定的工具可在 C:\Windows\Microsoft.NET\Framework\v3.0\Windows Communication Foundation 下找到。
