---
title: 重大更改和 .NET 库
description: 有关在创建 .NET 库时浏览重大更改的最佳做法建议。
ms.date: 10/02/2018
ms.openlocfilehash: 2cbd9e0a818b52aede6c9b1f60fdf52dcbd7b96f
ms.sourcegitcommit: 67ebdb695fd017d79d9f1f7f35d145042d5a37f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92223326"
---
# <a name="breaking-changes"></a>中断性变更

对于 .NET 库而言，在现有用户的稳定性与未来的创新之间找到平衡点非常重要。 库创建者倾向于重构和重新考虑代码，直到达到完美，但是使现有用户中断会造成负面影响，尤其是对于低级库。

## <a name="project-types-and-breaking-changes"></a>项目类型和重大更改

.NET 社区使用库的方式改变了重大更改对最终用户开发人员的影响。

- 低级和中级库  （如序列化程序、HTML 分析器、数据库对象关系映射程序或 Web 框架）受重大更改的影响最大。

  构建基块包由最终用户开发人员用于构建应用程序，并由其他库用作 NuGet 依赖项。 例如，你在构建应用程序并使用开放源代码客户端调用 Web 服务。 对客户端使用的依赖项的重大更新不是你可以解决的问题。 开放源代码客户端需要进行更改，你无法控制它。 你必须找到库的兼容版本或提交客户端库的修复程序并等待新版本。 最糟糕的情况是如果你要使用两个库，但它们依赖于第三个库的互相不兼容版本。

- 高级库  （如一套 UI 控件）对于重大更改不太敏感。

  高级库在最终用户应用程序中直接引用。 如果发生重大更改，则开发人员可以选择更新到最新版本，或者可以修改其应用程序以应对重大更改。

✔️ 请考虑将如何使用库。 重大更改会对使用它的应用程序和库有何影响？

✔️ 请在开发低级 .NET 库时尽量减少重大更改。

✔️ 请考虑将库的重大重写作为新 NuGet 包进行发布。

## <a name="types-of-breaking-changes"></a>重大更改的类型

重大更改划分为不同类别，其影响并不相同。

### <a name="source-breaking-change"></a>源代码重大更改

源代码重大更改不影响程序执行，但是会在下次重新编译应用程序时导致编译错误。 例如，新重载可能会在以前明确的方法调用中形成多义性，或是重命名的参数会中断使用命名参数的调用方。

```csharp
public class Task
{
    // Adding a type called Task could conflict with System.Threading.Tasks.Task at compilation
}
```

因为源代码重大更改仅在开发人员重新编译其应用程序时有害，所以它是破坏性最小的重大更改。 开发人员可以轻松地修复自己的中断源代码。

### <a name="behavior-breaking-change"></a>行为重大更改

行为更改是最常见的重大更改类型：行为中的几乎任何更改都可能会造成中断。 库的更改（如方法签名、引发的异常或是输入或输出数据格式）都可能会对库使用者产生负面影响。 如果用户依赖于以前中断的行为，则甚至 bug 修复都可以作为重大更改。

添加功能和改进不良行为是件好事，但如果不谨慎，则可能会使现有用户非常难以升级。 帮助开发人员处理行为重大更改的一种方法是将更改隐藏在设置后面。 设置使开发人员可以更新到库的最新版本，同时可选择加入或退出重大更改。 此策略使开发人员可以保持更新，同时使其使用代码随时间推移而进行适应。

例如，ASP.NET Core MVC 提出了[兼容性版本](/aspnet/core/mvc/compatibility-version)这一概念，可在 `MvcOptions` 中修改启用和禁用的功能。

✔️ 请考虑在默认情况下关闭新功能（如果它们会影响现有用户），通过设置让开发人员可以选择加入功能。

### <a name="binary-breaking-change"></a>二进制重大更改

二进制重大更改在更改库的公共 API 时发生，因此针对库的较旧版本编译的程序集将不再能够调用 API。 例如，通过添加新参数来更改方法签名会导致针对库的较旧版本编译的程序集引发 <xref:System.MissingMethodException>。

二进制重大更改还可能会中断整个程序集  。 使用 `AssemblyName` 重命名程序集会更改程序集的标识，如同添加、删除或更改程序集的强命名密钥一样。 程序集标识的更改会中断所有使用它的已编译代码。

❌ 请勿更改程序集名称。

❌ 请勿添加、删除或更改强命名密钥。

✔️ 请考虑使用抽象基类而不是接口。

> 向接口添加任何内容都会导致实现它的现有类型失败。 抽象基类允许添加默认虚拟实现。

✔️ 请考虑在打算删除的类型和成员上放置 <xref:System.ObsoleteAttribute>。 该属性应具有有关更新代码以不再使用已过时 API 的说明。

> 调用具有 <xref:System.ObsoleteAttribute> 的类型和方法的代码会生成一个生成警告，其中包含向该属性提供的消息。 警告向使用已过时 API 的人员提供了进行迁移的缓冲时间，以便在删除已过时 API 时，大多数人都不再使用它。

```csharp
public class Document
{
    [Obsolete("LoadDocument(string) is obsolete. Use LoadDocument(Uri) instead.")]
    public static Document LoadDocument(string uri)
    {
        return LoadDocument(new Uri(uri));
    }

    public static Document LoadDocument(Uri uri)
    {
        // Load the document
    }
}
```

✔️ 请考虑在低级和中级库中无限期地保留具有 <xref:System.ObsoleteAttribute> 的类型和方法。

> 删除 API 是一种二进制重大更改。 如果维护成本较低，并且不会向库增添大量技术债务，请考虑保留已过时类型和方法。 不删除类型和方法可以帮助避免上述最糟糕情况。

## <a name="see-also"></a>另请参阅

- [面向 C# 开发人员的版本和更新注意事项](../../csharp/whats-new/version-update-considerations.md)
- [.NET 中的 API 重大更改的权威指南](https://stackoverflow.com/questions/1456785/a-definitive-guide-to-api-breaking-changes-in-net)
- [.NET 重大更改规则](https://github.com/dotnet/runtime/blob/master/docs/coding-guidelines/breaking-change-rules.md)

>[!div class="step-by-step"]
>[上一页](versioning.md)
