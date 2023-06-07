---
title: -keycontainer
ms.date: 03/10/2018
helpviewer_keywords:
- -keycontainer compiler option [Visual Basic]
- keycontainer compiler option [Visual Basic]
- /keycontainer compiler option [Visual Basic]
ms.assetid: 6a9bc861-1752-4db1-9f64-b5252f0482cc
ms.openlocfilehash: 243583e55dcf278f951b813cca8384246d2d6db9
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91085225"
---
# <a name="-keycontainer"></a>-keycontainer

指定密钥对的密钥容器名称从而为程序集赋予强名称。  
  
## <a name="syntax"></a>语法  
  
```console  
-keycontainer:container  
```  
  
## <a name="arguments"></a>自变量  
  
|术语|定义|  
|---|---|  
|`container`|必需。 包含密钥的容器文件。 如果文件名包含空格，则将它括在引号 ("") 内。|  
  
## <a name="remarks"></a>备注  

 通过将公钥插入到程序集清单并且用私钥签名最终程序集，编译器可创建可共享的组件。 若要生成密钥文件，请在命令行键入 `sn -k file`。 `-i` 选项将密钥对安装到容器中。 有关详细信息，请参阅 [Sn.exe（强名称工具）](../../../framework/tools/sn-exe-strong-name-tool.md)。  
  
 如果使用 `-target:module` 进行编译，密钥文件的名称将保存在模块中，并在使用 [-addmodule](addmodule.md) 编译程序集时包含到创建的程序集中。  
  
 还可以将此选项指定为任何 Microsoft 中间语言 (MSIL) 模块的源代码中的自定义特性 (<xref:System.Reflection.AssemblyKeyNameAttribute>)。  
  
 此外，可使用 [-keyfile](keyfile.md) 将加密信息传递给编译器。 如果需要部分签名的程序集，请使用 [-delaysign](delaysign.md)。  
  
 有关对程序集签名的详细信息，请参阅[创建和使用具有强名称的程序集](../../../standard/assembly/create-use-strong-named.md)。  
  
> [!NOTE]
> Visual Studio 开发环境内无法使用 `-keycontainer` 选项；仅当从命令行编译时才可用。  
  
## <a name="example"></a>示例  

 下面的代码编译源文件 `Input.vb`，并指定一个密钥容器。  
  
```console  
vbc -keycontainer:key1 input.vb  
```  
  
## <a name="see-also"></a>请参阅

- [.NET 中的程序集](../../../standard/assembly/index.md)
- [Visual Basic 命令行编译器](index.md)
- [-keyfile](keyfile.md)
- [示例编译命令行](sample-compilation-command-lines.md)
