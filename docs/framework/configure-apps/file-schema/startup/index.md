---
title: 启动设置架构
ms.date: 03/30/2017
helpviewer_keywords:
- startup settings schema
- schema startup settings
- configuration schema [.NET Framework], startup settings
ms.assetid: 03de6972-442a-4648-9f3e-efa654e3b949
ms.openlocfilehash: e5f9c9af64ff38e7c0f1f26ccab039261b052e30
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "61701510"
---
# <a name="startup-settings-schema"></a>启动设置架构

启动设置会指定应运行应用程序的公共语言运行时的版本。  
  
|元素|说明|  
|-------------|-----------------|  
|[\<requiredRuntime>](requiredruntime-element.md)|指定应用程序仅支持 1.0 版本的公共语言运行时。 使用运行时版本1.1 生成的应用程序应使用 **\<supportedRuntime>** 元素。|  
|[\<supportedRuntime>](supportedruntime-element.md)|指定应用程序支持的公共语言运行时版本。|  
|[\<startup>](startup-element.md)|包含 **\<requiredRuntime>** 和 **\<supportedRuntime>** 元素。|  
  
## <a name="see-also"></a>请参阅

- [配置文件架构](../index.md)
- [如何：将应用配置为支持 .NET Framework 4 或更高版本](../../../migration-guide/how-to-configure-an-app-to-support-net-framework-4-or-4-5.md)
