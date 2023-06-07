---
title: 在 MaskedTextBox 控件中使用正则表达式
ms.date: 07/20/2015
helpviewer_keywords:
- strings [Visual Basic], regular expressions
- strings [Visual Basic], masked edit
ms.assetid: 2a048fb0-7053-487d-b2c5-ffa5e22ed6f9
ms.openlocfilehash: 493da7b8583b5cc73a9832afa81b7b1d84742f2d
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91072426"
---
# <a name="using-regular-expressions-with-the-maskedtextbox-control-in-visual-basic"></a>在 MaskedTextBox 控件中使用正则表达式 (Visual Basic)

此示例演示如何转换简单正则表达式以使用 <xref:System.Windows.Forms.MaskedTextBox> 控件。  
  
## <a name="description-of-the-masking-language"></a>掩码语言说明  

 标准 <xref:System.Windows.Forms.MaskedTextBox> 掩码语言基于 `Masked Edit` Visual Basic 6.0 中控件使用的语言，应熟悉从该平台迁移的用户。  
  
 <xref:System.Windows.Forms.MaskedTextBox.Mask%2A>控件的属性 <xref:System.Windows.Forms.MaskedTextBox> 指定要使用的输入掩码。 掩码必须是由下表中的一个或多个屏蔽元素组成的字符串。  
  
|屏蔽元素|说明|正则表达式元素|  
|---------------------|-----------------|--------------------------------|  
|0|0和9之间的任何一个数字。 需要输入。|\d|  
|9|数字或空格。 输入可选。|[\d]？|  
|#|数字或空格。 输入可选。 如果此位置在掩码中保留为空白，则将呈现为空格。 此外，还允许 (+) 和减号 (-) 符号。|[\d +-]？|  
|L|ASCII 字符。 需要输入。|[a zA-Z]|  
|?|ASCII 字符。 输入可选。|[a zA-Z]？|  
|&|字符。 需要输入。|[\p{Ll}\p{Lu}\p{Lt}\p{Lm}\p{Lo}]|  
|C|字符。 输入可选。|[\p{Ll}\p{Lu}\p{Lt}\p{Lm}\p{Lo}]?|  
|A|字符. 输入可选。|\W|  
|.|区域性适当的小数点占位符。|不可用。|  
|,|区域性适当的千位占位符。|不可用。|  
|:|区域性适当的时间分隔符。|不可用。|  
|/|区域性相应的日期分隔符。|不可用。|  
|$|区域性相应的货币符号。|不可用。|  
|\<|将后面的所有字符转换为小写。|不可用。|  
|>|将后面的所有字符转换为大写。|不可用。|  
|&#124;|撤消上一个 shift 或向下移动。|不可用。|  
|&#92;|转义掩码字符，并将其转换为文本。 " \\ \\ " 是反斜杠的转义序列。|&#92;|  
|所有其他字符。|文字。 所有非掩码元素都将作为自身显示在中 <xref:System.Windows.Forms.MaskedTextBox> 。|所有其他字符。|  
  
 Decimal ( ) ，千位数 (，) ，time (： ) 、date (/) 和 currency ($) 符号默认为按应用程序的区域性定义的方式显示这些符号。 可以通过使用属性强制它们显示另一区域性的符号 <xref:System.Windows.Forms.MaskedTextBox.FormatProvider%2A> 。  
  
## <a name="regular-expressions-and-masks"></a>正则表达式和掩码  

 尽管可以使用正则表达式和掩码来验证用户输入，但它们并不完全等效。 正则表达式可以表达比掩码更复杂的模式，但是掩码可以更简洁地以与区域性相关的格式表达相同的信息。  
  
 下表比较了四个正则表达式和每个正则表达式的等效掩码。  
  
|Regular Expression|Mask|备注|  
|------------------------|----------|-----------|  
|`\d{2}/\d{2}/\d{4}`|`00/00/0000`|`/`掩码中的字符是一个逻辑日期分隔符，并向用户显示与应用程序的当前区域性相对应的日期分隔符。|  
|`\d{2}-[A-Z][a-z]{2}-\d{4}`|`00->L<LL-0000`|美国格式的日期 (日、月份缩写和年份) ，其中，三个字母的月份缩写以后跟两个小写字母的前大写字母显示。|  
|`(\(\d{3}\)-)?\d{3}-d{4}`|`(999)-000-0000`|美国的电话号码，可选择的区域代码。 如果用户不希望输入可选字符，则可以输入空格，或者将鼠标指针直接置于掩码中由前0表示的位置。|  
|`$\d{6}.00`|`$999,999.00`|介于0到999999之间的货币值。 在运行时，将用其特定于区域性的等效项替换货币、千位和十进制字符。|  
  
## <a name="see-also"></a>请参阅

- <xref:System.Windows.Forms.MaskedTextBox.Mask%2A>
- <xref:System.Windows.Forms.MaskedTextBox>
- [验证字符串 (Visual Basic)](validating-strings.md)
- [MaskedTextBox 控件](/dotnet/desktop/winforms/controls/maskedtextbox-control-windows-forms)
