---
description: -debug（C# 编译器选项）
title: -debug（C# 编译器选项）
ms.date: 07/20/2015
f1_keywords:
- /debug
helpviewer_keywords:
- debug compiler option [C#]
- -debug compiler option [C#]
- /debug compiler option [C#]
ms.assetid: e2b48c07-01bc-45cc-a52c-92e9085eb969
ms.openlocfilehash: 77aa9bf0fc0911f441d00403d773ee5ae03ae99b
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91173290"
---
# <a name="-debug-c-compiler-options"></a>-debug（C# 编译器选项）

-debug 选项将使编译器生成调试信息，并将此信息放置在输出文件或文件中。  
  
## <a name="syntax"></a>语法  
  
```console  
-debug[+ | -]  
-debug:{full | pdbonly}  
```  
  
## <a name="arguments"></a>自变量  

 `+` &#124; `-`  
 指定 `+` 或仅 -debug 将导致编译器生成调试信息并将此信息放在程序数据库（.pdb 文件）中。 指定 `-`（在不指定 -debug 时生效）将导致不创建任何调试信息。  
  
 `full` &#124; `pdbonly`  
 指定编译器生成的调试信息的类型。 完整参数（在不指定 -debug:pdbonly 时生效）允许将调试器附加到正在运行的程序。 指定 pdbonly 允许在调试器中启动程序时进行源代码调试，但仅在正在运行的程序附加到调试器时才显示汇编程序。  
  
## <a name="remarks"></a>备注  

 使用此选项创建调试版本。 如果未指定 -debug、-debug+ 或 -debug:full，则无法调试程序的输出文件  。  
  
 如果使用 -debug:full，请注意，对经过优化的 JIT 代码的速度和大小会存在一定影响，且对包含 -debug:full 的代码质量也有一定影响 。 建议使用 -debug:pdbonly 或不使用 PDB 生成发布代码。  
  
> [!NOTE]
> -debug:pdbonly 和 -debug:full 之间的一个区别在于，使用 -debug:full，编译器将发出 <xref:System.Diagnostics.DebuggableAttribute>，用于告知 JIT 编译器有可用调试信息  。 因此，在使用 -debug:full 时，如果代码包含设置为 false 的 <xref:System.Diagnostics.DebuggableAttribute>，则将出现错误。  
  
 有关如何配置应用程序的调试性能的详细信息，请参阅[令映像更易于调试](../../../framework/debug-trace-profile/making-an-image-easier-to-debug.md)。  
  
 若要更改 .pdb 文件的位置，请参阅 [-pdb（C# 编译器选项）](./pdb-compiler-option.md)。  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>在 Visual Studio 开发环境中设置此编译器选项  
  
1. 打开项目的“属性”页。  
  
2. 单击“生成”属性页。  
  
3. 单击“高级”按钮。  
  
4. 修改“调试信息”属性。  
  
 有关如何以编程方式设置此编译器选项的信息，请参阅 <xref:VSLangProj80.CSharpProjectConfigurationProperties3.DebugSymbols%2A>。  
  
## <a name="example"></a>示例  

 将调试信息放在输出文件 `app.pdb`：  
  
```console  
csc -debug -pdb:app.pdb test.cs  
```  
  
## <a name="see-also"></a>另请参阅

- [C# 编译器选项](./index.md)
- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
