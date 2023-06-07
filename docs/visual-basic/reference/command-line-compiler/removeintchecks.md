---
title: -removeintchecks
ms.date: 03/13/2018
f1_keywords:
- removeintchecks
- -removeintchecks
helpviewer_keywords:
- removeintchecks compiler option [Visual Basic]
- /removeintchecks compiler option [Visual Basic]
- -removeintchecks compiler option [Visual Basic]
ms.assetid: c1835bd5-1e38-4fba-bd2f-6984774765d4
ms.openlocfilehash: ce1f24f25ea58cb6ddc2f5c582b6103d8f18d922
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91085160"
---
# <a name="-removeintchecks"></a>-removeintchecks

打开或关闭对整数运算的溢出错误检查。  
  
## <a name="syntax"></a>语法  
  
```console  
-removeintchecks[+ | -]  
```  
  
## <a name="arguments"></a>自变量  
  
|术语|定义|  
|---|---|  
|`+` &#124; `-`|可选。 `-removeintchecks-` 选项会使编译器检查所有整数计算是否存在溢出错误。 默认值为 `-removeintchecks-`。<br /><br /> 指定 `-removeintchecks` 或 `-removeintchecks+` 可阻止错误检查并可以使整数计算更快。 但是，不进行错误检查时，如果数据类型容量溢出，则可能会存储不正确的结果，而不会引发错误。|  
  
|若要在 Visual Studio 集成开发环境中设置 -removeintchecks|  
|---|  
|1.在 **“解决方案资源管理器”** 中选择一个项目。 在“项目”菜单上，单击“属性”   。 <br />2.单击“编译”  选项卡。<br />3.单击“高级”  按钮。<br />4.修改“不做整数溢出检查”  框的值。|  
  
## <a name="example"></a>示例  

 下面的代码编译 `Test.vb` 并关闭整数溢出错误检查。  
  
```console
vbc -removeintchecks+ test.vb  
```  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [示例编译命令行](sample-compilation-command-lines.md)
