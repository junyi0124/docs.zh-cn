---
title: 变量使用了不支持的自动化类型
ms.date: 07/20/2015
f1_keywords:
- vbrID458
ms.assetid: bde4f4da-493b-452c-b6e4-1d370edba4cd
ms.openlocfilehash: 7d52189e31823b63547c757434847c0e1717aada
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406542"
---
# <a name="variable-uses-an-automation-type-not-supported-in-visual-basic"></a>变量使用了 Visual Basic 不支持的自动化类型

尝试使用类型库或对象库中定义的变量，该变量的数据类型不受 Visual Basic 支持。

## <a name="to-correct-this-error"></a>更正此错误

- 使用 Visual Basic 可识别的类型的变量。

     \- 或 -

- 如果你在使用或时遇到此错误 `FileGet` `FileGetObject` ，请确保你尝试使用的文件已使用 `FilePut` 或写入 `FilePutObject` 。

## <a name="see-also"></a>另请参阅

- [数据类型](../data-types/index.md)
