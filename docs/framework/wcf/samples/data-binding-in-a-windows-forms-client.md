---
title: Windows 窗体客户端中的数据绑定
ms.date: 03/30/2017
ms.assetid: a2a30b37-d6e2-4552-820e-e60b2bbe8829
ms.openlocfilehash: 07af2f2e604c8eab7eca9908e0985fefee5b273f
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96289446"
---
# <a name="data-binding-in-a-windows-forms-client"></a>Windows 窗体客户端中的数据绑定

此示例演示如何绑定到 Windows 窗体应用程序中 Windows Communication Foundation (WCF) 服务返回的数据。  
  
> [!NOTE]
> 本文的最后介绍了此示例的设置过程和生成说明。  
  
 本示例演示一个服务，该服务可实现定义“请求-答复”通信模式的协定。 此示例由一个客户端 Windows 窗体应用程序 ( .exe) 和 Internet Information Services (IIS) 承载的 WCF 服务组成。  
  
 协定由 `IWeatherService` 接口定义，该接口公开一个名为 `GetWeatherData` 的操作。 此操作接受一个城市数组并返回一个 `WeatherData` 对象数组，这些对象表示城市的预报高温和预报低温。  
  
 在 Windows 窗体应用程序中的客户端上进行数据绑定。 在 Windows 窗体设计器中定义一个 `DataGridView`（它是数据的图形化表示形式）。 还会创建一个名为 `BindingSource` 的中间媒介。 将 `BindingSource` 的数据源设置为由服务返回的数据数组。 `BindingSource` 的用途是提供数据与数据视图之间的间接层。 与数据的所有交互（如导航、排序、筛选和更新）都是通过调用 `BindingSource` 组件来完成的。 若要完成对 `DataGridView` 的数据绑定，请将 `datasource` 的 `DataGridView` 设置为 `BindingSource` 对象。 然后，从 WCF 服务返回的所有数据都以图形方式显示给用户。  每当用户单击按钮时，会在数据绑定的 `DataGridView` 中自动更新返回的数据。  
  
### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。  
  
3. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Scenario\DataBinding\WindowsForms`  
