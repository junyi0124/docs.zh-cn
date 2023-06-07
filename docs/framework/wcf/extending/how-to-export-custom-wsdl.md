---
title: 如何：导出自定义 WSDL
ms.date: 03/30/2017
ms.assetid: 5c1e4b58-b76b-472b-9635-2f80d42a0734
ms.openlocfilehash: 723121798fee3a3c0b49a7f15995bb26444836e8
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/07/2019
ms.locfileid: "70797005"
---
# <a name="how-to-export-custom-wsdl"></a>如何：导出自定义 WSDL

本主题说明如何导出自定义 WSDL 信息。 为此，我们将定义一个名为 `WsdlDocumentationAttribute` 的新代码属性，该属性将把自定义信息添加到服务所生成的 WSDL 中。

## <a name="to-export-custom-wsdl-information"></a>导出自定义 WSDL 信息

1. 实现 <xref:System.ServiceModel.Description.IWsdlExportExtension> 接口。 在实现以下任一接口的类中，都可以实现此接口：<xref:System.ServiceModel.Description.IOperationBehavior>、<xref:System.ServiceModel.Description.IContractBehavior> 或 <xref:System.ServiceModel.Description.IEndpointBehavior>。 此外，在派生自 <xref:System.ServiceModel.Channels.BindingElement> 的类中，也可以实现此接口。 本示例在实现了 <xref:System.ServiceModel.Description.IWsdlExportExtension> 的特性类中实现 <xref:System.ServiceModel.Description.IContractBehavior>。

2. <xref:System.ServiceModel.Description.IWsdlExportExtension> 定义了两个方法：<xref:System.ServiceModel.Description.IWsdlExportExtension.ExportEndpoint%28System.ServiceModel.Description.WsdlExporter%2CSystem.ServiceModel.Description.WsdlEndpointConversionContext%29> 和 <xref:System.ServiceModel.Description.IWsdlExportExtension.ExportContract%28System.ServiceModel.Description.WsdlExporter%2CSystem.ServiceModel.Description.WsdlContractConversionContext%29>。 这些方法可用于修改 <xref:System.ServiceModel.Description.WsdlContractConversionContext> 中的信息和/或向其添加更多信息。 本示例在 <xref:System.ServiceModel.Description.IWsdlExportExtension.ExportContract%28System.ServiceModel.Description.WsdlExporter%2CSystem.ServiceModel.Description.WsdlContractConversionContext%29> 方法中检索 <xref:System.ServiceModel.Description.OperationDescription> 对象的集合，然后循环访问该集合，以查找 `WsdlDocumentationAttribute`。 如果找到该属性，则提取与之关联的文本，生成一个摘要元素，然后将该摘要元素添加到操作的 `DocumentationElement` 中。

    ```csharp
    public void ExportContract(WsdlExporter exporter, WsdlContractConversionContext context)
    {
        Console.WriteLine("Inside ExportContract");
        if (context.Contract != null)
        {
            // Set the document element; if this is not done first, there is no XmlElement in the
            // DocumentElement property.
            context.WsdlPortType.Documentation = string.Empty;
            // Contract comments.
            XmlDocument owner = context.WsdlPortType.DocumentationElement.OwnerDocument;
            XmlElement summaryElement = Formatter.CreateSummaryElement(owner, this.Text);
            context.WsdlPortType.DocumentationElement.AppendChild(summaryElement);

            foreach (OperationDescription op in context.Contract.Operations)
            {
                Operation operation = context.GetOperation(op);
                object[] opAttrs = op.SyncMethod.GetCustomAttributes(typeof(WsdlDocumentationAttribute), false);
                if (opAttrs.Length == 1)
                {
                    string opComment = ((WsdlDocumentationAttribute)opAttrs[0]).Text;

                    // This.Text returns the string for the operation-level attributes.
                    // Set the doc element; if this is not done first, there is no XmlElement in the
                    // DocumentElement property.
                    operation.Documentation = String.Empty;

                    XmlDocument opOwner = operation.DocumentationElement.OwnerDocument;
                    XmlElement newSummaryElement = Formatter.CreateSummaryElement(opOwner, opComment);
                    operation.DocumentationElement.AppendChild(newSummaryElement);
                }
            }
        }
    }
    ```

## <a name="example"></a>示例

下面的代码示例演示 `WsdlDocumentationAttribute` 类的完整实现。

```csharp
public class WsdlDocumentationAttribute : Attribute, IContractBehavior, IWsdlExportExtension
{
string text;
       XmlElement customWsdlDocElement = null;
public WsdlDocumentationAttribute(string text)
{ this.text = text;}

       public WsdlDocumentationAttribute(XmlElement wsdlDocElement)
        { this.customWsdlDocElement = wsdlDocElement; }

        public XmlElement WsdlDocElement
        {
            get { return this.customWsdlDocElement; }
            set { this.customWsdlDocElement = value; }
        }
       public string Text
{
get { return this.text; }
set { this.text = value; }
}

     public void ExportContract(WsdlExporter exporter, WsdlContractConversionContext context)
{
          Console.WriteLine("Inside ExportContract");
if (context.Contract != null)
{
                // Set the document element; if this is not done first, there is no XmlElement in the
                // DocumentElement property.
                context.WsdlPortType.Documentation = string.Empty;
                // Contract comments.
                XmlDocument owner = context.WsdlPortType.DocumentationElement.OwnerDocument;
                XmlElement summaryElement = Formatter.CreateSummaryElement(owner, this.Text);
                context.WsdlPortType.DocumentationElement.AppendChild(summaryElement);

                foreach (OperationDescription op in context.Contract.Operations)
                {
                    Operation operation = context.GetOperation(op);
                    object[] opAttrs = op.SyncMethod.GetCustomAttributes(typeof(WsdlDocumentationAttribute), false);
                    if (opAttrs.Length == 1)
                    {
                        string opComment = ((WsdlDocumentationAttribute)opAttrs[0]).Text;

                        // This.Text returns the string for the operation-level attributes.
                        // Set the document element; if this is not done first, there is no XmlElement in the
                        // DocumentElement property.
                        operation.Documentation = String.Empty;

                        XmlDocument opOwner = operation.DocumentationElement.OwnerDocument;
                        XmlElement newSummaryElement = Formatter.CreateSummaryElement(opOwner, opComment);
                        operation.DocumentationElement.AppendChild(newSummaryElement);
                    }
                }
            }
        }

public void ExportEndpoint(WsdlExporter exporter, WsdlEndpointConversionContext context)
        {
            Console.WriteLine("ExportEndpoint called.");
        }

        public void AddBindingParameters(ContractDescription description, ServiceEndpoint endpoint, BindingParameterCollection parameters)
        { return; }

        public void ApplyClientBehavior(ContractDescription description, ServiceEndpoint endpoint, ClientRuntime client)
        { return; }

        public void ApplyDispatchBehavior(ContractDescription description, ServiceEndpoint endpoint, DispatchRuntime dispatch)
        { return; }

        public void Validate(ContractDescription description, ServiceEndpoint endpoint) { return; }
    }

  public class Formatter
  {

#region Utility Functions

    public static XmlElement CreateSummaryElement(XmlDocument owningDoc, string text)
    {
      XmlElement summaryElement = owningDoc.CreateElement("summary");
      summaryElement.InnerText = text;
      return summaryElement;
    }

public static CodeCommentStatementCollection FormatComments(string text)
{
      /*
       * Note that in Visual C# the XML comment format absorbs a
       * documentation element with a line break in the middle. This sample
       * could take an XmlElement and create code comments in which
       * the element never had a line break in it.
      */

      CodeCommentStatementCollection collection = new CodeCommentStatementCollection();
collection.Add(new CodeCommentStatement("From WsdlDocumentation:", true));
collection.Add(new CodeCommentStatement(String.Empty, true));

foreach (string line in WordWrap(text, 80))
{
collection.Add(new CodeCommentStatement(line, true));
}

collection.Add(new CodeCommentStatement(String.Empty, true));
return collection;
}

public static Collection<string> WordWrap(string text, int columnWidth)
{
Collection<string> lines = new Collection<string>();
System.Text.StringBuilder builder = new System.Text.StringBuilder();

string[] words = text.Split(' ');
foreach (string word in words)
{
if ((builder.Length > 0) && ((builder.Length + word.Length + 1) > columnWidth))
{
lines.Add(builder.ToString());
builder = new System.Text.StringBuilder();
}
builder.Append(word);
builder.Append(' ');
}
lines.Add(builder.ToString());

return lines;
}

#endregion

    public static XmlElement CreateReturnsElement(XmlDocument owner, string p)
    {
      XmlElement returnsElement = owner.CreateElement("returns");
      returnsElement.InnerText = p;
      return returnsElement;
    }
  }
```

## <a name="see-also"></a>请参阅

- [元数据](../feature-details/metadata.md)
