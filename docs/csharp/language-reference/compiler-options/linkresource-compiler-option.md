---
description: -linkresource (C# 编译器选项)
title: -linkresource (C# 编译器选项)
ms.date: 07/20/2015
f1_keywords:
- /linkresource
helpviewer_keywords:
- /linkresource compiler option [C#]
- linkres compiler option [C#]
- /linkres compiler option [C#]
- -linkres compiler option [C#]
- -linkresource compiler option [C#]
- linkresource compiler option [C#]
ms.assetid: 440c26c2-77c1-4811-a0a3-57cce3f5fc96
ms.openlocfilehash: 4efa0cbf286b40ad971bad66a7acce15e553eb39
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91194097"
---
# <a name="-linkresource-c-compiler-options"></a>-linkresource (C# 编译器选项)

在输出文件中创建指向 .NET 资源的链接。 不会在输出文件中添加资源文件。 这不同于会在输出文件中嵌入资源文件的 [-resource](./resource-compiler-option.md) 选项。  
  
## <a name="syntax"></a>语法  
  
```console  
-linkresource:filename[,identifier[,accessibility-modifier]]  
```  
  
## <a name="arguments"></a>自变量  

 `filename`  
 希望从程序集链接到的 .NET 资源文件。  
  
 `identifier`（可选）  
 资源的逻辑名称；用于加载资源的名称。 默认值是文件的名称。  
  
 `accessibility-modifier`（可选）  
 资源的可访问性：public 或 private。 默认值为 public。  
  
## <a name="remarks"></a>备注  

 默认情况下，如果使用 C# 编译器创建链接资源，则这些资源在程序集中是公有的。 若要使资源变为私有，请将 `private` 指定为可访问性修饰符。 不允许使用 `public` 或 `private` 以外的任何其他修饰符。  
  
 -linkresource 需要某个 [-target](./target-compiler-option.md) 选项（-target:module 除外）。  
  
 例如，如果 `filename` 是由 [Resgen.exe](../../../framework/tools/resgen-exe-resource-file-generator.md) 创建的或在开发环境中创建的 .NET 资源文件，则可使用 <xref:System.Resources> 命名空间中的成员来访问它。 有关详细信息，请参阅 <xref:System.Resources.ResourceManager?displayProperty=nameWithType>。 对于所有其他资源，请使用 <xref:System.Reflection.Assembly> 类中的 `GetManifestResource` 方法在运行时访问资源。  
  
 `filename` 中指定的文件可为任何格式。 例如，你可能希望生成程序集的本机 DLL 部分，从而可将它安装到全局程序集缓存中，并且可从该程序集中的托管代码访问它。 以下示例中的第二个示例演示了如何执行此操作。 可在程序集链接器中执行相同的操作。 以下示例中的第三个示例演示了如何执行此操作。 有关详细信息，请参阅 [Al.exe（程序集链接器）](../../../framework/tools/al-exe-assembly-linker.md)和[使用程序集和全局程序集缓存](../../../framework/app-domains/working-with-assemblies-and-the-gac.md)。  
  
 -linkres 是 -linkresource 的缩写形式。  
  
 此编译器选项在 Visual Studio 中不可用，并且无法以编程方式更改。  
  
## <a name="example"></a>示例  

 编译 `in.cs` 并链接到资源文件 `rf.resource`：  
  
```console  
csc -linkresource:rf.resource in.cs  
```  
  
## <a name="example"></a>示例  

 将 `A.cs` 编译为 DLL，链接到本机 DLL N.dll，并将输出文件放在全局程序集缓存 (GAC) 中。 在此示例中，A.dll 和 N.dll 都存在于 GAC 中。  
  
```console  
csc -linkresource:N.dll -t:library A.cs  
gacutil -i A.dll  
```  
  
## <a name="example"></a>示例  

 此示例执行的操作与上一示例相同，但使用程序集链接器选项执行。  
  
```console  
csc -t:module A.cs  
al -out:A.dll A.netmodule -link:N.dll
gacutil -i A.dll  
```  
  
## <a name="see-also"></a>另请参阅

- [C# 编译器选项](./index.md)
- [Al.exe（程序集链接器）](../../../framework/tools/al-exe-assembly-linker.md)
- [使用程序集和全局程序集缓存](../../../framework/app-domains/working-with-assemblies-and-the-gac.md)
- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
