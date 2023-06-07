---
ms.openlocfilehash: 8c05af045d2d9666b20f36e36c68cc853f906eae
ms.sourcegitcommit: bc9c63541c3dc756d48a7ce9d22b5583a18cf7fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/11/2020
ms.locfileid: "94506843"
---

安装 .NET 包时，可能会看到类似于 `signature verification failed for file 'repomd.xml' from repository 'packages-microsoft-com-prod'` 的错误。 一般而言，此错误表示 .NET 的包源正在通过更新的包版本进行更新，应稍后重试。 升级期间，包源的不可用时间不应超过 2 小时。 如果持续收到此错误超过 2 小时，请在 <https://github.com/dotnet/core/issues> 中提交问题。
