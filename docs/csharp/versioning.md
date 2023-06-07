---
title: C# 版本控制 - C# 指南
description: 了解 C# 和 .NET 中的版本控制工作原理
ms.date: 01/08/2017
ms.technology: csharp-advanced-concepts
ms.assetid: aa8732d7-5cd0-46e1-994a-78017f20d861
ms.openlocfilehash: dc192337e4eaa5f9f1d6509ea8c15deeac34a48c
ms.sourcegitcommit: 62285ec11fa8e8424bab00511a90760c60e63c95
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/20/2020
ms.locfileid: "81645457"
---
# <a name="versioning-in-c"></a>C\# 中的版本控制

本教程将介绍版本控制在 .NET 中的含义。 还将介绍对库进行版本控制以及升级到新版本的库时需要考虑的因素。

## <a name="authoring-libraries"></a>创作库

对于创建 .NET 库以供公共使用的开发人员，经常需要推出新更新。 如何处理此过程关系重大，因为开发人员需确保从现有代码无缝转换到新版本的库。 以下是创建新版本时的几个注意事项：

### <a name="semantic-versioning"></a>语义版本控制

[语义版本控制](https://semver.org/)（简称 SemVer）是应用于库版本的命名约定，用于表示特定里程碑事件。
理想情况下，提供给库的版本信息应帮助开发人员确定版本是否与使用相同库的早期版本的项目兼容。

SemVer 的最基本方法是 3 组件格式 `MAJOR.MINOR.PATCH`，其中：

- 进行不兼容的 API 更改时，`MAJOR` 将会增加
- 以后向兼容方式添加功能时，`MINOR` 将会增加
- 进行后向兼容 bug 修复时，`PATCH` 将会增加

将版本信息应用于 .NET 库时，还可通过其他方法指定其他方案，如预发布版本等。

### <a name="backwards-compatibility"></a>后向兼容

发布新版本的库时，与先前版本的后向兼容很可能成为主要关注事项之一。
如果重新编译时，依赖于先前版本的代码适用于新版本，则新版本的库与先前版本是源兼容的。
在没有重新编译的情况下，如果依赖于先前版本的应用程序适用于新版本，则新版本的库是二进制兼容的。

以下是维护与较旧版本库的后向兼容时的注意事项：

- 虚拟方法：如果在新版本中使虚拟方法成为非虚拟方法，则必须更新替代该方法的项目。 这是一项重大更改，强烈建议不要执行此操作。
- 方法签名：虽然更新方法行为也需要更改其签名，但应创建重载，使调用该方法的代码仍可正常运行。
始终可以使用旧方法签名来调用新方法签名，以使实现保持一致。
- [已过时属性](language-reference/attributes/general.md#obsolete-attribute)：可在代码中使用此属性指定已弃用且很可能在将来版本中删除的类或类成员。 这可确保使用此库的开发人员能更好地为重大更改做好准备。
- 可选方法参数：如果使以前的可选方法参数变为强制性方法参数或更改它们的默认值，则需要更新不提供这些参数的所有代码。

> [!NOTE]
> 将强制性参数变为可选参数应几乎没有影响，尤其是在不更改方法的行为的情况下。

为用户提供的升级到新版本库的方法越简单，用户升级的速度很可能会越快。

### <a name="application-configuration-file"></a>应用程序配置文件

.NET 开发人员很可能已在大多数项目类型中遇到过 [`app.config` 文件](../framework/configure-apps/file-schema/index.md)。
此类简单配置文件对于改进新更新的推出有重要作用。 通常应以以下方式设计库：将可能定期更改的信息存储在 `app.config` 文件中。这样，当更新此类信息时，只需使用新版本的配置文件替换旧版本配置文件即可，而无需重新编译库。

## <a name="consuming-libraries"></a>使用库

作为使用由其他开发人员构建的 .NET 库的开发人员，你很可能已经发现新版本的库可能不与项目完全兼容，你经常需要更新代码以使用这些更改。

幸运的是，C# 和 .NET 生态系统附带一些功能和技术，通过这些功能和技术，我们可以轻松更新应用，使其适用于可能引入重大更改的新版本库。

### <a name="assembly-binding-redirection"></a>程序集绑定重定向

可使用 app.config  文件更新应用使用的库版本。 通过添加所谓的[绑定重定向  ](../framework/configure-apps/redirect-assembly-versions.md)，可在无需重新编译应用的情况下使用新的库版本。 下面的示例演示了更新应用的 app.config  文件的方法，以便使用 `ReferencedLibrary` 的 `1.0.1` 修补程序版本，而不是最初编译时使用的 `1.0.0` 版本。

```xml
<dependentAssembly>
    <assemblyIdentity name="ReferencedLibrary" publicKeyToken="32ab4ba45e0a69a1" culture="en-us" />
    <bindingRedirect oldVersion="1.0.0" newVersion="1.0.1" />
</dependentAssembly>
```

> [!NOTE]
> 仅当 `ReferencedLibrary` 的新版本与应用二进制兼容时，此方法有效。
> 有关确定兼容性时需要注意的更改，请参阅上文中的[后向兼容](#backwards-compatibility)部分。

### <a name="new"></a>new

使用 `new` 修饰符隐藏基类的继承成员。 这是派生类响应基类中的更新的一种方法。

请参见以下示例：

[!code-csharp[Sample usage of the 'new' modifier](~/samples/snippets/csharp/versioning/new/Program.cs#sample)]

**输出**

```console
A base method
A derived method
```

上面的示例演示 `DerivedClass` 如何隐藏 `BaseClass` 中的 `MyMethod`方法。
也就是说，当新版本库中的基类添加派生类中已存在的成员时，在派生类成员上使用 `new` 修饰符即可隐藏基类成员。

未指定 `new` 修饰符时，派生类将默认隐藏基类中的冲突成员，尽管会生成编译器警告，但仍将编译代码。 也就是说，仅需向现有类添加新成员，新版本的库即可与依赖于它的代码实现源兼容和二进制兼容。

### <a name="override"></a>override

`override` 修饰符指派生实现会扩展基类成员的实现而不是将其隐藏。 基类成员需要具有应用于自身的 `virtual` 修饰符。

[!code-csharp[Sample usage of the 'override' modifier](../../samples/snippets/csharp/versioning/override/Program.cs#sample)]

**输出**

```console
Base Method One: Method One
Derived Method One: Derived Method One
```

`override` 修饰符将在编译时计算，如果此修饰符找不到要重写的虚拟成员，编译器将引发错误。

了解所讨论的这些技术以及使用情境，对于简化库版本之间的转换有重要作用。
