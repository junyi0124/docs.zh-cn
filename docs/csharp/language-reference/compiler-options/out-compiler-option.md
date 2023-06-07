---
description: -out（C# 编译器选项）
title: -out（C# 编译器选项）
ms.date: 07/20/2015
f1_keywords:
- /out
helpviewer_keywords:
- /out compiler option [C#]
- out compiler option [C#]
- -out compiler option [C#]
ms.assetid: 70d91d01-7bd2-4aea-ba8b-4e9807e9caa5
ms.openlocfilehash: 409760ee0b147065a2128c62c304fb5d70cfcf42
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91193876"
---
# <a name="-out-c-compiler-options"></a>-out（C# 编译器选项）

-out 选项指定输出文件的名称。  
  
## <a name="syntax"></a>语法  
  
```console  
-out:filename  
```  
  
## <a name="arguments"></a>自变量  

 `filename`  
 编译器创建的输出文件的名称。  
  
## <a name="remarks"></a>备注  

 在命令行中，可以为编译指定多个输出文件。 编译器应在 -out 选项下查找一个或多个源代码文件。 然后，所有源代码文件都将编译为 -out 选项指定的输出文件。  
  
 指定想要创建的文件的完整名称和扩展名。  
  
 如果不指定输出文件的名称：  
  
- .exe 将采用包含 Main 方法的源代码文件中的名称。  
  
- .dll 或者 .netmodule 将从第一个源代码文件中获取其名称。  
  
 用于编译一个输出文件的源代码文件不能同样用于编译另一输出文件。  
  
 如果在命令行编译中生成多个输出文件，请记住，仅其中一个输出文件可以是程序集，且只有（使用 -out 隐式或显式）指定的第一个输出文件可以是程序集。  
  
 在编译时生成的任何模块都将成为与编译时生成的程序集关联的文件。 使用 [ildasm.exe](../../../framework/tools/ildasm-exe-il-disassembler.md) 查看程序集清单，了解关联文件。  
  
 为使 exe 成为友元程序集的目标，-out 编译器选项是必需的。 有关详细信息，请参阅[友元程序集](../../../standard/assembly/friend.md)。  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>在 Visual Studio 开发环境中设置此编译器选项  
  
1. 打开项目的“属性”页。  
  
2. 单击“应用程序”属性页。  
  
3. 修改程序集名称属性。  
  
     以编程方式设置此编译器选项：<xref:VSLangProj80.ProjectProperties3.OutputFileName%2A> 是只读属性，由项目类型（exe、库等）和程序集名称的组合决定。 设置输出文件名称必须修改一个或两个属性。  
  
## <a name="example"></a>示例  

 编译 `t.cs` 并创建输出文件 `t.exe`以及生成 `t2.cs` 并创建模块输出文件 `mymodule.netmodule`：  
  
```console  
csc t.cs -out:mymodule.netmodule -target:module t2.cs  
```  
  
## <a name="see-also"></a>另请参阅

- [C# 编译器选项](./index.md)
- [友元程序集](../../../standard/assembly/friend.md)
- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
