---
title: -vbruntime
ms.date: 03/13/2018
f1_keywords:
- vbruntime
- -vbruntime
helpviewer_keywords:
- vbruntime compiler option [Visual Basic]
- -vbruntime compiler option [Visual Basic]
- /vbruntime compiler option [Visual Basic]
ms.assetid: 1aa0239e-511a-4c29-957d-fd72877b350a
ms.openlocfilehash: 46d4b095d4a2bf9aac9124d5d4b2a09adb347ba1
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91085056"
---
# <a name="-vbruntime"></a>-vbruntime

指定编译器应在不引用 Visual Basic 运行库的情况下进行编译，或在引用特定运行库的情况下进行编译。  
  
## <a name="syntax"></a>语法  
  
```console  
-vbruntime:{ - | + | * | path }  
```  
  
## <a name="arguments"></a>自变量  

 \-  
 在不引用 Visual Basic 运行时库的情况下进行编译。  
  
 \+  
 在引用默认 Visual Basic 运行时库的情况下进行编译。  
  
 \*  
 在不引用 Visual Basic 运行时库的情况下进行编译，并将 Visual Basic 运行时库中的核心功能嵌入到程序集中。  
  
 `path`  
 在引用指定库 (DLL) 的情况下进行编译。  
  
## <a name="remarks"></a>备注  

 通过 `-vbruntime` 编译器选项可以指定应在不引用 Visual Basic 运行时库的情况下进行编译。 如果在不引用 Visual Basic 运行时库的情况下进行编译，则会对生成 Visual Basic 运行时帮助程序调用的代码或语言构造记录错误或警告。 （*Visual Basic 运行时帮助程序*是 Microsoft.VisualBasic.dll 中定义的一个函数，在运行时调用以执行特定语言语义。）  
  
 `-vbruntime+` 选项会生成在未指定 `-vbruntime` 开关时发生的相同行为。 可以使用 `-vbruntime+` 选项重写以前的 `-vbruntime` 开关。  
  
 使用 `-vbruntime-` 或 `-vbruntime:path` 选项时，`My` 类型的大多数对象不可用。  
  
## <a name="embedding-visual-basic-runtime-core-functionality"></a>嵌入 Visual Basic 运行时核心功能  

 通过 `-vbruntime*` 选项可以在不引用运行时库的情况下进行编译。 而是将 Visual Basic 运行时库中的核心功能嵌入在用户程序集中。 如果应用程序在不包含 Visual Basic 运行时的平台上运行，则可以使用此选项。  
  
 会嵌入以下运行时成员：  
  
- <xref:Microsoft.VisualBasic.CompilerServices.Conversions> 类  
  
- <xref:Microsoft.VisualBasic.Strings.AscW%28System.Char%29> 方法  
  
- <xref:Microsoft.VisualBasic.Strings.AscW%28System.String%29> 方法  
  
- <xref:Microsoft.VisualBasic.Strings.ChrW%28System.Int32%29> 方法  
  
- <xref:Microsoft.VisualBasic.Constants.vbBack> 常数  
  
- <xref:Microsoft.VisualBasic.Constants.vbCr> 常数  
  
- <xref:Microsoft.VisualBasic.Constants.vbCrLf> 常数  
  
- <xref:Microsoft.VisualBasic.Constants.vbFormFeed> 常数  
  
- <xref:Microsoft.VisualBasic.Constants.vbLf> 常数  
  
- <xref:Microsoft.VisualBasic.Constants.vbNewLine> 常数  
  
- <xref:Microsoft.VisualBasic.Constants.vbNullChar> 常数  
  
- <xref:Microsoft.VisualBasic.Constants.vbNullString> 常数  
  
- <xref:Microsoft.VisualBasic.Constants.vbTab> 常数  
  
- <xref:Microsoft.VisualBasic.Constants.vbVerticalTab> 常数  
  
- `My` 类型的某些对象  
  
 如果使用 `-vbruntime*` 选项进行编译，并且代码从没有嵌入核心功能的 Visual Basic 运行时库中引用了一个成员，则编译器会返回错误，指示该成员不可用。  
  
## <a name="referencing-a-specified-library"></a>引用指定库  

 可以使用 `path` 参数在引用自定义运行时库（而不是默认 Visual Basic 运行时库）的情况下进行编译。  
  
 如果 `path` 参数的值是 DLL 的完全限定路径，则编译器将使用该文件作为运行时库。 如果 `path` 参数的值不是 DLL 的完全限定路径，则 Visual Basic 编译器将首先在当前文件夹中搜索标识的 DLL。 随后它将在使用 [-sdkpath](sdkpath.md) 编译器选项指定的路径中进行搜索。 如果未使用 `-sdkpath` 编译器选项，则编译器将在 .NET Framework 文件夹 (`%systemroot%\Microsoft.NET\Framework\versionNumber`) 中搜索标识的 DLL。  
  
## <a name="example"></a>示例  

 下面的示例演示如何使用 `-vbruntime` 选项在引用自定义库的情况下进行编译。  
  
```console
vbc -vbruntime:C:\VBLibraries\CustomVBLibrary.dll  
```  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 核心 – Visual Studio 2010 SP1 中的新编译模式](https://devblogs.microsoft.com/vbteam/vb-core-new-compilation-mode-in-visual-studio-2010-sp1/)
- [Visual Basic 命令行编译器](index.md)
- [示例编译命令行](sample-compilation-command-lines.md)
- [-sdkpath](sdkpath.md)
