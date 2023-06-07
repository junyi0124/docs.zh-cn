---
title: 如何：使用强名称为程序集签名
description: 本文说明如何使用“签名”选项卡、程序集链接器、程序集属性或编译器选项对 .NET 程序集进行强名称签名。
ms.date: 08/20/2019
helpviewer_keywords:
- strong-named assemblies, signing with strong names
- signing assemblies
- assemblies [.NET], signing
- assemblies [.NET], strong-named
ms.assetid: 2c30799a-a826-46b4-a25d-c584027a6c67
dev_langs:
- csharp
- vb
- cpp
ms.openlocfilehash: 5192f7f372b9ef7927930c3599aebc6fca9f1f0f
ms.sourcegitcommit: 279fb6e8d515df51676528a7424a1df2f0917116
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92687656"
---
# <a name="how-to-sign-an-assembly-with-a-strong-name"></a>如何：使用强名称为程序集签名

> [!NOTE]
> 虽然 .NET Core 支持强名称程序集，而且 .NET Core 库中的所有程序集均已签名，但大多数第三方程序集不需要强名称。 有关详细信息，请参阅 GitHub 上的[强名称签名](https://github.com/dotnet/runtime/blob/master/docs/project/strong-name-signing.md)。

可通过许多方法为程序集签署强名称：  
  
- 在 Visual Studio 中，通过使用项目的 **“属性”** 对话框中的 **“签名”** 选项卡。 这是为程序集签署强名称的最简单且最方便的方法。  
  
- 通过使用[程序集链接器 (Al.exe)](../../framework/tools/al-exe-assembly-linker.md) 来链接 .NET Framework 代码模块（.netmodule 文件）与密钥文件。  
  
- 通过使用程序集特性将强名称信息插入代码中。 你可以使用 <xref:System.Reflection.AssemblyKeyFileAttribute> 或 <xref:System.Reflection.AssemblyKeyNameAttribute> 特性，具体取决于要使用的密钥文件所在的位置。  
  
- 通过使用编译器选项。  
  
 要使用强名称为程序集签名，必须具有加密密钥对。 有关创建密钥对的详细信息，请参阅[如何：创建公钥/私钥对](create-public-private-key-pair.md)。  
  
## <a name="create-and-sign-an-assembly-with-a-strong-name-by-using-visual-studio"></a>使用 Visual Studio 创建程序集并使用强名称为程序集签名  
  
1. 在“解决方案资源管理器” 中，打开项目的快捷菜单，然后选择“属性” 。  
  
2. 选择 **“签名”** 选项卡。  
  
3. 选择“为程序集签名”框。  
  
4. 在“选择强名称密钥文件”框中，选择“浏览”，然后导航到密钥文件 。 若要创建新的密钥文件，请选择“新建”，并在“创建强名称密钥”对话框中输入其名称 。  
  
> [!NOTE]
> 为了[延迟为程序集签名](delay-sign.md)，请选择公钥文件。  
  
### <a name="create-and-sign-an-assembly-with-a-strong-name-by-using-the-assembly-linker"></a>使用程序集链接器创建程序集并使用强名称为程序集签名  
  
在 [Visual Studio 开发人员命令提示](../../framework/tools/developer-command-prompt-for-vs.md)处，输入以下命令：  

al /out:\<*assemblyName*> \<moduleName> /keyfile:\<*keyfileName*>   

其中：  

- assemblyName 是程序集链接器将发出的强签名程序集（.dll 或 .exe 文件）的名称  。  
  
- moduleName 是包含一个或多个类型的 .NET Framework 代码模块（.netmodule 文件）的名称 。 可通过在 C# 或 Visual Basic 中使用 `/target:module` 开关对代码进行编译来创建.netmodule 文件。
  
- keyfileName 是包含密钥对的容器或文件的名称。 程序集链接器解释与当前目录相关的相对路径。  

下面的示例使用密钥文件 sgKey.snk 为程序集 MyAssembly.dll 签署强名称 。  

```console
al /out:MyAssembly.dll MyModule.netmodule /keyfile:sgKey.snk  
```  
  
有关此工具的详细信息，请参阅 [程序集链接器](../../framework/tools/al-exe-assembly-linker.md)。  
  
## <a name="sign-an-assembly-with-a-strong-name-by-using-attributes"></a>使用属性为程序集签署强名称  
  
1. 将 <xref:System.Reflection.AssemblyKeyFileAttribute?displayProperty=nameWithType> 或 <xref:System.Reflection.AssemblyKeyNameAttribute> 特性添加到源代码文件中，并指定包含为程序集签署强名称时要使用的密钥对的文件或容器的名称。  

2. 通常会编译源代码文件。  

   > [!NOTE]
   > 当 C# 和 Visual Basic 编译器在源代码中遇到 <xref:System.Reflection.AssemblyKeyFileAttribute> 或 <xref:System.Reflection.AssemblyKeyNameAttribute> 特性时，会发出编译器警告（分别为 CS1699 和 BC41008）。 你可以忽略这些警告。  

下面的代码示例将 <xref:System.Reflection.AssemblyKeyFileAttribute> 属性用于名为 keyfile.snk 的密钥文件（位于编译程序集的目录中）。  

```cpp
[assembly:AssemblyKeyFileAttribute("keyfile.snk")];
```

```csharp
[assembly:AssemblyKeyFileAttribute("keyfile.snk")]
```

```vb
<Assembly:AssemblyKeyFileAttribute("keyfile.snk")>
```

在编译源文件时，也可以延迟为程序集签名。 有关详细信息，请参阅[延迟为程序集签名](delay-sign.md)。  

## <a name="sign-an-assembly-with-a-strong-name-by-using-the-compiler"></a>使用编译器为程序集签署强名称  

使用 C# 和 Visual Basic 中的 `/keyfile` 或 `/delaysign` 编译器选项，或使用 C++ 中的 `/KEYFILE` 或 `/DELAYSIGN` 链接器选项编译源代码文件。 在选项名称后，添加冒号和密钥文件的名称。 使用命令行编译器时，你可以将密钥文件复制到包含源代码文件的目录中。  

有关延迟签名的信息，请参阅[延迟为程序集签名](delay-sign.md)。  

下面的示例使用 C# 编译器并借助密钥文件 sgKey.snk 为程序集 UtilityLibrary.dll 签署强名称 。  

```cmd
csc /t:library UtilityLibrary.cs /keyfile:sgKey.snk  
```  

## <a name="see-also"></a>请参阅

- [创建和使用具有强名称的程序集](create-use-strong-named.md)
- [如何：创建公钥/私钥对](create-public-private-key-pair.md)
- [Al.exe（程序集链接器）](../../framework/tools/al-exe-assembly-linker.md)
- [延迟为程序集签名](delay-sign.md)
- [管理程序集和清单签名](/visualstudio/ide/managing-assembly-and-manifest-signing)
- [项目设计器的“签名”页](/visualstudio/ide/reference/signing-page-project-designer)
