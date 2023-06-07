---
description: -resource（C# 编译器选项）
title: -resource（C# 编译器选项）
ms.date: 07/20/2015
f1_keywords:
- /resource
helpviewer_keywords:
- -resource compiler option [C#]
- /resource compiler option [C#]
- -res compiler option [C#]
- /res compiler option [C#]
- res compiler option [C#]
- resource compiler option [C#]
ms.assetid: 5212666e-98ab-47e4-a497-b5545ab15c7f
ms.openlocfilehash: 6f90ce6c1590784cefbd5f15ca8a36941aad77ed
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91193772"
---
# <a name="-resource-c-compiler-options"></a>-resource（C# 编译器选项）

将指定资源嵌入输出文件。  
  
## <a name="syntax"></a>语法  
  
```console  
-resource:filename[,identifier[,accessibility-modifier]]  
```  
  
## <a name="arguments"></a>自变量  

 `filename`  
 要嵌入到输出文件的 .NET 资源文件。  
  
 `identifier`（可选）  
 资源的逻辑名称；用于加载资源的名称。 默认值是文件名的名称。  
  
 `accessibility-modifier`（可选）  
 资源的可访问性：public 或 private。 默认值为 public。  
  
## <a name="remarks"></a>备注  

 使用 [linkresource](./linkresource-compiler-option.md) 将资源链接至程序集，不向输出文件添加资源文件。  
  
 默认情况下，如果使用 C# 编译器创建资源，则这些资源在程序集中是公有的。 若要使资源变为私有，请将 `private` 指定为可访问性修饰符。 不允许使用 `public` 或 `private` 以外的任何其他可访问性。  
  
 例如，如果 `filename` 是由 [Resgen.exe](../../../framework/tools/resgen-exe-resource-file-generator.md) 创建的或在开发环境中创建的 .NET 资源文件，则可使用 <xref:System.Resources> 命名空间中的成员来访问它。 有关详细信息，请参阅 <xref:System.Resources.ResourceManager?displayProperty=nameWithType>。 对于所有其他资源，请使用 <xref:System.Reflection.Assembly> 类中的 `GetManifestResource` 方法在运行时访问资源。  
  
 /res 是 /resource 的缩写形式。  
  
 输出文件中资源的顺序由命令行所指定的顺序决定。  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>在 Visual Studio 开发环境中设置此编译器选项  
  
1. 向项目添加资源文件。  
  
2. 选择要嵌入解决方案资源管理器的文件。  
  
3. 在“属性”窗口中为文件选择“生成操作”。  
  
4. 将“生成操作”设置为“嵌入的资源”。  
  
 有关如何以编程方式设置此编译器选项的信息，请参阅 <xref:VSLangProj80.FileProperties2.BuildAction%2A>。  
  
## <a name="example"></a>示例  

 编译 `in.cs` 并附加资源文件 `rf.resource`：  
  
```console  
csc -resource:rf.resource in.cs  
```  
  
## <a name="see-also"></a>另请参阅

- [C# 编译器选项](./index.md)
- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
