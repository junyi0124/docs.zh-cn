---
title: Xamarin 限制
ms.date: 12/13/2019
description: 描述使用 Xamarin 时可能会遇到的一些限制。
ms.openlocfilehash: 192f25954726919dc66d706e755e0853404b4d85
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75450404"
---
# <a name="xamarin-limitations"></a>Xamarin 限制

Microsoft.Data.Sqlite 以 .NET Standard 2.0 为目标，并且支持 Xamarin。 下表显示默认 SQLitePCLRaw 捆绑为其提供本机 SQLite 二进制文件的平台。 请参阅[自定义 SQLite 版本](custom-versions.md)，详细了解如何使用不同的捆绑，或提供自己的本机 SQLite 二进制文件。

| Platform | SQLite 二进制文件 |
| --- | --- |
| **Xamarin.Android** | — |
| &nbsp;&nbsp;&nbsp;&nbsp;`arm64-v8a` | ✔ |
| &nbsp;&nbsp;&nbsp;&nbsp;`armeabi-v7a` | ✔ |
| &nbsp;&nbsp;&nbsp;&nbsp;`x86` | ✔ |
| &nbsp;&nbsp;&nbsp;&nbsp;`x86_64` | ✔ |
| **Xamarin.iOS** | ✔ |
| **Xamarin.Mac** | ✔ |
| **Xamarin.TVOS** | ✔ |
| **UWP** | — |
| &nbsp;&nbsp;&nbsp;&nbsp;`arm` | ✔ |
| &nbsp;&nbsp;&nbsp;&nbsp;`arm64` | ✔ |
| &nbsp;&nbsp;&nbsp;&nbsp;`x64` | ✔ |
| &nbsp;&nbsp;&nbsp;&nbsp;`x86` | ✔ |

## <a name="ios"></a>iOS

Microsoft.Data.Sqlite 尝试自动初始化 SQLitePCLRaw 捆绑。 遗憾的是，由于针对 Xamarin.iOS 的预先 (AOT) 编译存在限制，因此尝试失败，并出现以下错误。

> 需要调用 `SQLitePCL.raw.SetProvider()`。 如果使用的是捆绑包，可以通过调用 `SQLitePCL.Batteries.Init()` 来完成此操作。

若要初始化该绑定，请在使用 Microsoft.Data.Sqlite 之前，将以下代码行添加到应用。

```csharp
SQLitePCL.Batteries_V2.Init();
```

## <a name="see-also"></a>请参阅

* [异步限制](async.md)
* [自定义 SQLite 版本](custom-versions.md)
