---
title: ServiceModel 注册工具 (ServiceModelReg.exe)
description: 如果在服务激活过程中遇到问题，请使用此命令行工具来管理在一台计算机上注册 WCF 和 WF 组件。
ms.date: 03/30/2017
ms.assetid: 396ec5ae-e34f-4c64-a164-fcf50e86b6ac
ms.openlocfilehash: d8d5bc4dc3de021e2110fb953dbc4d6c7d7972d0
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96235917"
---
# <a name="servicemodel-registration-tool-servicemodelregexe"></a>ServiceModel 注册工具 (ServiceModelReg.exe)

利用此命令行工具可以管理 WCF 和 WF 组件在单一计算机上的注册。 在正常情况下，不应使用此工具，因为 WCF 和WF 组件在安装时配置。 但如果您遇到服务激活方面的问题，可以尝试使用此工具注册组件。  
  
## <a name="syntax"></a>语法  
  
```console  
ServiceModelReg.exe[(-ia|-ua|-r)|((-i|-u) -c:<command>)] [-v|-q] [-nologo] [-?]  
```  
  
## <a name="remarks"></a>备注  

 可在以下位置中找到此工具：  
  
 %SystemRoot%\Microsoft.Net\Framework\v3.0\Windows Communication Foundation\  
  
> [!NOTE]
> 当在 Windows Vista 上运行 "带中注册" 工具时，" **Windows 功能**" 对话框可能不会反映 Windows Communication Foundation 启用 **Microsoft .NET FRAMEWORK 3.0** 的 **HTTP 激活** 选项。 可以通过单击 "**开始**"，然后单击 "**运行**"，然后键入 " **OptionalFeatures**" 来访问 " **Windows 功能**" 对话框。  
  
 下表介绍可用于 ServiceModelReg.exe 的选项。  
  
|选项|描述|  
|------------|-----------------|  
|`-ia`|安装所有 WCF 和 WF 组件。|  
|`-ua`|卸载所有 WCF 和 WF 组件。|  
|`-r`|修复所有 WCF 和 WF 组件。|  
|`-i`|安装使用 –c 指定的 WCF 和 WF 组件。|  
|`-u`|卸载使用 –c 指定的 WCF 和 WF 组件。|  
|`-c`|安装或卸载组件：<br /><br /> -httpnamespace – HTTP 命名空间保留<br />-tcpportsharing – TCP 端口共享服务<br />-tcpactivation – .NET 4 客户端配置文件中不支持的 TCP 激活服务 () <br />-namedpipeactivation – .NET 4 客户端配置文件不支持命名管道激活服务 (<br />-msmqactivation – .NET 4 客户端配置文件中不支持 MSMQ 激活服务 (<br />-etw-ETW 事件跟踪清单 (Windows Vista 或更高版本) |  
|`-q`|安静模式（仅显示错误日志记录）|  
|`-v`|详细模式。|  
|`-nologo`|取消版权和标题消息。|  
|`-?`|显示帮助文本|  
  
## <a name="fixing-the-fileloadexception-error"></a>修复 FileLoadException 错误  

 如果在计算机上安装了早期版本的 WCF，则在 `FileLoadFoundException` 运行 servicemodelreg.exe 工具注册新安装时，可能会收到错误。 即使从以前的安装中手动移除了文件（但保留 machine.config 设置不变），也可能发生这种情况。  
  
 错误消息类似于以下内容。  
  
```console  
Error: System.IO.FileLoadException: Could not load file or assembly 'System.ServiceModel, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference. (Exception from HRESULT: 0x80131040)  
File name: 'System.ServiceModel, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089'  
```  
  
 从错误消息中，您应注意到早期的客户技术预览 (CTP) 版本安装了 System.ServiceModel 2.0.0.0 版程序集。 而已发布 System.ServiceModel 程序集的当前版本为 3.0.0.0。 因此，当你想要在安装了 WCF 的早期 CTP 版本但未完全卸载的计算机上安装官方 WCF 版本时，会遇到此问题。  
  
 ServiceModelReg.exe 无法清除以前的版本项，也无法注册新的版本项。 唯一的解决方法是手动编辑 machine.config。您可以在以下位置找到此文件。  
  
```console  
%windir%\Microsoft.NET\Framework\v2.0.50727\config\machine.config
```  
  
 如果在64位计算机上运行 WCF，还应在此位置编辑同一文件。  
  
```console  
%windir%\Microsoft.NET\Framework64\v2.0.50727\config\machine.config
```  
  
 在此文件中找到引用 "System.servicemodel，Version = 2.0.0.0" 的任何 XML 节点，删除它们和任何子节点。 保存文件并重新运行 ServiceModelReg.exe 可解决此问题。  
  
## <a name="examples"></a>示例  

 下面的示例演示如何使用 ServiceModelReg.exe 工具的最常用选项。  
  
```console  
ServiceModelReg.exe -ia  
  Installs all components  
ServiceModelReg.exe -i -c:httpnamespace -c:etw  
  Installs HTTP namespace reservation and ETW manifests  
ServiceModelReg.exe -u -c:etw  
  Uninstalls ETW manifests  
ServiceModelReg.exe -r  
  Repairs an extended install  
```
