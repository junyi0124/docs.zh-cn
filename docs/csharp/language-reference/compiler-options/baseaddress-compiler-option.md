---
description: -baseaddress（C# 编译器选项）
title: -baseaddress（C# 编译器选项）
ms.date: 07/20/2015
f1_keywords:
- /dllbase
helpviewer_keywords:
- baseaddress compiler option [C#]
- -baseaddress compiler option [C#]
- /baseaddress compiler option [C#]
ms.assetid: ce13c965-dfe4-4433-94f5-63b476e3a608
ms.openlocfilehash: 76da496f7045f12778bba273947b913be1b94e3e
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91196840"
---
# <a name="-baseaddress-c-compiler-options"></a>-baseaddress（C# 编译器选项）

通过 -baseaddress 选项可指定加载 DLL 的首选基址。 若要深入了解何时且为何要使用此选项，请参阅 [Larry Osterman 的网络日志](/archive/blogs/larryosterman/why-should-i-even-bother-to-use-dlls-in-my-system)。  
  
## <a name="syntax"></a>语法  
  
```console  
-baseaddress:address  
```  
  
## <a name="arguments"></a>自变量  

 `address`  
 DLL 的基址。 可将此地址指定为十进制数、十六进制数或八进制数。  
  
## <a name="remarks"></a>备注  

 DLL 的默认基址由 .NET 公共语言运行时设置。  
  
 请注意，此地址中的低序字将被舍入取整。 例如，如果指定 0x11110001，它将被舍入为 0x11110000。  
  
 要完成 DLL 的签名过程，请使用具有 -R 选项的 SN.EXE。  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>在 Visual Studio 开发环境中设置此编译器选项  
  
1. 打开项目的“属性”页。  
  
2. 单击“生成”属性页。  
  
3. 单击“高级”按钮。  
  
4. 修改“DLL 基址”属性。  
  
     若要以编程方式设置此编译器选项，请参阅 <xref:VSLangProj80.CSharpProjectConfigurationProperties3.BaseAddress%2A>。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Diagnostics.ProcessModule.BaseAddress%2A?displayProperty=nameWithType>
- [C# 编译器选项](./index.md)
- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
