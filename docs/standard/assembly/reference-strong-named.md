---
title: 如何：引用具有强名称的程序集
description: 本文介绍如何在编译时或运行时引用具有强名称的 .NET 程序集中的类型或资源。
ms.date: 08/20/2019
helpviewer_keywords:
- strong-named assemblies, compile-time references
- compile-time assembly referencing
- assemblies [.NET], strong-named
- assembly binding, strong-named
ms.assetid: 4c6a406a-b5eb-44fa-b4ed-4e95bb95a813
dev_langs:
- csharp
- vb
- cpp
ms.openlocfilehash: 919e4f4cf467e8fc28c3d007963393dad134ab57
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95724047"
---
# <a name="how-to-reference-a-strong-named-assembly"></a>如何：引用具有强名称的程序集

引用强名称程序集中的类型或资源的过程通常是透明的。 可在编译时（早期绑定）或在运行时进行引用。  
  
向编译器指示要编译的程序集显式引用另一程序集时，会发生编译时引用。 使用编译时引用时，编译器会自动获取目标强名称程序集的公钥，并将其放在正在编译的程序集的程序集引用中。
  
> [!NOTE]
> 具有强名称的程序集只能使用其他具有强名称的程序集的类型。 否则，将会危害具有强名称的程序集的安全性。  
  
## <a name="make-a-compile-time-reference-to-a-strong-named-assembly"></a>对具有强名称的程序集进行编译时引用  

在命令提示符下，键入以下命令：  

\<*compiler command*> /reference:\<*assembly name*>  

在此命令中，compiler command 是所用语言的编译器命令，assembly name 是引用的强名称程序集的名称 。 还可使用其他编译器选项，如用于创建库程序集的 /t:library 选项。  

以下示例创建名为 myAssembly.dll 的程序集，该程序集从名为 myAssembly.cs 的代码模块中引用名为 myLibAssembly.dll 的强名称程序集  。  

```cmd
csc /t:library myAssembly.cs /reference:myLibAssembly.dll  
```  

## <a name="make-a-run-time-reference-to-a-strong-named-assembly"></a>对强名称程序集进行运行时引用  
  
对强名称程序集进行运行时引用（例如使用 <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> 或 <xref:System.Reflection.Assembly.GetType%2A?displayProperty=nameWithType> 方法）时，必须使用引用的强名称程序集的显示名称。 显示名称的语法如下：  

\<*assembly name*>**,** \<*version number*>**,** \<*culture*>**,** \<*public key token*>  

例如：  

```console
myDll, Version=1.1.0.0, Culture=en, PublicKeyToken=03689116d3a4ae33
```  

在此示例中，`PublicKeyToken` 是十六进制格式的公钥标记。 如果没有区域性值，请使用 `Culture=neutral`。  

下面的代码示例演示如何将此信息用于 <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> 方法。  

```cpp
Assembly^ myDll =
    Assembly::Load("myDll, Version=1.0.0.1, Culture=neutral, PublicKeyToken=9b35aa32c18d4fb1");
```

```csharp
Assembly myDll =
    Assembly.Load("myDll, Version=1.0.0.1, Culture=neutral, PublicKeyToken=9b35aa32c18d4fb1");
```

```vb
Dim myDll As Assembly = _
    Assembly.Load("myDll, Version=1.0.0.1, Culture=neutral, PublicKeyToken=9b35aa32c18d4fb1")
```

可使用以下[强名称 (Sn.exe)](../../framework/tools/sn-exe-strong-name-tool.md) 命令为特定程序集打印十六进制格式的公钥和公钥标记：  

sn-Tp \<** *assembly* **>  

如果有公钥文件，则可改用以下命令（请注意命令行选项大小写的区别）：  

sn-tp \<** *public key file* **>  

## <a name="see-also"></a>请参阅

- [创建和使用具有强名称的程序集](create-use-strong-named.md)
