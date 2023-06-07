---
title: 如何：通过使用程序集执行 XSLT 转换
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 76ee440b-d134-4f8f-8262-b917ad6dcbf6
ms.openlocfilehash: 64ae2ecf4dac15170115e232fca0ddee272afd0f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722695"
---
# <a name="how-to-perform-an-xslt-transformation-by-using-an-assembly"></a>如何：通过使用程序集执行 XSLT 转换

XSLT 编译器 (xsltc.exe) 编译 XSLT 样式表并生成一个程序集。 可以将该程序集直接传递到 <xref:System.Xml.Xsl.XslCompiledTransform.Load%28System.Type%29?displayProperty=nameWithType> 方法中。  
  
### <a name="to-copy-the-xml-and-xslt-files-to-your-local-computer"></a>将 XML 和 XSLT 文件复制到本地计算机  
  
- 将 XSLT 文件复制到本地计算机并将其命名为 Transform.xsl。  
  
    ```xml  
    <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform"  
      xmlns:msxsl="urn:schemas-microsoft-com:xslt"  
      xmlns:user="urn:my-scripts">  
      <msxsl:script language="C#" implements-prefix="user">  
        <![CDATA[  
      public string discount(string price){  
        char[] trimChars = { '$' };  
        //trim leading $, convert price to type double  
        double discount_value = Convert.ToDouble(price.TrimStart(trimChars));  
        //apply 10% discount and round appropriately  
        discount_value = .9*discount_value;  
        //convert value to decimal and format as currency  
        string discount_price = discount_value.ToString("C");  
        return discount_price;  
      }  
      ]]>  
      </msxsl:script>  
      <xsl:template match="catalog">  
        <html>  
          <head></head>  
          <body>  
            <table border="1">  
              <tr>  
                <th align="left">Title</th>  
                <th align="left">Author</th>  
                <th align="left">Genre</th>  
                <th align="left">Publish Date</th>  
                <th align="left">Price</th>  
              </tr>  
              <xsl:for-each select="book">  
                <tr>  
                  <td>  
                    <xsl:value-of select="title"/>  
                  </td>  
                  <td>  
                    <xsl:value-of select="author"/>  
                  </td>  
                  <td>  
                    <xsl:value-of select="genre"/>  
                  </td>  
                  <td>  
                    <xsl:value-of select="publish_date"/>  
                  </td>  
                  <xsl:choose>  
                    <xsl:when test="genre = 'Fantasy'">  
                      <td>  
                        <xsl:value-of select="user:discount(price)"/>  
                      </td>  
                    </xsl:when>  
                    <xsl:otherwise>  
                      <td>  
                        <xsl:value-of select="price"/>  
                      </td>  
                    </xsl:otherwise>  
                  </xsl:choose>  
                </tr>  
              </xsl:for-each>  
            </table>  
          </body>  
        </html>  
      </xsl:template>  
    </xsl:stylesheet>  
    ```  
  
- 将 XML 文件复制到本地计算机并将其命名为 `books.xml`。  
  
    ```xml  
    <?xml version="1.0"?>  
    <catalog>  
       <book id="bk101">  
          <author>Gambardella, Matthew</author>  
          <title>XML Developer's Guide</title>  
          <genre>Computer</genre>  
          <price>$44.95</price>  
          <publish_date>2000-10-01</publish_date>  
       </book>  
       <book id="bk102">  
          <author>Ralls, Kim</author>  
          <title>Midnight Rain</title>  
          <genre>Fantasy</genre>  
          <price>$5.95</price>  
          <publish_date>2000-12-16</publish_date>  
       </book>  
       <book id="bk103">  
          <author>Corets, Eva</author>  
          <title>Maeve Ascendant</title>  
          <genre>Fantasy</genre>  
          <price>$5.95</price>  
          <publish_date>2000-11-17</publish_date>  
       </book>  
       <book id="bk106">  
          <author>Randall, Cynthia</author>  
          <title>Lover Birds</title>  
          <genre>Romance</genre>  
          <price>$4.95</price>  
          <publish_date>2000-09-02</publish_date>  
       </book>  
       <book id="bk107">  
          <author>Thurman, Paula</author>  
          <title>Splish Splash</title>  
          <genre>Romance</genre>  
          <price>$4.95</price>  
          <publish_date>2000-11-02</publish_date>  
       </book>  
    </catalog>  
    ```  
  
### <a name="to-compile-the-style-sheet-with-the-script-enabled"></a>编译启用脚本的样式表。  
  
1. 从命令行执行下面的命令可创建两个名为 `Transform.dll` 和 `Transform_Script1.dll` 的程序集（这是默认行为。 除非另行指定，否则类和程序集的名称默认为主样式表的名称）：  
  
    ```console  
    xsltc /settings:script+ Transform.xsl  
    ```
  
    下面的命令将类名称显式设置为 Transform：  
  
    ```console  
    xsltc /settings:script+ /class:Transform Transform.xsl  
    ```  
  
### <a name="to-include-the-compiled-assembly-as-a-reference-when-you-compile-your-code"></a>编译代码时以引用形式包含已编译的程序集。  
  
1. 通过在解决方案资源管理器中或从命令行添加引用，可以在 Visual Studio 中包括程序集。  
  
2. 对于 C# 的命令行，使用下面的命令：  
  
    ```console  
    csc myCode.cs /r:system.dll;system.xml.dll;Transform.dll  
    ```  
  
3. 对于 Visual Basic 的命令行，使用下面的命令：  
  
    ```console  
    vbc myCode.vb /r:system.dll;system.xml.dll;Transform.dll  
    ```  
  
### <a name="to-use-the-compiled-assembly-in-your-code"></a>在代码中使用已编译的程序集。  
  
下面的示例演示如何通过使用已编译的样式表执行 XSLT 转换。  
  
[!code-csharp[XslTransform_XSLTC#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XslTransform_XSLTC/CS/XslTransform_XSLTC.cs#1)]
[!code-vb[XslTransform_XSLTC#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XslTransform_XSLTC/VB/XslTransform_XSLTC.vb#1)]  
  
若要动态链接到已编译的程序集，请在上面的示例中将
  
```csharp  
xslt.Load(typeof(Transform));  
```  
  
替换为  
  
```csharp
xslt.Load(System.Reflection.Assembly.Load("Transform").GetType("Transform"));  
```
  
。 若要详细了解 Assembly.Load 方法，请参阅 <xref:System.Reflection.Assembly.Load%2A>。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Xml.Xsl.XslCompiledTransform>
- [XSLT 编译器 (xsltc.exe)](xslt-compiler-xsltc-exe.md)
- [XSLT 转换](xslt-transformations.md)
- [在命令行上使用 csc.exe 生成](../../../csharp/language-reference/compiler-options/command-line-building-with-csc-exe.md)
