---
title: My.WebServices 对象
ms.date: 07/20/2015
f1_keywords:
- My.WebServices
- My.MyProject.WebServices
helpviewer_keywords:
- My.WebServices object
ms.assetid: f188dc05-2c75-41b6-bb68-122d1c3110a2
ms.openlocfilehash: 0b63b44c2cd9d55094fb83fed6c04e4de528a25c
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90867209"
---
# <a name="mywebservices-object"></a>My.WebServices 对象

提供用于创建和访问当前项目所引用的每个 XML Web service 的单个实例的属性。  
  
## <a name="remarks"></a>备注  

 `My.WebServices` 对象提供当前项目所引用的每个 Web 服务的实例。 按需实例化每个实例。 可以通过 `My.WebServices` 对象的属性访问这些 Web 服务。 该属性的名称与该属性所访问的 Web 服务的名称相同。 任何自 <xref:System.Web.Services.Protocols.SoapHttpClientProtocol> 继承的类均为 Web 服务。 有关将 Web 服务添加到项目的信息，请参阅 [访问应用程序 Web 服务](../../developing-apps/programming/accessing-application-web-services.md)。  
  
 `My.WebServices`对象只公开与当前项目相关联的 Web 服务。 它不提供对引用 Dll 中声明的 Web 服务的访问权限。 若要访问 DLL 提供的 Web 服务，必须使用 Web 服务的限定名称，格式为 *DllName*。*WebServiceName*。 有关详细信息，请参阅 [访问应用程序 Web 服务](../../developing-apps/programming/accessing-application-web-services.md)。  
  
 对象及其属性不适用于 Web 应用程序。  
  
## <a name="properties"></a>属性  

 对象的每个属性都 `My.WebServices` 提供对当前项目所引用的 Web 服务的实例的访问。 属性的名称与属性所访问的 Web 服务的名称相同，属性类型与 Web 服务的类型相同。  
  
> [!NOTE]
> 如果存在名称冲突，则用于访问 Web 服务的属性名称为*RootNamespace*_*Namespace* \_ *ServiceName*。 例如，请考虑两个名为 `Service1` 的 Web 服务。 如果这些服务之一位于根命名空间 `WindowsApplication1` 和命名空间中 `Namespace1` ，则可以使用来访问该服务 `My.WebServices.WindowsApplication1_Namespace1_Service1` 。  
  
 首次访问某个 `My.WebServices` 对象的属性时，它将创建 Web 服务的新实例并将其存储。 此属性的后续访问将返回该 Web 服务的实例。  
  
 可以通过 `Nothing` 将分配给 web 服务的属性来释放 web 服务。 属性 setter 分配 `Nothing` 给存储的值。 如果为属性分配除以外的任何值 `Nothing` ，则 setter 会引发 <xref:System.ArgumentException> 异常。  
  
 您可以 `My.WebServices` 使用 or 运算符测试对象的属性是否存储了 Web 服务的实例 `Is` `IsNot` 。 您可以使用这些运算符来检查属性的值是否为 `Nothing` 。  
  
> [!NOTE]
> 通常， `Is` 或 `IsNot` 运算符必须读取属性的值才能执行比较。 但是，如果属性当前存储 `Nothing` ，则属性创建 Web 服务的新实例，然后返回该实例。 不过，Visual Basic 编译器会专门处理对象的属性 `My.WebServices` ，并允许 `Is` 或 `IsNot` 运算符检查属性的状态，而无需更改其值。  
  
## <a name="example"></a>示例  

 此示例调用 `FahrenheitToCelsius` XML Web service 的方法 `TemperatureConverter` ，并返回结果。  
  
 [!code-vb[VbVbalrMyWebService#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyWebService/VB/Form1.vb#1)]  
  
 要使此示例正常运行，你的项目必须引用名为的 Web 服务 `Converter` ，并且该 web 服务必须公开 `ConvertTemperature` 方法。 有关详细信息，请参阅 [访问应用程序 Web 服务](../../developing-apps/programming/accessing-application-web-services.md)。  
  
 此代码在 Web 应用程序项目中不起作用。  
  
## <a name="requirements"></a>要求  
  
### <a name="availability-by-project-type"></a>按项目类型的可用性  
  
|项目类型|可用|  
|---|---|  
|Windows 应用程序|**是**|  
|类库|**是**|  
|控制台应用程序|**是**|  
|Windows 控件库|**是**|  
|Web 控件库|**是**|  
|Windows 服务|**是**|  
|网站|否|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Web.Services.Protocols.SoapHttpClientProtocol>
- <xref:System.ArgumentException>
- [访问应用程序 Web 服务](../../developing-apps/programming/accessing-application-web-services.md)
