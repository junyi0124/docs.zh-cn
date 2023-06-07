---
description: -warn（C# 编译器选项）
title: -warn（C# 编译器选项）
ms.date: 07/20/2015
f1_keywords:
- /warn
helpviewer_keywords:
- warning level [C#]
- /w compiler option [C#]
- -w compiler option [C#]
- -warn compiler option [C#]
- /warn compiler option [C#]
- w compiler option [C#]
- warn compiler option [C#]
ms.custom: updateeachrelease
ms.assetid: 5f80ff59-4991-4382-9f9a-77da18446e71
ms.openlocfilehash: d59274423e6f9844d3ab22f3ac513ba1a05d7f07
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91171346"
---
# <a name="-warn-c-compiler-options"></a>-warn（C# 编译器选项）

-warn 选项指定编译器显示的警告等级。  
  
## <a name="syntax"></a>语法  
  
```console  
-warn:option  
```  
  
## <a name="arguments"></a>自变量  

 `option`  
 想要为编译显示的警告等级：较低的数字仅显示高严重性警告；较高的数字显示更多警告。 该值必须是零或正整数：

|警告级别|含义|
|-------------------|-------------|
|0|关闭发出所有警告消息。|
|1|显示严重警告消息。|  
|2|显示等级 1 警告以及某些不太严重的警告，如有关隐藏类成员的警告。|  
|3|显示等级 2 警告以及某些不太严重的警告，如有关经常计算为 `true` 或 `false` 的表达式的警告。|  
|4（默认值）|显示所有等级 3 警告以及信息性警告。|
|5|显示等级 4 警告以及 C# 9.0 附带的编译器中的[其他警告](https://github.com/dotnet/roslyn/blob/a6013f3213c902c0973b2d371c3007217d610533/docs/compilers/CSharp/Warnversion%20Warning%20Waves.md)。|
|大于 5|任何大于 5 的值都将被视为 5。 通常，如果使用新的警告等级更新编译器，则可以输入任意大的值（例如 `9999`）以确保始终获取所有警告。|
  
## <a name="remarks"></a>备注  

 若要获取有关错误或警告的信息，可以在帮助索引中查找错误代码。 有关获取错误或警告信息的其他方法，请参阅 [C# 编译器错误](../compiler-messages/index.md)。  
  
 使用 [-warnaserror](./warnaserror-compiler-option.md) 将所有警告视为错误。 使用 [-nowarn](./nowarn-compiler-option.md) 禁用某些警告。  
  
 -w 是 -warn 的缩写形式。  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>在 Visual Studio 开发环境中设置此编译器选项  
  
1. 打开项目的“属性”页。  
  
2. 单击“生成”属性页。  
  
3. 修改警告等级属性。  
  
 有关如何以编程方式设置此编译器选项的信息，请参阅 <xref:VSLangProj80.CSharpProjectConfigurationProperties3.WarningLevel%2A>。  
  
## <a name="example"></a>示例  

 编译 `in.cs` 并使编译器仅显示等级 1 警告：  
  
```console  
csc -warn:1 in.cs  
```  
  
## <a name="see-also"></a>另请参阅

- [C# 编译器选项](./index.md)
- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
