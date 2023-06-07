---
title: .NET Standard 中的新增功能
description: 本文总结了所有新版 .NET Standard 中的新功能和增强功能。
ms.custom: updateeachrelease
ms.date: 04/12/2018
ms.prod: dotnet-whatsnew
ms.openlocfilehash: 299477a7375381fa7f8064562e2a68e221944a05
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94817169"
---
# <a name="whats-new-in-net-standard"></a>.NET Standard 中的新增功能

.NET Standard 是一种正式规范，它定义了一组版本化 API。这些 API 必须可用于符合相应 Standard 版本要求的 .NET 实现。 .NET Standard 面向库开发者。 定目标到 .NET Standard 版本的库可用于任意 .NET Framework、.NET Core 或支持 Standard 版本的 Xamarin 实现。

.NET Core SDK 及选择 .NET Core 工作负载时的 Visual Studio 包含 .NET Standard。

## <a name="supported-net-implementations"></a>支持的 .NET 实现

以下 .NET 实现支持 .NET Standard 2.0：

- .NET Core 2.0 或更高版本
- .NET Framework 4.6.1 或更高版本
- Mono 5.4 或更高版本
- Xamarin.iOS 10.14 或更高版本
- Xamarin.Mac 3.8 或更高版本
- Xamarin.Android 8.0 或更高版本
- 通用 Windows 平台 10.0.16299 或更高版本

## <a name="whats-new-in-net-standard-20"></a>.NET Standard 2.0 中的新增功能

.NET Standard 2.0 新增了以下功能：

### <a name="a-vastly-expanded-set-of-apis"></a>大幅扩展了 API 集

.NET Standard 版本 1.6 中包含了相对较小的一部分 API。 不包含的 API 许多都是 .NET Framework 或 Xamarin 中的常用 API。 这样一来，开发变得更为棘手，因为开发人员必须在开发定目标到多个 .NET 实现的应用和库时，寻找常用 API 的合适替代项。 为了消除此限制，.NET Standard 2.0 向 Standard 旧版本 .NET Standard 1.6 中的可用 API 补充了 20,000 多个 API。 有关添加到 .NET Standard 2.0 的 API 列表，请参阅 [.NET Standard 2.0 与 1.6](https://raw.githubusercontent.com/dotnet/standard/master/docs/versions/netstandard2.0_diff.md)。

.NET Standard 2.0 的 <xref:System> 命名空间中新增的一些功能包括：

- 支持 <xref:System.AppDomain> 类。
- 更好地支持通过 <xref:System.Array> 类中的附加成员处理数组。
- 更好地支持通过 <xref:System.Attribute> 类中的附加成员处理属性。
- 改进了日历支持，并附加了 <xref:System.DateTime> 值的格式设置选项。
- 附加了 <xref:System.Decimal> 舍入功能。
- 在 <xref:System.Environment> 类中附加了功能。
- 增强了通过 <xref:System.GC> 类控制垃圾回收器。
- 增强了 <xref:System.String> 类中的字符串比较、枚举和规范化支持。
- <xref:System.TimeZoneInfo.AdjustmentRule> 和 <xref:System.TimeZoneInfo.TransitionTime> 类支持夏令时调整和时间转换。
- 显著改进了 <xref:System.Type> 类中的功能。
- 通过添加包含 <xref:System.Runtime.Serialization.SerializationInfo> 和 <xref:System.Runtime.Serialization.StreamingContext> 参数的异常构造函数，改进了对异常对象反序列化的支持。

### <a name="support-for-net-framework-libraries"></a>支持 .NET Framework 库

许多库定目标到 .NET Framework，而不是 .NET Standard。 不过，这些库大多调用的是 .NET Standard 2.0 中的 API。 自 .NET Standard 2.0 起，可以使用[兼容性填充码](https://github.com/dotnet/standard/blob/master/docs/planning/netstandard-2.0/README.md#assembly-unification)从 .NET Standard 库访问 .NET Framework 库。 此兼容性层对开发人员透明；无需执行任何操作，即可使用 .NET Framework 库。

只有一项要求就是，.NET Framework 类库调用的 API 必须是 .NET Standard 2.0 中的 API。

### <a name="support-for-visual-basic"></a>支持 Visual Basic

现在可以使用 Visual Basic 开发 .NET Standard 库。 安装了 .NET Core 工作负载的 Visual Studio 2019 和 Visual Studio 2017 版本 15.3 或更高版本包含 .NET Standard 类库模板。 对于使用其他开发工具和环境的 Visual Basic 开发人员，可以使用 [dotnet new](../../core/tools/dotnet-new.md) 命令创建 .NET Standard 库项目。 有关详细信息，请参阅 [.NET Standard 库的工具支持](#tooling-support-for-net-standard-libraries)。

### <a name="tooling-support-for-net-standard-libraries"></a>.NET Standard 库的工具支持

随着 .NET Core 2.0 和 .NET Standard 2.0 发布，Visual Studio 2017 和 [.NET Core CLI](../../core/tools/index.md) 均包含创建 .NET Standard 库所需的工具支持。

如果安装含 .NET Core 跨平台开发工作负荷  的 Visual Studio，可以使用项目模板创建 .NET Standard 2.0 库项目，如下图所示：

<!-- markdownlint-disable MD025 -->

# <a name="c"></a>[C#](#tab/csharp)

![添加新的 .NET Standard 库项目](./media/std-project-cs.png)

如果使用的是 .NET Core CLI，可以运行下面的 [dotnet new](../../core/tools/dotnet-new.md) 命令，以创建以 .NET Standard 2.0 为目标的类库项目：

```dotnetcli
dotnet new classlib
```

# <a name="visual-basic"></a>[Visual Basic](#tab/vb)

![添加新的 .NET Standard 库项目](./media/std-project-vb.png)

如果使用的是 .NET Core CLI，可以运行下面的 [dotnet new](../../core/tools/dotnet-new.md) 命令，以创建以 .NET Standard 2.0 为目标的类库项目：

```dotnetcli
dotnet new classlib -lang vb
```

---

## <a name="see-also"></a>请参阅

- [.NET Standard](../net-standard.md)
- [.NET Standard 简介](https://devblogs.microsoft.com/dotnet/introducing-net-standard/)
- [下载 .NET SDK](https://dotnet.microsoft.com/download)
