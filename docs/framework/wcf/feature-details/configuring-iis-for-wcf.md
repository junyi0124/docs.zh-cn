---
title: 为 Windows Communication Foundation 配置 Internet Information Services 7.0
ms.date: 03/30/2017
ms.assetid: 1050d395-092e-44d3-b4ba-66be3b039ffb
ms.openlocfilehash: 865e19d6626846481347274774d3ea59f2f7ecdd
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96284207"
---
# <a name="configuring-internet-information-services-70-for-windows-communication-foundation"></a>为 Windows Communication Foundation 配置 Internet Information Services 7.0

Internet Information Services (IIS) 7.0 具有模块化设计，允许有选择地安装需要的组件。 此设计基于 Windows Vista 中引入的新的清单驱动组件化技术。 IIS 7.0 的40多个独立功能组件可以独立安装。 这使 IT 专业人士能够轻松地按需要自定义安装。 本主题讨论如何配置 IIS 7.0 以与 Windows Communication Foundation (WCF) 一起使用，并确定所需的组件。

## <a name="minimal-installation-installing-was"></a>最小安装：安装 WAS

 整个 IIS 7.0 包的最小安装是安装) 的 Windows 进程激活服务 (。 WAS 是一项独立功能，并且是 IIS 7.0 中唯一适用于所有 Windows Vista 操作系统的功能， (Home Basic、Home Premium、Business 和旗舰版和企业版) 。

 在 "控制面板" 中，单击 "**程序**"，然后单击 "**程序和功能**" 下的 "**打开或关闭 Windows 功能**"，WAS 组件显示在列表中，如下图所示。

 ![打开或关闭功能对话框](media/wcfc-turnfeaturesonoroffs.gif "wcfc_TurnFeaturesOnOrOffs")

 此功能具有下列子组件：

- .NET 环境

- 配置 API

- 进程模型

 如果选择的根节点为，则默认情况下仅检查 " **进程模型** " 子节点。 请注意，使用此安装时，你将只安装 WAS，因为没有用于 Web 服务器的支持功能。

 若要使 WCF 或任何 ASP.NET 应用程序正常工作，请选中 " **.Net 环境** " 复选框。 这意味着，所有 WAS 组件都需要使 WCF 和 ASP.NET 能够正常运行。 您安装其中任何组件后，会自动选中这些组件。

## <a name="iis-70-default-installation"></a>IIS 7.0：默认安装

 通过检查 **Internet Information Services** 功能，会自动检查某些子节点，如下图所示。

 ![IIS 7.0 功能的默认设置](media/wcfc-turningfeaturesonoroff2.gif "wcfc_TurningFeaturesOnOrOff2")

 这是 IIS 7.0 的默认安装。 使用此安装，你可以使用 IIS 7.0 来为静态内容提供服务 (如 HTML 页面和其他内容) 。 但是，不能运行 ASP.NET 或 CGI 应用程序，也不能托管 WCF 服务。

## <a name="iis-70-installation-with-aspnet-support"></a>IIS 7.0：带 ASP.NET 支持的安装

 必须安装 ASP.NET 才能在 IIS 7.0 上进行 ASP.NET 工作。 检查 **ASP.NET** 后，屏幕应如下图所示。

 ![Asp.NET 需要的设置](media/wcfc-trunfeaturesonoroff3s.gif "wcfc_TrunFeaturesOnOrOFf3s")

 这是 WCF 和 ASP.NET 应用程序在 IIS 7.0 中工作的最小环境。

## <a name="iis-70-installation-with-iis-60-compatibility-components"></a>IIS 7.0：带 IIS 6.0 兼容组件的安装

 当使用 Visual Studio 2005 或其他一些自动化脚本或工具在系统上安装 IIS 7.0 时 (例如 Adsutil.vbs) 配置使用 IIS 6.0 元数据库 API 的虚拟应用程序时，请确保检查 IIS 6.0 **脚本编写工具**。 这会自动检查 IIS 6.0 **管理兼容性** 的其他子节点。 下图显示了完成此操作后的屏幕：

 ![IIS 6.0 管理兼容性设置](media/scfc-turnfeaturesonoroff5s.gif "scfc_TurnFeaturesOnOrOff5s")

 通过此安装，你可以使用 Web 上提供的 IIS 7.0、ASP.NET 和 WCF 功能和示例所需的一切。

## <a name="request-limits"></a>请求限制

 在带有 IIS 7 的 Windows Vista 上，和设置的默认值已 `maxUri` `maxQueryStringSize` 更改。 默认情况下，IIS 7.0 中的请求筛选功能允许 URL 的长度为 4096 个字符，查询字符串的长度为 2048 个字符。 更改这些默认值，可将以下 XML 添加到 App.config 文件中。

```xml
 <system.webServer>
    <security>
        <requestFiltering>
            <requestLimits maxUrl="8192" maxQueryString="8192" />
        </requestFiltering>
    </security>
 </system.webServer>
 ```

## <a name="see-also"></a>另请参阅

- [WAS 激活体系结构](was-activation-architecture.md)
- [配置 WAS 以用于 WCF](configuring-the-wpa--service-for-use-with-wcf.md)
- [如何：安装和配置 WCF 激活组件](how-to-install-and-configure-wcf-activation-components.md)
- [Windows Server App Fabric 承载功能](/previous-versions/appfabric/ee677189(v=azure.10))
