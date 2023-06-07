---
title: -optioncompare
ms.date: 07/20/2015
f1_keywords:
- /optioncompare
- -optioncompare
helpviewer_keywords:
- optioncompare compiler option [Visual Basic]
- -optioncompare compiler option [Visual Basic]
- /optioncompare compiler option [Visual Basic]
ms.assetid: 7237b766-b44d-4cc5-9a3c-885348a7d9e4
ms.openlocfilehash: ed9adc7cddd9eb204937b9819e4eeff176821e95
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400548"
---
# <a name="-optioncompare"></a>-optioncompare

指定如何进行字符串比较。

## <a name="syntax"></a>语法

```console
-optioncompare:{binary | text}
```

## <a name="remarks"></a>备注

可以使用以下两种形式之一指定 `-optioncompare`：使用二进制字符串比较的 `-optioncompare:binary`，和使用文本字符串比较的 `-optioncompare:text`。 默认情况下，编译器使用 `-optioncompare:binary`。

在 Microsoft Windows 中，当前代码页确定二进制排序顺序。 典型的二进制排序顺序如下所示：

`A < B < E < Z < a < b < e < z < À < Ê < Ø < à < ê < ø`

基于由系统的区域设置确定的不区分大小写的文本排序顺序对基于文本的字符串进行比较。 典型的文本排序顺序如下所示：

`(A = a) < (À = à) < (B=b) < (E=e) < (Ê = ê) < (Z=z) < (Ø = ø)`

### <a name="to-set--optioncompare-in-the-visual-studio-ide"></a>若要在 Visual Studio IDE 中设置 -optioncompare

1. 在 **“解决方案资源管理器”** 中选择一个项目。 在“项目”菜单上，单击“属性”   。

2. 单击“编译”  选项卡。

3. 修改“Option Compare”  框中的值。

### <a name="to-set--optioncompare-programmatically"></a>以编程方式设置 -optioncompare

请参阅 [Option Compare 语句](../../language-reference/statements/option-compare-statement.md)。

## <a name="example"></a>示例

下面的代码编译 `ProjFile.vb`，并使用二进制字符串比较。

```console
vbc -optioncompare:binary projFile.vb
```

## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [-optionexplicit](optionexplicit.md)
- [-optionstrict](optionstrict.md)
- [-optioninfer](optioninfer.md)
- [示例编译命令行](sample-compilation-command-lines.md)
- [Option Compare 语句](../../language-reference/statements/option-compare-statement.md)
- [“选项”对话框 ->“项目”->“Visual Basic 默认值”](/visualstudio/ide/reference/visual-basic-defaults-projects-options-dialog-box)
