---
title: 如何：安装和配置 WCF 激活组件
description: 了解如何在 Windows Vista 上设置 Windows 进程激活服务 () 来托管不能通过 HTTP 进行通信的 WCF 服务。
ms.date: 03/30/2017
helpviewer_keywords:
- HTTP activation [WCF]
ms.assetid: 33a7054a-73ec-464d-83e5-b203aeded658
ms.openlocfilehash: 085a69421f0aa7b763bd2222820ced4b4a7e1c81
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90557861"
---
# <a name="how-to-install-and-configure-wcf-activation-components"></a>如何：安装和配置 WCF 激活组件

本主题介绍设置 Windows 进程激活服务所需的步骤，该服务在 Windows Vista 上 (也称为) ，以承载 Windows Communication Foundation (不通过 HTTP 网络协议进行通信的 WCF) 服务。 下面的部分略述此配置的步骤：

- 安装 (或确认 WCF 激活组件) 的安装。

- 配置 WAS 以支持非 HTTP 协议。 以下过程将 Windows Vista 配置为进行 TCP 激活。

安装和配置 WAS 之后，请参阅 [中的如何：在 WAS 中承载 Wcf 服务](how-to-host-a-wcf-service-in-was.md) ，了解如何创建 wcf 服务，该服务公开了采用 WAS 的非 HTTP 终结点。

## <a name="to-install-the-wcf-non-http-activation-components"></a>安装 WCF 非 HTTP 激活组件

1. 单击****“开始”按钮，然后单击****“控制面板”。

2. 单击“程序”****，然后单击“程序和功能”****。

3. 在 " **任务** " 菜单上，单击 " **打开或关闭 Windows 功能**"。

4. 找到 WinFX 节点，选择它，然后将其展开。

5. 选择 " **WCF 非 Http 激活组件** " 框并保存设置。

## <a name="to-configure-the-was-to-support-tcp-activation"></a>配置 WAS 以支持 TCP 激活

1. 若要支持 net.tcp 激活，必须首先将默认的网站绑定到一个 net.tcp 端口。 可以通过使用随 IIS 7.0 管理工具集一起安装 Appcmd.exe 来完成此操作。 在管理员级别命令提示符窗口中，运行以下命令。

    ```console
    %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site" -+bindings.[protocol='net.tcp',bindingInformation='808:*']
    ```

    > [!NOTE]
    > 此命令是单行文本。 此命令将 net.tcp 网站绑定添加到以任何主机名侦听 TCP 端口 808 的默认网站。

2. 尽管网站内的所有应用程序共享一个公共 net.tcp 绑定，但是每个应用程序可以单独启用 net.tcp 支持。 若要启用应用程序的 net.tcp，请从管理员级别命令提示符运行以下命令。

    ```console
    %windir%\system32\inetsrv\appcmd.exe set app
    "Default Web Site/<WCF Application>" /enabledProtocols:http,net.tcp
    ```

    > [!NOTE]
    > 此命令是单行文本。 此命令允许 \<*WCF Application*> 使用和访问/应用程序 `http://localhost/<WCF Application>` `net.tcp://localhost/<WCF Application>` 。

     移除为此示例添加的 net.tcp 网站绑定。

     为方便起见，在位于示例目录中名为 RemoveNetTcpSiteBinding.cmd 的批处理文件中实现以下两个步骤。

    1. 通过在管理员级别命令提示符窗口中运行以下命令，从启用的协议列表移除 net.tcp。

        ```console
        %windir%\system32\inetsrv\appcmd.exe set app
        "Default Web Site/servicemodelsamples<WCF Application>" " /enabledProtocols:http
        ```

        > [!NOTE]
        > 此命令是单行文本。

    2. 通过在提升的命令提示符窗口中运行以下命令移除 net.tcp 网站绑定：

        ```console
        %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site"
        --bindings.[protocol='net.tcp',bindingInformation='808:*']
        ```

        > [!NOTE]
        > 此命令是单行文本。

## <a name="to-remove-nettcp-from-the-list-of-enabled-protocols"></a>从启用的协议列表中移除 net.tcp

1. 若要从启用的协议列表中移除 net.tcp，请在管理员级别命令提示符窗口中运行以下命令。

    ```console
    %windir%\system32\inetsrv\appcmd.exe set app "Default Web Site/servicemodelsamples<WCF Application>" " /enabledProtocols:http
    ```

    > [!NOTE]
    > 此命令是单行文本。

## <a name="to-remove-the-nettcp-site-binding"></a>移除 net.tcp 网站绑定

1. 要移除 net.tcp 网站绑定，请在管理员级别命令提示窗口中运行以下命令。

    ```console
    %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site"
    -bindings.[protocol='net.tcp',bindingInformation='808:*']
    ```

    > [!NOTE]
    > 此命令是单行文本。

## <a name="see-also"></a>请参阅

- [TCP 激活](../samples/tcp-activation.md)
- [MSMQ 激活](../samples/msmq-activation.md)
- [NamedPipe 激活](../samples/namedpipe-activation.md)
- [Windows Server App Fabric 承载功能](/previous-versions/appfabric/ee677189(v=azure.10))
