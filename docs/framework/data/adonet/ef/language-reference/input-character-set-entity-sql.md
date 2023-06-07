---
title: 输入字符集 (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 13d291d3-e6bc-4719-b953-758b61a590b6
ms.openlocfilehash: 94615a8f4aec51347f451d6f6a53b9d5b459a336
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91203652"
---
# <a name="input-character-set-entity-sql"></a>输入字符集 (Entity SQL)

[!INCLUDE[esql](../../../../../../includes/esql-md.md)] 接受使用 UTF-16 进行编码的 UNICODE 字符。  
  
 字符串文字可以包含用单引号括起来的任何 UTF-16 字符。 例如，N'文字列リテラル'。 比较字符串文字时，将使用原始的 UTF-16 值。 例如，N'ABC' 在日语代码页和拉丁语代码页中是不同的。  
  
 注释可包含任何 UTF-16 字符。  
  
 转义标识符可以包含用方括号括起来的任何 UTF-16 字符。 例如，[エスケープされた識別子]。 UTF-16 转义标识符的比较不区分大小写。 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 将看上去相同但来自不同代码页的字母视为不同的字符。 例如，如果 [ABC] 和 [abc] 的字符来自同一代码页，则 [ABC] 等效于 [abc]。 但如果这两个标识符来自不同的代码页，它们就不等效。  
  
 空白是任何 UTF-16 空白字符。  
  
 换行符是任何标准化的 UTF-16 换行符。 例如，'\n' 和 '\r\n' 被视为换行符，但 '\r' 不是换行符。  
  
 关键字、表达式和标点可以是任何标准化为拉丁语的 UTF-16 字符。 例如，SELECT 在日语代码页中是有效的关键字。  
  
 关键字、表达式和标点只能是拉丁语字符。 `SELECT` 在日语代码页中不是关键字。 +、-、 \* 、/、=、 (、) 、'、[，] 以及此处不带引号的任何其他语言构造只能是拉丁字符。  
  
 简单标识符只能为拉丁字符。 这可以避免比较时的不确定性，因为比较时使用的是原始值。 例如，在日语和拉丁语代码页中，ABC 将有所不同。  
  
## <a name="see-also"></a>请参阅

- [Entity SQL 概述](entity-sql-overview.md)
