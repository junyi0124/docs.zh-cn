---
description: -publicsign（C# 编译器选项）
title: -publicsign（C# 编译器选项）
ms.date: 05/15/2018
f1_keywords:
- /publicsign
helpviewer_keywords:
- -publicsign compiler option [C#]
- publicsign compiler option [C#]
- /publicsign compiler option [C#]
ms.openlocfilehash: 525912c7f50aa6b51e602be7b9258f1f5907daac
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89124807"
---
# <a name="-publicsign-c-compiler-options"></a>-publicsign（C# 编译器选项）

此选项会导致编译器应用公钥，但不会实际对程序集签名。  -publicsign 选项还会在程序集中设置位，以告知运行时该文件实际已签名。

## <a name="syntax"></a>语法

```console
-publicsign
```

## <a name="arguments"></a>参数

无。

## <a name="remarks"></a>备注

 -publicsign 选项需要使用 [-keyfile](keyfile-compiler-option.md) 或 [-keycontainer](keycontainer-compiler-option.md)。  keyfile 或  keycontainer 选项指定公钥。

 -publicsign 和 -delaysign  选项互斥。

公共签名有时称为“假签名”或“OSS 签名”，它包括输出程序集中的公钥并设置“已签名”标记，但实际上并未使用私钥对程序集进行签名。 这对开放源代码项目非常有用，人们希望生成与已发布的“完全签名”程序集兼容的程序集，但无权访问用于对程序集进行签名的私钥。 由于几乎没有使用者实际需要检查程序集是否完全签名，因此这些公开生成的程序集几乎适用于每个使用完全签名程序集的方案。

### <a name="to-set-this-compiler-option-in-a-csproj-file"></a>在 csproj 文件中设置此编译器选项

打开项目的 csproj 文件，并添加以下元素：

```xml
<PublicSign>true</PublicSign>
```

## <a name="see-also"></a>另请参阅

- [C# 编译器 -delaysign 选项](delaysign-compiler-option.md)
- [C# 编译器 -keyfile 选项](keyfile-compiler-option.md)
- [C# 编译器 -keycontainer 选项](keycontainer-compiler-option.md)
- [C# 编译器选项](index.md)
- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
