---
title: 在 Visual Studio 中创建启用 AJAX 的 WCF 服务和 ASP.NET 客户端
ms.date: 08/17/2018
ms.assetid: 95012df8-2a66-420d-944a-8afab261013e
ms.openlocfilehash: 0bfe55c68f68bfef7b7ec2034413b53d41b0c785
ms.sourcegitcommit: 97405ed212f69b0a32faa66a5d5fae7e76628b68
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/01/2020
ms.locfileid: "91609351"
---
# <a name="how-to-create-an-ajax-enabled-wcf-service-and-an-aspnet-client-that-accesses-the-service"></a>如何：创建支持 AJAX 的 WCF 服务和访问该服务的 ASP.NET 客户端

本主题演示如何使用 Visual Studio 创建启用 AJAX 的 Windows Communication Foundation (WCF) 服务和访问服务的 ASP.NET 客户端。

## <a name="create-an-aspnet-web-app"></a>创建 ASP.NET Web 应用

1. 打开 Visual Studio。

1. 从 "**文件**" 菜单中选择 "**新建**  >  **项目**"

1. 在 "**新建项目**" 对话框中，展开 "**已安装**的  >  **Visual c #**  >  **Web** " 类别，然后选择 " **ASP.NET Web 应用程序 ( .NET Framework) **"。

1. 将项目命名为 **SandwichServices** ，然后单击 **"确定"**。

1. 在 " **新建 ASP.NET Web 应用程序** " 对话框中，选择 " **空** "，然后选择 **"确定"**。

   ![Visual Studio 中的 ASP.NET web 应用类型对话框](./media/create-an-ajax-wcf-asp-net-client/new-asp-net-web-app-type.png)

## <a name="add-a-web-form"></a>添加 web 窗体

1. 在**解决方案资源管理器**中右键单击 "SandwichServices" 项目，然后选择 "**添加**  >  **新项**"。

1. 在 "**添加新项**" 对话框中，展开 "**已安装**的  >  **Visual c #**  >  **Web** " 类别，然后选择 " **Web 窗体**" 模板。

1. 接受默认名称 " (**webform1.aspx** ") ，然后选择 " **添加**"。

   *Webform1.aspx* 在 **源** 视图中打开。

1. 在标记中添加以下标记 **\<body>** ：

   ```html
   <input type="button" value="Price of 3 sandwiches" onclick="Calculate()"/>
   <br />
   <span id="additionResult"></span>
   ```

## <a name="create-an-ajax-enabled-wcf-service"></a>创建启用 AJAX 的 WCF 服务

1. 在**解决方案资源管理器**中右键单击 "SandwichServices" 项目，然后选择 "**添加**  >  **新项**"。

1. 在 "**添加新项**" 对话框中，展开 "**已安装**的  >  **Visual c #**  >  **Web** " 类别，然后选择**WCF 服务 (启用 AJAX 的) **模板。

   ![WCF 服务 (Visual Studio 中启用 AJAX 的) 项模板](./media/create-an-ajax-wcf-asp-net-client/add-wcf-service.png)

1. 将服务命名为 " **CostService** "，然后选择 " **添加**"。

   *CostService.svc.cs* 在编辑器中打开。

1. 在服务中实现该操作。 将以下方法添加到 CostService 类，以计算数量三明治的成本：

    ```csharp
    [OperationContract]
    public double CostOfSandwiches(int quantity)
    {
        return 1.25 * quantity;
    }
    ```

## <a name="configure-the-client-to-access-the-service"></a>配置客户端以访问服务

1. 打开 *webform1.aspx* 文件，然后选择 " **设计** " 视图。

2. 从 " **视图** " 菜单中选择 **"工具箱**"。

3. 展开 " **AJAX 扩展** " 节点，将 **ScriptManager** 拖放到窗体上。

4. 返回到 **源** 视图，在标记之间添加以下代码， **\<ScriptManager>** 以指定 WCF 服务的路径：

    ```xml
    <Services>
       <asp:ServiceReference Path="~/CostService.svc" />
    </Services>
    ```

5. 添加 JavaScript 函数的代码 `Calculate()` 。 将以下代码放置在 web 窗体的 **head** 部分：

    ```html
    <script type="text/javascript">

        function Calculate() {
            CostService.CostOfSandwiches(3, onSuccess);
        }

        function onSuccess(result) {
            var myres = $get("additionResult");
            myres.innerHTML = result;
        }

    </script>
    ```

   此代码将调用 CostService 的方法来计算三个三明治的价格，然后将结果显示在名为 **additionResult**的范围内。

## <a name="run-the-program"></a>运行程序

请确保 *webform1.aspx* 具有焦点，然后按 " **启动** " 按钮以启动 web 客户端。 该按钮具有一个绿色三角形，并显示类似于 **IIS Express (Microsoft Edge) **的内容。 也可以按 <kbd>F5</kbd>。 单击 " **3 三明治** " 按钮的 "价格" 以生成 "3.75" 的预期输出。

## <a name="example"></a>示例

下面是 *CostService.svc.cs* 文件中的完整代码：

```csharp
using System.ServiceModel;
using System.ServiceModel.Activation;

namespace SandwichServices
{
    [ServiceContract(Namespace = "")]
    [AspNetCompatibilityRequirements(RequirementsMode = AspNetCompatibilityRequirementsMode.Allowed)]
    public class CostService
    {
        [OperationContract]
        public double CostOfSandwiches(int quantity)
        {
            return 1.25 * quantity;
        }
    }
}
```

下面是 *webform1.aspx* 页的完整内容：

```aspx-csharp
<%@ Page Language="C#" AutoEventWireup="true" CodeBehind="WebForm1.aspx.cs" Inherits="SandwichServices.WebForm1" %>

<!DOCTYPE html>

<html xmlns="http://www.w3.org/1999/xhtml">
<head runat="server">
    <title></title>
    <script type="text/javascript">

        function Calculate() {
            CostService.CostOfSandwiches(3, onSuccess);
        }

        function onSuccess(result) {
            var myres = $get("additionResult");
            myres.innerHTML = result;
        }

    </script>
</head>
<body>
    <form id="form1" runat="server">
        <div>
        </div>
        <asp:ScriptManager ID="ScriptManager1" runat="server">
            <Services>
                <asp:ServiceReference Path="~/CostService.svc" />
            </Services>
        </asp:ScriptManager>

        <input type="button" value="Price of 3 sandwiches" onclick="Calculate()" />
        <br />
        <span id="additionResult"></span>
    </form>
</body>
</html>
```
