---
title: -reference
ms.date: 03/13/2018
helpviewer_keywords:
- /reference compiler option [Visual Basic]
- r compiler option [Visual Basic]
- -reference compiler option [Visual Basic]
- /r compiler option [Visual Basic]
- reference compiler option [Visual Basic]
- -r compiler option [Visual Basic]
ms.assetid: 66bdfced-bbf6-43d1-a554-bc0990315737
ms.openlocfilehash: b489a164e56a5e3bdbf7e3cdf24ec330fadedf38
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91097548"
---
# <a name="-reference-visual-basic"></a>-reference (Visual Basic)

使编译器让指定程序集中的类型信息可供当前正在编译的项目使用。  
  
## <a name="syntax"></a>语法  
  
```console  
-reference:fileList  
```

or

```console
-r:fileList  
```  
  
## <a name="arguments"></a>自变量  
  
|术语|定义|  
|---|---|  
|`fileList`|必需。 程序集文件名的逗号分隔列表。 如果文件名包含空格，则将名称括在引号内。|  
  
## <a name="remarks"></a>备注  

 导入的文件必须包含程序集元数据。 仅公共类型在程序集外部可见。 [-addmodule](addmodule.md) 选项从模块导入元数据。  
  
 如果引用的程序集（程序集 A）引用了另一个程序集（程序集 B），那么在下列情况下需要引用程序集 B：  
  
- 程序集 A 中的类型继承自程序集 B 中的类型或实现程序集 B 中的接口。  
  
- 调用具有程序集 B 中的返回类型或参数类型的字段、属性、事件或方法。  
  
 使用 [-libpath](libpath.md) 指定一个或多个程序集引用所在的目录。  
  
 为了使编译器能够识别程序集（而非模块）中的类型，必须强制其解析该类型。 如何执行此操作的一个示例是定义类型的实例。 还可以使用其他方法来为编译器解析程序集中的类型名称。 例如，如果从程序集中的类型继承，则编译器将知道类型名称。  
  
 默认情况下使用 Vbc.rsp 响应文件，该文件引用常用的 .NET Framework 程序集。 如果希望编译器不要使用 Vbc.rsp，请使用 `-noconfig`。  
  
 `-reference` 的缩写形式是 `-r`。  
  
## <a name="example"></a>示例  

 下面的命令编译源文件 `Input.vb` 并引用来自 `Metad1.dll` 和 `Metad2.dll` 的程序集以生成 `Out.exe`。  
  
```console
vbc -reference:metad1.dll,metad2.dll -out:out.exe input.vb  
```  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [-noconfig](noconfig.md)
- [-target (Visual Basic)](target.md)
- [Public](../../language-reference/modifiers/public.md)
- [示例编译命令行](sample-compilation-command-lines.md)
