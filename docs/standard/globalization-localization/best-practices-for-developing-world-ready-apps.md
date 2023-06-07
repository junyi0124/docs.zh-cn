---
title: 开发全球通用应用程序的最佳做法
ms.date: 03/30/2017
helpviewer_keywords:
- global applications, best practices
- world-ready applications, best practices
- globalization [.NET], best practices
- international applications [.NET], best practices
ms.assetid: f08169c7-aad8-4ec3-9a21-9ebd3b89986c
ms.openlocfilehash: 3705fece48c136f73c4e31cd025e76dc570a142b
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94827134"
---
# <a name="best-practices-for-developing-world-ready-applications"></a>开发全球通用应用程序的最佳做法

本节描述在开发全球通用的应用程序时应遵循的最佳做法。

## <a name="globalization-best-practices"></a>全球化最佳做法

1. 在内部使应用程序代码成为 Unicode。

2. 使用 <xref:System.Globalization> 命名空间提供的区域性识别类来操作和格式化数据。

    - 对于排序，使用 <xref:System.Globalization.SortKey> 类和 <xref:System.Globalization.CompareInfo> 类。

    - 对于字符串比较，使用 <xref:System.Globalization.CompareInfo> 类。

    - 对于日期和时间格式化，使用 <xref:System.Globalization.DateTimeFormatInfo> 类。

    - 对于数字格式化，使用 <xref:System.Globalization.NumberFormatInfo> 类。

    - 对于公历和非公历，使用 <xref:System.Globalization.Calendar> 类或特定的日历实现之一。

3. 在适当的情况下，使用 <xref:System.Globalization.CultureInfo?displayProperty=nameWithType> 类提供的区域性属性设置。 使用 <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> 属性来执行格式化任务，如日期和时间或数字的格式化。 使用 <xref:System.Globalization.CultureInfo.CurrentUICulture%2A?displayProperty=nameWithType> 属性来检索资源。 请注意，`CurrentCulture` 和 `CurrentUICulture` 属性可以通过线程进行设置。

4. 通过使用 <xref:System.Text> 命名空间中的编码类，使应用程序能够与各种编码相互进行数据读写。 不要采用 ASCII 数据。 假定在用户可以输入文本的任何位置都将提供国际字符。 例如，应用程序应接受服务器名、目录、文件名、用户名和 URL 中的国际字符。

5. 使用 <xref:System.Text.UTF8Encoding> 类时，出于安全原因，应使用此类提供的错误检测功能。 为了打开错误检测功能，请使用具有一个 `throwOnInvalidBytes` 参数的构造函数来创建该类的一个实例，并将该参数的值设置为 `true`。

6. 尽可能将字符串按整个字符串处理，而不是按一系列个别字符处理。 这在排序或搜索子字符串时尤为重要。 这可以防止与分析组合字符有关的问题。 还可以通过 <xref:System.Globalization.StringInfo?displayProperty=nameWithType> 类使用文本单元而不是单个字符。

7. 使用 <xref:System.Drawing> 命名空间提供的类来显示文本。

8. 为保持操作系统间的一致性，不要允许用户设置重写 <xref:System.Globalization.CultureInfo>。 使用接受 `CultureInfo` 参数的 `useUserOverride` 构造函数并将此参数设置为 `false`。

9. 在国际操作系统版本上使用国际数据来测试应用程序功能。

10. 如果安全决策基于字符串比较或大小写更改操作的结果，请使用不区分区域性的字符串操作。 这种做法可确保结果不会受 `CultureInfo.CurrentCulture` 值的影响。 有关展示了区分区域性字符串比较如何产生不一致结果的示例，请参阅[字符串使用最佳做法](../base-types/best-practices-strings.md)的[“使用当前区域性的字符串比较”](../base-types/best-practices-strings.md#string-comparisons-that-use-the-current-culture)部分。

## <a name="localization-best-practices"></a>本地化最佳做法

1. 将所有可本地化的资源移动到单独的纯资源 DLL 中。 可本地化的资源包括用户界面元素，如字符串、错误消息、对话框、菜单以及嵌入的对象资源。

2. 不要对字符串或用户界面资源进行硬编码。

3. 不要将不可本地化的资源放在纯资源 DLL 中。 否则会使翻译人员产生困惑。

4. 不要使用在运行时从串联词组生成的复合字符串。 复合字符串难以本地化，因为它们往往采用英语语法顺序，而此顺序并不适用于所有语言。

5. 避免不明确的结构，如“Empty Folder”，因为根据字符串组成部分的语法规则，这些字符串可能产生不同的翻译。 例如，“empty”既可以是一个动词，也可以是一个形容词，因此在诸如意大利语或法语等语言中就可能导致不同的翻译。

6. 避免在应用程序中使用包含文本的图像和图标。 本地化这些图像和图标的成本是很大的。

7. 允许在用户界面中为字符串长度的扩展保留足够的空间。 在某些语言中，词组所需的空间可能比在其他语言中多 50-75%。

8. 使用 <xref:System.Resources.ResourceManager?displayProperty=nameWithType> 类来根据区域性检索资源。

9. 使用 [Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) 创建 Windows 窗体对话框，以便可以使用 [Windows 窗体资源编辑器 (Winres.exe)](../../framework/tools/winres-exe-windows-forms-resource-editor.md) 对它们进行本地化。 不要对 Windows 窗体对话框进行手动编码。

10. 安排进行专业本地化工作（翻译）。

11. 有关创建并本地化资源的完整说明，请参阅[应用中的资源](../../framework/resources/index.md)。

## <a name="globalization-best-practices-for-aspnet-applications"></a>ASP.NET 应用程序的全球化最佳做法

1. 在应用程序中显式设置 <xref:System.Globalization.CultureInfo.CurrentUICulture%2A> 和 <xref:System.Globalization.CultureInfo.CurrentCulture%2A> 属性。 不要依赖于默认设置。

2. 请注意，ASP.NET 应用程序是托管应用程序，因此可以使用与其他托管应用程序相同的类，以根据区域性检索、显示和操作信息。

3. 注意在 ASP.NET 中可以指定以下三种编码类型：

    - requestEncoding 指定从客户端浏览器接收的编码。

    - responseEncoding 指定要发送到客户端浏览器的编码。 在大多数情形下，此编码应该与为 requestEncoding 指定的编码相同。

    - fileEncoding 指定用于 .aspx、.asmx 和 .asax 文件分析的默认编码。

4. 在 ASP.NET 应用程序中的以下三个位置指定 requestEncoding、responseEncoding、fileEncoding、culture 和 uiCulture 特性的值：

    - 在 Web.config 文件的全球化一节中。 此文件是 ASP.NET 应用程序的外部文件。 有关详细信息，请参阅 [\<globalization> 元素](/previous-versions/dotnet/netframework-4.0/hy4kkhe0(v=vs.100))。

    - 在页面指令中。 请注意，当应用程序在页面中时，文件已经被读取。 因此，指定 fileEncoding 和 requestEncoding 为时已晚。 只有 uiCulture、Culture 和 responseEncoding 可以在页面指令中指定。

    - 在应用程序代码中以编程方式指定。 该设置可能随请求的不同而不同。 同页面指令一样，到打开应用程序代码时，指定 fileEncoding 和 requestEncoding 为时已晚。 只有 uiCulture、Culture 和 responseEncoding 可以在应用程序代码中指定。

5. 请注意，uiCulture 值可以设置为浏览器接受的语言。

## <a name="see-also"></a>请参阅

- [全球化和本地化](index.md)
- [桌面应用中的资源](../../framework/resources/index.md)
