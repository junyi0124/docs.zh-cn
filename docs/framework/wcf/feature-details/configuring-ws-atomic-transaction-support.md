---
title: 配置 WS-Atomic 事务支持
ms.date: 03/30/2017
helpviewer_keywords:
- WS-AT protocol [WCF], configuring WS-Atomic Transaction
ms.assetid: cb9f1c9c-1439-4172-b9bc-b01c3e09ac48
ms.openlocfilehash: 9c0e75d58fbcf61137ceae3fba9d8acfe3902171
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90556585"
---
# <a name="configure-ws-atomic-transaction-support"></a>配置 WS 原子事务支持

本主题介绍如何通过 WS-AT 配置实用工具来配置 WS-AtomicTransaction (WS-AT) 支持。

## <a name="use-the-ws-at-configuration-utility"></a>使用 WS-AT 配置实用工具

WS-AT 配置实用工具 (wsatConfig.exe) 用于配置 WS-AT 设置。 为了启用 WS-AT 协议服务，必须使用该配置实用工具为 WS-AT 配置 HTTPS 端口，将 X.509 证书绑定到该 HTTPS 端口，并通过指定证书主题名称或指纹来配置授权的合作伙伴证书。 通过该配置实用工具，还可以选择跟踪模式，设置默认的外发事务超时和最大传入事务超时。

使用“组件服务”管理控制台中的 Microsoft 管理控制台 (MMC) 属性页管理单元，或者在命令行窗口中，都可访问此工具的功能。 通过命令行窗口配置本地计算机上的 WS-AT 支持；使用 MMC 管理单元配置本地和远程计算机上的设置。

在 Windows SDK 安装位置“%WINDIR%\Microsoft.NET\Framework\v3.0\Windows Communication Foundation”中，可访问命令行窗口。

有关命令行工具的详细信息，请参阅 [Ws-atomictransaction 配置实用工具 ( # A0) ](../ws-atomictransaction-configuration-utility-wsatconfig-exe.md)。

如果运行的是 Windows XP 或 Windows Server 2003，则可以通过导航到 **"控制面板"/"管理工具"/"组件服务**"，右键单击 **我的电脑**，然后选择 " **属性**" 来访问 MMC 管理单元。 Microsoft 分布式事务处理协调器 (MSDTC) 也可以在这个位置配置。 可用于配置的选项分组在 " **ws-at** " 选项卡下。如果运行的是 Windows Vista 或 Windows Server 2008，则可以通过单击 " **开始** " 按钮，并 `dcomcnfg.exe` 在 " **搜索** " 框中输入来找到 MMC 管理单元。 打开 MMC 时，导航到 "我的 **Computer\Distributed Transaction 处理协调器 DTC** " 节点，右键单击并选择 " **属性**"。 可用于配置的选项分组在 " **ws-at** " 选项卡下。

有关管理单元的详细信息，请参阅 [Ws-atomictransaction 配置 MMC 管理单元](../ws-atomictransaction-configuration-mmc-snap-in.md)。

若要启用该工具的用户界面，必须首先注册 WsatUI.dll 文件，该文件位于以下路径下

%PROGRAMFILES%\Microsoft SDKs\Windows\v6.0\Bin

若要注册产品，请在“命令提示”窗口中执行以下命令：

`regasm.exe /codebase WsatUI.dll`

## <a name="enable-ws-at"></a>启用 WS-AT

若要使用端口 443 和带私钥的 X.509 证书（已安装在本地机器存储区中）在 MSDTC 内启用 WS-AT 协议服务，请使用 wsatConfig.exe 工具执行以下命令。

`WsatConfig.exe –network:enable –port:8443 –endpointCert:<machine|"Issuer\SubjectName"> -accountsCerts:<thumbprint|"Issuer\SubjectName"> -restart`

将相应的参数替换为与环境相关的值。

若要在 MSDTC 内禁用 WS-AT 协议服务，请使用 wsatConfig.exe 工具执行以下命令。

`WsatConfig.exe –network:disable -restart`

## <a name="configure-trust-between-two-machines"></a>在两台计算机之间配置信任

WS-AT 协议服务要求管理员显式授权各个帐户参与分布式事务。 如果您是两台计算机的管理员，可以按以下方式配置两台计算机以建立相互信任关系：在两台计算机之间交换正确的证书集，将这些证书安装到相应的证书存储区，然后使用 wsatConfig.exe 工具将每台计算机的证书添加到对方的授权参与者证书列表中。 若要在两台计算机之间使用 WS-AT 执行分布式事务，则必须执行此步骤。

下面的示例概述了在两台计算机 A 和 B 之间建立信任关系的步骤。

### <a name="create-and-export-certificates"></a>创建和导出证书

此过程需要使用 MMC“证书”管理单元。 打开“开始”/“运行”菜单，并在输入框中输入“mmc”，然后按“确定”可访问该管理单元。 然后，在 "**控制台**1" 窗口中，导航到 **"文件"/"添加/删除**" 管理单元，单击 "添加"，然后从 "**可用的独立管理单元**" 列表中选择 "**证书**"。 最后，选择要管理的 **计算机帐户** ，然后单击 **"确定"**。 " **证书** " 节点显示在管理单元控制台中。

必须拥有必需的证书才能建立信任。 若要了解如何在执行以下步骤之前创建和安装新证书，请参阅 [如何：在开发过程中在 WCF 中创建和安装临时客户端证书](/previous-versions/msp-n-p/ff650751(v=pandp.10))。

1. 在计算机 A 上，使用 MMC“证书”管理单元将现有证书 (certA) 导入 LocalMachine\MY（“个人”节点）和 LocalMachine\ROOT 存储区（“受信任的根证书颁发机构”节点）。 若要将证书导入到特定节点，请右键单击该节点，然后选择 " **所有任务/导入**"。

2. 在计算机 B 上，使用 MMC“证书”管理单元创建或获取带私钥的证书 certB，并将其导入 LocalMachine\MY（“个人”节点）和 LocalMachine\ROOT 存储区（“受信任的根证书颁发机构”节点）。

3. 如果 certA 的公钥还未导出到文件，则完成此操作。

4. 如果 certB 的公钥还未导出到文件，则完成此操作。

### <a name="establish-mutual-trust-between-machines"></a>在计算机之间建立相互信任

1. 在计算机 A 上，将 certB 的文件表示形式导入 LocalMachine\MY 和 LocalMachine\ROOT 存储区。 这表明计算机 A 信任 certB 与之通信。

2. 在计算机 B 上，将 certA 的文件表示形式导入 LocalMachine\MY 和 LocalMachine\ROOT 存储区。 这表示计算机 B 信任 certA 与之通信。

完成这些步骤之后，即在两台计算机之间建立了信任，这时，就可以将它们配置为使用 WS-AT 相互通信。

### <a name="configure-msdtc-to-use-certificates"></a>将 MSDTC 配置为使用证书

由于 WS-AT 协议服务既作为客户端又作为服务器，因此它必须侦听传入连接和发起传出连接。 因此，需要对 MSDTC 进行配置，以使它知道在与外部各方通信时要使用哪个证书，在接受传入通信时要授权哪个证书。

可以使用 MMC WS-AT 管理单元进行这一配置。 有关此工具的详细信息，请参阅 [Ws-atomictransaction 配置 MMC 管理单元](../ws-atomictransaction-configuration-mmc-snap-in.md) 主题。 下面的步骤介绍如何在运行 MSDTC 的两台计算机之间建立信任。

1. 配置计算机 A 的设置。 对于 "终结点证书"，请选择 certA。 对于 "授权的证书"，请选择 certB。

2. 配置计算机 B 的设置。 对于 "终结点证书"，请选择 certB。 对于 "授权的证书"，请选择 certA。

> [!NOTE]
> 当一台计算机向另一台计算机发送消息时，发送方尝试验证接收方证书的主题名称和接收方计算机的名称是否匹配。 如果不匹配，则证书验证失败，并且两台计算机无法通信。
>
> 对于加入域的计算机，该名称为完全限定域名。 默认情况下，工作站中的计算机名称就是计算机的 NetBIOS 名称。 但是，对于两台计算机之间所用的连接，如果存在域名系统 (DNS) 后缀，则名称中还可包含 DNS 后缀。
>
> 如果计算机的名称更改，例如，当工作组计算机加入域时，必须重新颁发证书或手动配置 DNS 后缀。

## <a name="security"></a>安全性

由于某些与 MSDTC 和 WS-AT 关联的设置分别存储在注册表的 HKLM\Software\Microsoft\MSDTC 和 HKLM\Software\Microsoft\WSAT 下面，因此，请确保这些注册表项得到了保护，只有管理员才能写入这些项。 在 "注册表编辑器" 工具中，右键单击要保护的项，然后选择 " **权限** " 以设置适当的访问控制。 对于系统的安全性和完整性而言，重要的项对于低权限用户只读至关重要。

部署 MSDTC 时，管理员必须确保任何 MSDTC 数据交换都是安全的。 在工作组部署中，应将事务基础结构与恶意用户隔离；而在群集部署中，应保护群集注册表。

## <a name="tracing"></a>跟踪

WS-AT 协议服务支持集成的事务特定跟踪，可通过使用 [Ws-atomictransaction 配置 MMC 管理单元](../ws-atomictransaction-configuration-mmc-snap-in.md) 工具启用和管理。 跟踪可包含指示以下内容的数据：特定事务的登记时间、事务达到其最终状态的时间、每个事务登记收到的结果。 可以使用 [服务跟踪查看器工具 ( # A0) ](../service-trace-viewer-tool-svctraceviewer-exe.md) 工具查看所有跟踪。

通过 ETW 跟踪会话，WS-AT 协议服务还支持集成 ServiceModel 跟踪。 这样，除了现有事务跟踪之外，可提供更多特定于通信的详细跟踪。  若要启用这些附加跟踪，请按照以下步骤操作

1. 打开 " **开始/运行** " 菜单，在 "输入" 框中键入 "regedit"，然后选择 **"确定**"。

2. 在 **注册表编辑器**中，导航到左侧窗格中的以下文件夹，Hkey_Local_Machine \software\microsoft\wsat\3.0\

3. 右键单击 `ServiceModelDiagnosticTracing` 右窗格中的值，然后选择 " **修改**"。

4. 在 " **值数据** 输入" 框中，输入以下有效值之一，以指定要启用的跟踪级别。

- 0：关闭

- 1：严重

- 3：错误。 此为默认值。

- 7：警告

- 15：信息

- 31：详细

## <a name="see-also"></a>请参阅

- [WS-AtomicTransaction 配置实用工具 (wsatConfig.exe)](../ws-atomictransaction-configuration-utility-wsatconfig-exe.md)
- [WS-AtomicTransaction 配置 MMC 管理单元](../ws-atomictransaction-configuration-mmc-snap-in.md)
