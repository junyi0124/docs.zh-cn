---
description: -lib（C# 编译器选项）
title: -lib（C# 编译器选项）
ms.date: 07/20/2015
f1_keywords:
- /lib
helpviewer_keywords:
- lib compiler option [C#]
- -lib compiler option [C#]
- /lib compiler option [C#]
ms.assetid: b0efcc88-e8aa-4df4-a00b-8bdef70b7673
ms.openlocfilehash: 9478501ea98ec1b9d3ec2761bc4ebf3f6bef656c
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91152437"
---
# <a name="-lib-c-compiler-options"></a>-lib（C# 编译器选项）

-lib 选项指定通过 [-reference（C# 编译器选项）](./reference-compiler-option.md)选项引用的程序集的位置。  
  
## <a name="syntax"></a>语法  
  
```console  
-lib:dir1[,dir2]  
```  
  
## <a name="arguments"></a>自变量  

 `dir1`  
 在当前工作目录（调用编译器的目录）或公共语言运行时的系统目录中未找到引用的程序集时，编译器将在其中进行查找的目录。  
  
 `dir2`  
 要在其中搜索程序集引用的一个或多个附加目录。 用逗号分隔每个附加目录的名称，中间不要有空格。  
  
## <a name="remarks"></a>备注  

 编译器按以下顺序搜索未完全限定的程序集引用：  
  
1. 当前工作目录。 该目录为从其调用编译器的目录。  
  
2. 公共语言运行时系统目录。  
  
3. 由 -lib 指定的目录。  
  
4. 由 LIB 环境变量指定的目录。  
  
 使用 -reference 指定程序集引用。  
  
 -lib 是累加的；每一次指定的值都追加到以前的值中。  
  
 另一种使用 -lib 的方法是，将任何所需的程序集复制到工作目录中；这样可以仅将程序集名称传递给 -reference 。 然后可以从工作目录中删除这些程序集。 由于程序集清单中未指定依赖程序集的路径，因此应用程序可以在目标计算机上启动，然后查找并使用全局程序集缓存中的程序集。  
  
 编译器可以引用程序集并不表示公共语言运行时可以在运行时找到并加载程序集。 有关运行时如何搜索引用的程序集的详细信息，请参阅[运行时如何定位程序集](../../../framework/deployment/how-the-runtime-locates-assemblies.md)。  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>在 Visual Studio 开发环境中设置此编译器选项  
  
1. 打开项目的“属性页”  对话框。  
  
2. 单击“引用路径”属性页。  
  
3. 修改列表框的内容。  
  
 有关如何以编程方式设置此编译器选项的信息，请参阅 <xref:VSLangProj80.ProjectProperties3.ReferencePath%2A>。  
  
## <a name="example"></a>示例  

 编译 t2.cs 以创建 .exe 文件。 编译器将在工作目录和 C 驱动器的根目录中查找程序集引用。  
  
```console  
csc -lib:c:\ -reference:t2.dll t2.cs  
```  
  
## <a name="see-also"></a>另请参阅

- [C# 编译器选项](./index.md)
- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
