---
title: 如何：将数据移入和移出变量
ms.date: 07/20/2015
helpviewer_keywords:
- variables [Visual Basic], retrieving values
- variables [Visual Basic], storing data
ms.assetid: 93744f46-bf78-4fa0-9640-1de01bc38d9a
ms.openlocfilehash: fe19a6160623aa9ea867becdf7a15b51319abf45
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410433"
---
# <a name="how-to-move-data-into-and-out-of-a-variable-visual-basic"></a>如何：将数据移入和移出变量 (Visual Basic)

您可以通过将变量名称放在赋值语句的左侧来将值存储在变量中。

## <a name="putting-data-in-a-variable"></a>将数据放入变量

#### <a name="to-store-a-value-in-a-variable"></a>将值存储在变量中

- 使用赋值语句左侧的变量名称。

    下面的示例设置变量的值 `alpha` 。

    ```vb
    alpha = (beta * 6.27) / (gamma + 2.1)
    ```

    赋值语句右侧生成的值存储在变量中。

## <a name="getting-data-from-a-variable"></a>从变量中获取数据

通过在表达式中包含变量名称来检索变量的值。

#### <a name="to-retrieve-a-value-from-a-variable"></a>从变量中检索值

- 在表达式中使用变量名。 可以在可以使用常量或文本的任何位置使用变量，但定义常量值的表达式除外。

  \- 或 -

- 在赋值语句中使用等号（）后面的变量名称 `=` 。

  下面的示例读取变量的值 `startValue` ，然后 `counter` 在表达式中使用该变量的值。

  ```vb
  counter = startValue
  cellValue = (counter + 5) ^ 2
  ```

  变量的值将作为常数加入表达式，然后将其存储在赋值语句左侧的变量或属性中。

## <a name="see-also"></a>另请参阅

- [变量](index.md)
- [变量声明](variable-declaration.md)
- [对象变量](object-variables.md)
