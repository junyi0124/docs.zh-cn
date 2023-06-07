---
description: -main（C# 编译器选项）
title: -main（C# 编译器选项）
ms.date: 07/20/2015
f1_keywords:
- /main
helpviewer_keywords:
- -main compiler option [C#]
- main compiler option [C#]
- /main compiler option [C#]
ms.assetid: 975cf4d5-36ac-4530-826c-4aad0c7f2049
ms.openlocfilehash: c27898de2a7cc2f3c01c51f8de1122e81b2233b4
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91194110"
---
# <a name="-main-c-compiler-options"></a>-main（C# 编译器选项）

如果多个类包含 **Main** 方法，此选项将指定包含程序入口点的类。

## <a name="syntax"></a>语法

```console
-main:class
```

## <a name="arguments"></a>自变量

 `class`  
 此类型包含 **Main** 方法。  
 提供的类名必须是完全限定类名；它必须包括完整命名空间（包含类），后跟类名。 例如，当 `Main` 方法位于 `MyApplication.Core` 命名空间中的 `Program` 类中时，编译器选项必须为 `-main:MyApplication.Core.Program`。

## <a name="remarks"></a>备注

如果编译包含具有 [Main](../../programming-guide/main-and-command-args/index.md) 方法的多个类型，则可以指定哪个类型包含你想用作程序入口点的 **Main** 方法。

此选项在编译 .exe 文件时使用。

### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>在 Visual Studio 开发环境中设置此编译器选项

1. 打开项目的“属性”页。

2. 单击“应用程序”属性页。

3. 修改“启动对象”属性。

    若要以编程方式设置此编译器选项，请参阅 <xref:VSLangProj80.ProjectProperties3.StartupObject%2A>。

### <a name="to-set-this-compiler-option-by-manually-editing-the-csproj-file"></a>通过手动编辑 .csproj 文件来设置此编译器选项

可以通过编辑 .csproj 文件并在 `PropertyGroup` 部分添加元素 `StartupObject` 来设置此选项。 例如：

```xml
  <PropertyGroup>
    ...
    <StartupObject>MyApplication.Core.Program</StartupObject>
  </PropertyGroup>
```

## <a name="example"></a>示例

编译 `t2.cs` 和 `t3.cs`，指出 **Main** 方法可在 `Test2` 中找到：

```console
csc t2.cs t3.cs -main:Test2
```

## <a name="see-also"></a>请参阅

- [C# 编译器选项](./index.md)
- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
