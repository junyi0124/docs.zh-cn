---
title: 部署服务
ms.date: 03/30/2017
ms.assetid: ac361bfb-017d-4da9-a2d7-fc0fb72d65bb
ms.openlocfilehash: 07bd2a3938f238336dc00fa2e0826a28a9363a4a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96294802"
---
# <a name="deploying-services"></a>部署服务

本主题介绍如何将 Windows Communication Foundation (WCF) 应用程序部署到运行时环境。  
  
## <a name="choosing-the-hosting-environment-for-your-application"></a>为您的应用程序选择宿主环境  

 WCF 服务设计为在支持托管代码的任意 Windows 进程中运行。 要变为活动状态，服务必须承载于创建它并控制它的上下文和生存期的运行时环境中。 宿主选项包括在最简单的控制台应用程序内运行，也包括在服务器环境中运行。服务器环境包括 Windows 服务、Internet 信息服务 (IIS) 或由 Windows 激活服务 (WAS) 托管的辅助进程等等。 若要查看适用于 WCF 应用程序的不同托管选项，请参阅 [托管服务](../hosting-services.md)。  
  
## <a name="see-also"></a>另请参阅

- [承载](../feature-details/hosting.md)
- [承载服务](../hosting-services.md)
