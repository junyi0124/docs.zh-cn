---
title: '#ExternalSource 指令'
ms.date: 07/20/2015
f1_keywords:
- '#Externalsource'
- '#ExternalSource'
- vb.ExternalSource
- Externalsource
- vb.#ExternalSource
- ExternalSource
helpviewer_keywords:
- ExternalSource directive (#ExternalSource)
- '#ExternalSource directive'
ms.assetid: 243bc6a2-34c3-4eeb-a776-9fd2bf988149
ms.openlocfilehash: e4c7704c32c3a6c73e069d0b7129d5386696b438
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84402987"
---
# <a name="externalsource-directive"></a>#ExternalSource 指令

指示源代码的特定行与源外部的文本之间的映射。  
  
## <a name="syntax"></a>语法  
  
```vb  
#ExternalSource( StringLiteral , IntLiteral )  
    [ LogicalLine+ ]  
#End ExternalSource  
```  
  
## <a name="parts"></a>组成部分  

 `StringLiteral`  
 外部源的路径。  
  
 `IntLiteral`  
 外部源的第一行的行号。  
  
 `LogicalLine`  
 外部源中发生错误的行。  
  
 `#End ExternalSource`  
 终止 `#ExternalSource` 块。  
  
## <a name="remarks"></a>备注  

 此指令仅由编译器和调试器使用。  
  
 源文件可能包含外部源指令，这表示源文件中特定代码行与源外部的文本之间的映射，如 .aspx 文件。 如果编译过程中在指定的源代码中遇到错误，则会将它们标识为来自外部源。  
  
 外部源指令对编译不起作用，因此不能嵌套。 它们仅供应用程序内部使用。  
  
## <a name="see-also"></a>另请参阅

- [条件编译](../../programming-guide/program-structure/conditional-compilation.md)
