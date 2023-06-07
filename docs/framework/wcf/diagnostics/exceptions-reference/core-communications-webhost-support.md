---
title: 核心通信：Webhost 支持
ms.date: 03/30/2017
ms.assetid: 034c501f-96f9-4ef7-9602-3ac21788fd3e
ms.openlocfilehash: 8ee107ffcb9fab629541ce004d1c587fcad652c8
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "61998707"
---
# <a name="core-communications-webhost-support"></a>核心通信：Webhost 支持

本主题列出由 Webhost 支持生成的所有异常。

## <a name="exception-list"></a>异常列表

|资源代码|资源字符串|
|-------------------|---------------------|
|Hosting_CompatibilityServiceNotHosted|此服务需要 ASP.NET 兼容性。 它还必须承载于 IIS 中。 将服务承载于 IIS 中，并在 Web.config 中打开 ASP.NET 兼容性，或将 AspNetCompatibilityRequirementsAttribute.AspNetCompatibilityRequirementsMode 属性设置为“Required”以外的值。|
|Hosting_ListenerNotFoundForActivationInRecycling|当前没有侦听指定地址的通道。 如果正在回收应用程序，则会关闭服务。|
|Hosting_NonHTTPInCompatibilityMode|受 ASP.NET 兼容性支持的协议只有 HTTP 和 HTTPS。 删除指定终结点或对应用程序禁用 ASP.NET 兼容性。|
|Hosting_ProcessNotExecutingUnderHostedContext|不能在当前宿主环境内调用指定宿主进程。 此 API 要求调用应用程序承载于 Internet 信息服务或 Windows 进程激活服务中。|
|Hosting_ServiceCompatibilityRequire|无法激活此服务，因为此服务需要 ASP.NET 兼容性。 没有对此应用程序启用 ASP.NET 兼容性。 请在 Web.config 文件中启用 ASP.NET 兼容性，或设置 AspNetCompatibilityRequirementsAttribute.AspNetCompatibility。|
