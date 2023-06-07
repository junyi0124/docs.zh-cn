---
title: 发布 NuGet 包
description: 将 .NET 库发布到 NuGet 的最佳做法建议。
ms.date: 10/02/2018
ms.openlocfilehash: 089c660bc51252c6295858b1462ae59bde968564
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2020
ms.locfileid: "76744556"
---
# <a name="publishing-a-nuget-package"></a>发布 NuGet 包

从包存储库发布和使用 NuGet 包。 虽然 NuGet.org 是最广为人知和使用范围最广的存储库，但有很多平台可以发布 NuGet 包：

* [NuGet.org](https://www.nuget.org/)  是 NuGet 包的主要联机存储库。 NuGet.org 上的所有包是向所有用户公开提供的。 默认情况下，Visual Studio 将 NuGet.org 作为包源，并且对于许多开发人员来说，NuGet.org 是他们与之交互的唯一包存储库。 NuGet.org 是发布想要获得社区反馈的稳定包和预发行包的最佳位置。

* [MyGet](https://myget.org/)  是支持开源项目的自定义包源的存储库服务。 MyGet 公共自定义源是发布由你的 CI 服务创建的预发行包的理想位置。 MyGet 还提供用于商业目的的专用源。

* [本地源](/nuget/hosting-packages/local-feeds)使你能够将文件夹视为包存储库并使文件夹中的 `*.nupkg` 文件可由 NuGet 访问。 本地源可用于在将 NuGet 发布到 MyGet.org 前对 NuGet 包进行测试。

> [!NOTE]
> 上传包后，NuGet.org [不允许将其删除](/nuget/policies/deleting-packages)。 可以不列出包，使其不在 UI 中公开可见，但仍可在还原后下载 `*.nupkg`。 此外，nuget.org 不允许重复的包版本。 若要更正存在错误的 NuGet 包，必须取消列出不正确的包，递增版本号并发布新版本的包。

✔️ 务必将想要获取社区反馈的[稳定包和预发行包发布](/nuget/create-packages/publish-a-package)到 NuGet.org。

✔️ 请考虑将预发行包从持续集成版本发布到 MyGet 源。

✔️ 请考虑使用本地源或 MyGet 在开发环境中测试包。 检查包是否运行正常，然后将其发布到 NuGet.org。

## <a name="nugetorg-security"></a>NuGet.org 安全

不良因素不能访问 NuGet 帐户且不能上传库的恶意版本，这一点很重要。 NuGet.org 在发布包时提供双因素身份验证和电子邮件通知。 登录到“帐户设置”  页面上的 NuGet.org 后，启用这些功能。

![替换文字](./media/publish-nuget-package/nuget-2fa.png "NuGet 帐户安全")

✔️ 务必使用 Microsoft 帐户登录到 NuGet。

✔️ 务必启用双重身份验证来访问 NuGet。

✔️ 务必在发布包时启用电子邮件通知。

>[!div class="step-by-step"]
>[上一页](sourcelink.md)
>[下一页](versioning.md)
