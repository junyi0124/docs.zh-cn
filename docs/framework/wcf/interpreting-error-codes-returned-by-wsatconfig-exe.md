---
title: 解释 wsatConfig.exe 返回的错误代码
ms.date: 03/30/2017
ms.assetid: ab65f22b-0d69-4c21-9aaf-74acef0ca102
ms.openlocfilehash: c5f423f5054a3a80bc0c730444ca9e90c203e288
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96262744"
---
# <a name="interpreting-error-codes-returned-by-wsatconfigexe"></a>解释 wsatConfig.exe 返回的错误代码

本主题列出了 WS-AtomicTransaction 配置实用工具 (wsatConfig.exe) 生成的所有错误代码，以及建议采取的操作。  
  
## <a name="list-of-error-codes"></a>错误代码列表  
  
|错误代码|说明|建议采取的操作|  
|----------------|-----------------|------------------------------------|  
|0|操作成功|无|  
|1|错误|联系 Microsoft|  
|2|尝试联系 MSDTC 以检索其安全设置时发生错误。|确保 MSDTC 服务未禁用，并解决返回的异常中列出的所有问题。|  
|3|运行 WsatConfig.exe 时所使用的帐户没有足够的权限读取网络安全设置。|使用管理员用户帐户执行 WsatConfig.exe。|  
|4|在尝试启用 WS-AT 支持之前为 MSDTC 启用“网络 DTC 访问”。|为 MSDTC 启用“网络 DTC 访问”，并重新运行实用工具。|  
|5|输入的端口超出范围。 值必须在 1 到 65535 的范围内。|按照错误消息中的指示，更正 `-port:<portNum>`<br /><br /> 命令行选项。|  
|6|在命令行上指定了无效的终结点证书。  找不到证书，或证书未通过验证。|更正 `-endpointCert` 命令行选项。 确保证书有私钥、可同时用于 ClientAuthentication 和 ServerAuthentication、安装在 LocalMachine\MY 证书存储区中，并且完全受信任。|  
|7|在命令行上指定了无效的帐户证书。|更正 `-accountsCerts` 命令行选项。 指定的证书要么未正确指定，要么无法找到。|  
|8|指定的默认超时超出 1 到 3600 秒的范围。|按照指示输入正确的默认超时值。|  
|10|尝试访问注册表时发生错误。|检查错误消息和错误代码中的可操作项|  
|11|无法写入注册表。|确保错误消息中列出的键能够支持通过执行 WsatConfig.exe 所使用的帐户访问注册表。|  
|12|尝试访问证书存储区时发生错误。|使用返回的错误代码映射到相应的系统错误。|  
|13|http.sys 配置失败。 无法为 MSDTC 创建新的 HTTPS 端口预留。|使用返回的错误代码映射到相应的系统错误。|  
|14|http.sys 配置失败。 无法为 MSDTC 移除以前的 HTTPS 端口预留。|使用返回的错误代码映射到相应的系统错误。|  
|15|http.sys 配置失败。 以前的 HTTPS 端口预留对于指定的端口已存在。|另一个应用程序已获得特定端口的所有权。 更改为其他端口，或者卸载或重新配置当前应用程序。|  
|16|http.sys 配置失败。 无法将指定证书绑定到端口。|使用错误消息中返回的错误代码映射到相应的系统错误|  
|17|http.sys 配置失败。 无法取消 SSL 证书与以前的端口的绑定。|使用错误消息中返回的错误代码映射到相应的系统错误。 如有必要，使用 httpcfg.exe 或 netsh.exe 移除错误的端口预留。|  
|18|http.sys 配置失败。 由于以前的 SSL 绑定已存在，因此无法将指定的证书绑定到端口。|另一个应用程序已获得特定端口的所有权。 更改为其他端口，或者卸载或重新配置当前应用程序。|  
|19|重新启动 MSDTC 失败|如有必要，请手动重新启动 MSDTC。 如果问题仍然存在，请与 Microsoft 联系。|  
|20|远程计算机上未安装 WinFX，或者安装不正确。|在计算机上安装 WinFX。|  
|21|远程配置由于操作超时而失败。|用于在远程计算机上配置 WS-AT 的调用所花时间超过 90 秒。|  
|22|远程计算机上未安装 WinFX，或者安装不正确。|在计算机上安装 WinFX。|  
|23|远程配置由于远程计算机上出现异常而失败。|检查错误消息中的可操作项|  
|26|传递到 WsatConfig.exe 的自变量无效。|检查命令行中的错误。|  
|27|`-accounts` 命令行选项无效。|更正 -`accounts` 命令行选项以正确地指定用户帐户。|  
|28|`-network` 命令行选项无效。|更正 `-network` 命令行选项以正确地指定“enable”或“disable”。|  
|29|`-maxTimeout` 命令行选项无效。|请按照指示更正 `-maxTimeout` 命令行选项。|  
|30|`-timeout` 命令行选项无效。|请按照指示更正 `-timeout` 命令行选项。|  
|31|`-traceLevel` 命令行选项无效。|更正 `-traceLevel` 命令行选项以指定以下其中一个有效值，<br /><br /> -关闭<br />-   Error<br />-   严重<br />-   Warning<br />-信息<br />-Verbose<br />-所有|  
|32|`-traceActivity` 命令行选项无效。|通过指定“enable”或“disable”，更正 `-traceActivity` 命令行选项。|  
|33|`-traceProp` 命令行选项无效。|通过指定“enable”或“disable”，更正 `-traceProp` 命令行选项。|  
|34|`-tracePII` 命令行选项无效。|通过指定“enable”或“disable”，更正 `-tracePII` 命令行选项。|  
|37|WsatConfig.exe 无法确定准确的计算机证书。 如果有多个证书或者不存在任何证书，则可能会发生此情况。|指定证书指纹或“颁发者\主题名称”对，以正确地标识要配置的准确证书。|  
|38|进程或用户没有足够的权限更改防火墙配置。|使用管理员用户帐户执行 WsatConfig.exe。|  
|39|WsatConfig.exe 在更新防火墙配置时遇到了错误。|检查错误消息中的可操作项。|  
|40|WsatConfig.exe 无法授予 MSDTC 对证书私钥文件的读取访问权限|使用管理员用户帐户执行 WsatConfig.exe。|  
|41|找不到安装 WinFX，或找到的版本与工具可以配置的版本不匹配。|请确保正确安装了 WinFX 并仅使用该版本 WinFX 附带的 WsatConfig.exe 工具来配置 WS-AT。|  
|42|在命令行上多次指定了某个自变量。|在执行 WsatConfig.exe 时只指定每个自变量一次。|  
|43|如果未启用 WS-AT，则 WsatConfig.exe 无法更新 WS-AT 设置。|指定 `-network:enable` 作为附加命令行自变量。|  
|44|缺少必需的修补程序，在安装该修补程序之前将无法配置 WS-AT。|请参阅 WinFX 发行说明，了解有关安装所需修补程序的说明。|  
|45|`-virtualServer` 命令行选项无效。|通过指定要在其中进行配置的群集资源的网络名称，更正 `-virtualServer` 命令行选项。|  
|46|尝试启动 ETW 跟踪会话时发生错误|使用返回的错误代码映射到相应的系统错误。|  
|47|进程或用户没有足够的权限启用 ETW 跟踪会话。|使用管理员用户帐户执行 WsatConfig.exe。|  
|48|尝试启动 ETW 跟踪会话时发生错误。|请与 Microsoft 联系。|  
|49|由于 %systemdrive% 上的空间不足而无法创建新日志文件|确保 %systemdrive% 上有足够的空间来存储日志文件。|  
|51|尝试启动 ETW 跟踪会话时发生错误。|请与 Microsoft 联系。|  
|52|尝试启动 ETW 跟踪会话时发生错误。|请与 Microsoft 联系。|  
|53|备份以前的 ETW 会话日志文件不成功。|确保 %systemdrive% 上有足够的空间来存储日志文件和以前日志文件的备份（如果有）。 如有必要，请手动移除以前的日志文件。|  
|55|尝试启动 ETW 跟踪会话时发生错误。|请与 Microsoft 联系。|  
|56|尝试启动 ETW 跟踪会话时发生错误。|请与 Microsoft 联系。|  
  
## <a name="see-also"></a>另请参阅

- [WS-AtomicTransaction 配置实用工具 (wsatConfig.exe)](ws-atomictransaction-configuration-utility-wsatconfig-exe.md)
