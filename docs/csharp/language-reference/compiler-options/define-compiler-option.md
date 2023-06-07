---
description: -define（C# 编译器选项）
title: -define（C# 编译器选项）
ms.date: 07/20/2015
f1_keywords:
- /define
helpviewer_keywords:
- -define compiler option [C#]
- /define compiler option [C#]
- -d compiler option [C#]
- define compiler option [C#]
- /d compiler option [C#]
- d compiler option [C#]
ms.assetid: f17d7b4d-82d0-4133-8563-68cced1cac6e
ms.openlocfilehash: 74c9a23cd1b3a691063c2976a593c9b3a63ca618
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91173264"
---
# <a name="-define-c-compiler-options"></a>-define（C# 编译器选项）

-define 选项将 `name` 定义为程序中所有源代码文件的符号。  
  
## <a name="syntax"></a>语法  
  
```console  
-define:name[;name2]  
```  
  
## <a name="arguments"></a>自变量  

 `name`, `name2`  
 一个或多个要定义的符号的名称。  
  
## <a name="remarks"></a>注解  

 -define 选项具有与使用 [#define](../preprocessor-directives/preprocessor-define.md) 预处理器指令相同的效果，但编译器选项对项目中的所有文件都有效。 符号在源文件中保持已定义状态，直到源文件中的 [#undef](../preprocessor-directives/preprocessor-undef.md) 指令删除该定义。 使用 -define 选项时，一个文件中的 `#undef` 指令不影响项目中其他的源代码文件。  
  
 可以将由此选项创建的符号同 [#if](../preprocessor-directives/preprocessor-if.md)、[#else](../preprocessor-directives/preprocessor-else.md)、[#elif](../preprocessor-directives/preprocessor-elif.md) 和 [#endif](../preprocessor-directives/preprocessor-endif.md) 一起使用，对源文件进行条件编译。  
  
 -d 是 -define 的缩写形式。  
  
 通过使用分号或逗号分隔符号名称，可用 -define 定义多个符号。 例如：  
  
```console  
-define:DEBUG;TUESDAY  
```  
  
 C# 编译器本身不定义源代码中使用的符号或宏；所有符号定义必须都是用户定义的。  
  
> [!NOTE]
> 同 C++ 等语言一样，C# `#define` 不允许为符号赋值。 例如，`#define` 不能用于创建宏或定义常数。 如果需要定义一个常数，请使用 `enum` 变量。 若要创建 C++ 风格的宏，请考虑泛型等替代项。 由于宏非常容易出错，所以 C# 不允许使用宏，但提供了更安全的替代项。  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>在 Visual Studio 开发环境中设置此编译器选项  
  
1. 打开项目的“属性”页。  
  
2. 在“生成”选项卡上，键入要在“条件编译符号”框中定义的符号。 例如，如果使用以下代码示例，只需在文本框中键入 `xx`。  
  
 有关如何以编程方式设置此编译器选项的信息，请参阅 <xref:VSLangProj80.CSharpProjectConfigurationProperties3.DefineConstants%2A>。  
  
## <a name="example"></a>示例  
  
```csharp  
// preprocessor_define.cs  
// compile with: -define:xx  
// or uncomment the next line  
// #define xx  
using System;  
public class Test
{  
    public static void Main()
    {  
        #if (xx)
            Console.WriteLine("xx defined");  
        #else  
            Console.WriteLine("xx not defined");  
        #endif  
    }  
}  
```  
  
## <a name="see-also"></a>另请参阅

- [C# 编译器选项](./index.md)
- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
