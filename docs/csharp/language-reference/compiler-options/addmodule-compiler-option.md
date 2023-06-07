---
description: -addmodule（C# 编译器选项）
title: -addmodule（C# 编译器选项）
ms.date: 07/20/2015
f1_keywords:
- /addmodule
helpviewer_keywords:
- /addmodule compiler option [C#]
- -addmodule compiler option [C#]
- addmodule compiler option [C#]
ms.assetid: ed604546-0dc2-4bd4-9a3e-610a8d973e58
ms.openlocfilehash: ec72fc76b3d550029b1286f64b8f86e69e721468
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91150565"
---
# <a name="-addmodule-c-compiler-options"></a>-addmodule（C# 编译器选项）

此选项将添加一个模块，该模块通过将 target: module 切换到当前编译进行创建。  
  
## <a name="syntax"></a>语法  
  
```console  
-addmodule:file[;file2]  
```  
  
## <a name="arguments"></a>自变量  

 `file`, `file2`  
 包含元数据的输出文件。 该文件不能包含程序集清单。 若要导入多个文件，请用逗号或分号将文件名隔开。  
  
## <a name="remarks"></a>备注  

 通过 -addmodule 添加的所有模块在运行时必须位于与输出文件相同的目录中。 也就是说，在编译时可在任何目录中指定模块，但在运行时该模块必须位于应用程序目录中。 如果在运行时该模块不位于应用程序目录中，则将出现 <xref:System.TypeLoadException>。  
  
 `file` 不能包含程序集。 例如，如果输出文件使用 [-target:module](./target-module-compiler-option.md) 创建，其元数据可通过 -addmodule 导入。  
  
 如果输出文件通过使用 -target 选项而不是 -target:module 创建，则其元数据不能通过 -addmodule 导入，但可以通过 [-reference](./reference-compiler-option.md) 导入  。  
  
 此编译器选项在 Visual Studio 中不可用；项目不能引用模块。 此外，不能以编程方式更改此编译器选项。  
  
## <a name="example"></a>示例  

 编译源文件 `input.cs`，并从 `metad1.netmodule` 和 `metad2.netmodule` 添加元数据以生成 `out.exe`：  
  
```console  
csc -addmodule:metad1.netmodule;metad2.netmodule -out:out.exe input.cs  
```  
  
## <a name="see-also"></a>另请参阅

- [C# 编译器选项](./index.md)
- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
- [多文件程序集](../../../framework/app-domains/multifile-assemblies.md)
- [如何：生成单文件程序集](../../../framework/app-domains/build-multifile-assembly.md)
