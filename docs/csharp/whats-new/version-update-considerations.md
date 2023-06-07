---
title: 面向 C# 开发人员的版本和更新注意事项
description: 在库中引入新语言功能可能会影响使用它的代码。
ms.topic: reference
ms.date: 09/19/2018
ms.openlocfilehash: 6de0e3ba33af34edba73819dcb9268f40f8a5ea4
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90552422"
---
# <a name="version-and-update-considerations-for-c-developers"></a>面向 C# 开发人员的版本和更新注意事项

兼容性是一个非常重要的目标，因为新功能已添加到 C# 语言中。 在几乎所有情况下，都可以使用新的编译器版本重新编译现有代码，不会出现任何问题。

在库中采用新语言功能时，可能需要多加注意。 你可能要使用最新版本中提供的功能创建一个新库，并且需要确保使用以前版本的编译器生成的应用可以使用它。 或者，你可能要升级现有库，但许多用户可能还没有升级版本。 在决定采用新功能时，需要考虑两种兼容性：源兼容和二进制兼容。

## <a name="binary-compatible-changes"></a>二进制兼容的更改

如果更新的库可以在不重新生成应用程序和使用它的库的情况下使用，那么对库的更改属于二进制兼容  的更改。 不需要重新生成从属程序集，也不需要更改任何源代码。 二进制兼容的更改也是源兼容的更改。

## <a name="source-compatible-changes"></a>源兼容的更改

如果使用库的应用程序和库不需要更改源代码，但必须根据新版本重新编译源才能正常工作，那么对库的更改属于源兼容  的更改。

## <a name="incompatible-changes"></a>不兼容的更改

如果更改既不是源兼容  的更改，也不是二进制兼容  的更改，则需要在从属库和应用程序中进行源代码更改和重新编译。

## <a name="evaluate-your-library"></a>评估库

这些兼容性概念会影响库的公共声明和受保护声明，而不会影响其内部实现。 在内部采用任何新功能始终是二进制兼容  的更改。  

二进制兼容  的更改提供新语法，可以生成与旧语法相同的公共声明的已编译代码。 例如，将方法更改为 expression-bodied 的成员是二进制兼容  的更改：

原始代码：

```csharp
public double CalculateSquare(double value)
{
    return value * value;
}
```

新代码：

```csharp
public double CalculateSquare(double value) => value * value;
```

 源兼容的更改引入了可更改公共成员的已编译代码的语法，只不过是通过与现有调用站点兼容的方式执行。 例如，将方法签名从 by 值参数更改为 `in` by 引用参数是源兼容的更改，但不是二进制兼容的更改：

原始代码：

```csharp
public double CalculateSquare(double value) => value * value;
```

新代码：

```csharp
public double CalculateSquare(in double value) => value * value;
```

[新增功能](./csharp-9.md)一文说明了引入会影响公共声明的功能是源兼容的更改还是二进制兼容的更改。
