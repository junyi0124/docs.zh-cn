---
title: 标识
description: 构建适用于 Azure 的云本机 .NET 应用 |标识
ms.date: 05/13/2020
ms.openlocfilehash: 66ff29947093d7c4fe57b11039190836dc37db08
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91163747"
---
# <a name="identity"></a>标识

大多数软件应用程序都需要对调用它们的用户或进程有一定的了解。 与应用程序交互的用户或进程称为 "安全主体"，并对这些主体进行身份验证和授权的过程称为 "标识管理" 或简称为 " *标识*"。 简单的应用程序可能会在应用程序中包含它们的所有标识管理，但这种方法并不能很好地适应许多应用程序以及许多类型的安全主体。 Windows 支持使用 Active Directory 来提供集中式身份验证和授权。

<!-- (insert figure showing Windows AD auth model) -->

虽然此解决方案在企业网络中有效，但它不是由 AD 域之外的用户或应用程序使用的。 随着基于 Internet 的应用程序和云本机应用程序的增长，安全模式也在不断发展。

在当今的云本机标识模型中，体系结构假定为已分发。 应用可在任何位置进行部署，并且可以在任何位置与其他应用进行通信。 客户端可以从任何位置与这些应用程序进行通信，实际上，客户端可能包含平台和设备的任意组合。 云本机标识解决方案利用开放标准来实现客户端的安全应用程序访问。 这些客户端范围从电脑或手机上的用户用户，到在世界各地托管的其他应用程序，再到在世界各地运行任何软件平台的机顶盒和 IOT 设备。

新式云本机标识解决方案通常利用由安全令牌服务/服务器 (STS) 在确定其标识后颁发给安全主体的访问令牌。  (JWT) 的访问令牌（通常为 JSON Web 令牌）包含有关安全主体的 *声明* 。 这些声明将最少包括用户的标识，但也可能包括其他声明，应用程序可以使用这些声明来确定授予主体的访问权限级别。

<!-- (insert figure showing basic handshake involving a principal, an STS, and an app) -->

通常，STS 只负责对主体进行身份验证。 确定其对资源的访问级别是否留给了应用程序的其他部分。

## <a name="references"></a>参考

- [Microsoft 标识平台](/azure/active-directory/develop/)

>[!div class="step-by-step"]
>[上一页](azure-monitor.md)
>[下一页](authentication-authorization.md)
