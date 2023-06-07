---
title: 将 ASP.NET Web 应用迁移到 Azure VM
description: 了解如何将 ASP.NET Web 应用程序从本地迁移到 Azure 虚拟机。
ms.topic: how-to
ms.date: 06/20/2020
ms.openlocfilehash: 940243310c5e6ed13d2a42c8d9d87244200479f5
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91171554"
---
# <a name="migrate-an-aspnet-web-application-to-an-azure-virtual-machine"></a>将 ASP.NET Web 应用程序迁移到 Azure 虚拟机

本文档概述了解如何将 ASP.NET Web 应用程序从本地迁移到 Azure 虚拟机。

## <a name="quickstart"></a>快速入门

了解如何创建虚拟机并将应用发布到其中：[发布到 Azure VM](https://tutorials.visualstudio.com/aspnet-vm/intro)

## <a name="get-started"></a>开始操作

这些教程演示了创建（或迁移）虚拟机、将 Web 应用程序发布到该虚拟机的步骤，以及在 Azure 中支持应用程序所要执行的其他任务。

- 使用以下选项之一在 Azure 中为 ASP.NET 应用程序创建虚拟机：
  - [为 ASP.NET 应用程序创建新的虚拟机](https://go.microsoft.com/fwlink/?linkid=863237)
  - [迁移现有的本地 VMWare 虚拟机](/azure/migrate/tutorial-migrate-vmware)
  - [迁移现有的本地 Hyper-V 虚拟机](/azure/migrate/tutorial-migrate-hyper-v)
- [使用 Visual Studio 发布应用](https://go.microsoft.com/fwlink/?linkid=863240)
- [为 VM 创建安全虚拟网络](/azure/virtual-network/virtual-network-get-started-vnet-subnet)
- [为应用程序创建 CI/CD 管道](/vsts/build-release/apps/cd/deploy-webdeploy-iis-deploygroups)
- [转移到 VM 规模集以实现高可用性和可伸缩性](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-deploy-app)

## <a name="considerations"></a>注意事项

### <a name="benefits"></a>优点

虚拟机提供将应用程序从本地迁移到云的最简单路径。 这样就可以复制应用程序在本地使用的相同环境，同时无需维护自己的数据中心。 虚拟机规模集为虚拟机中运行的应用程序提供高可用性和可伸缩性。

### <a name="virtual-machine-size"></a>虚拟机大小

选择最大程度地针对工作负荷进行优化的虚拟机大小和类型。 有关详细信息，请参阅 [Azure 中的 Windows 虚拟机大小](/azure/virtual-machines/windows/sizes)。

### <a name="maintenance"></a>维护

就像在本地计算机上一样，你需要负责维护和更新虚拟机 <sup>&#42;</sup>。 如果应用程序可以在 [Azure 应用服务](/azure/app-service/)等平台即服务 (PaaS) 环境或者在[容器](/azure/app-service/containers/)中运行，则不需要执行维护和更新。

*<sup>&#42;</sup>[虚拟机规模集的自动 OS 升级](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade)目前以预览服务提供。*

### <a name="virtual-networks"></a>虚拟网络

使用 Azure 虚拟网络可以：

- 构建可控的混合基础结构
- 自带 IP 地址和 DNS 服务器
- 为应用程序创建独立且高度安全的环境
- 使用多个[连接选项](/azure/vpn-gateway/vpn-gateway-about-vpngateways#s2smulti)之一将 VM 连接到本地网络
- 使用 [ExpressRoute](https://azure.microsoft.com/services/expressroute/) 将虚拟机集成到本地网络

若要开始，请参阅[虚拟网络文档](/azure/virtual-network/)

### <a name="active-directory"></a>Active Directory

许多应用程序使用 Active Directory 进行身份验证和标识管理。

- 使用 Azure AD Connect 可将本地目录与 Azure Active Directory 集成。 若要开始集成，请参阅[将本地目录与 Azure Active Directory 集成](/azure/active-directory/connect/active-directory-aadconnect)。
- 或者，可以让应用程序使用 [ExpressRoute](https://azure.microsoft.com/services/expressroute/) 访问本地 Active Directory。

### <a name="sql-databases"></a>SQL 数据库

如果应用程序使用本地数据库，默认情况下，应用无法与该数据库通信。 可以：

- 配置一个混合网络来让应用程序访问本地运行的数据库。
- 将数据库迁移到 Azure 有关详细信息，请参阅[将 SQL Server 数据库迁移到 Azure](sql.md)。

### <a name="high-availability-and-scalability"></a>高可用性和可伸缩性

#### <a name="virtual-machine-scale-sets"></a>虚拟机规模集

如果想要确保应用程序具有高可用性并可缩放，可将 VM 映像迁移到 Azure 虚拟机规模集，以提高应用程序的可用性和可伸缩性。 借助 VM 规模集，可以使用已配置的现有 VM 或设置一个生成管道来生成包含应用程序的映像。

若要开始，请参阅[在虚拟机规模集上部署应用程序](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-deploy-app)。

#### <a name="centralized-logging"></a>集中式日志记录

跨多个实例运行应用程序时，请考虑将日志存储在某个中心位置，例如 [Azure 存储](/azure/storage/)。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [将 SQL Server 数据库迁移到 Azure](sql.md)
