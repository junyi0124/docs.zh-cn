---
title: AspNetCompatibilityRequirementsAttribute
ms.date: 03/30/2017
ms.assetid: 00908a39-a21b-4029-bbb9-33e5a6ed25a7
ms.openlocfilehash: 9cf7031b58fcf9a5d29761d9ffbdaee45d9ff3ff
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96251992"
---
# <a name="aspnetcompatibilityrequirementsattribute"></a>AspNetCompatibilityRequirementsAttribute

AspNetCompatibilityRequirementsAttribute  
  
## <a name="syntax"></a>语法  
  
```csharp
class AspNetCompatibilityRequirementsAttribute : Behavior  
{  
  string RequirementsMode;  
};  
```  
  
## <a name="methods"></a>方法  

 AspNetCompatibilityRequirementsAttribute 类不定义任何方法。  
  
## <a name="properties"></a>属性  

 AspNetCompatibilityRequirementsAttribute 类具有以下属性。  
  
### <a name="requirementsmode"></a>RequirementsMode  

 数据类型：字符串  
  
 访问类型：只读  
  
 指示 ASP.NET 兼容模式是否处于活动状态。  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.ServiceHostingEnvironment.AspNetCompatibilityEnabled%2A>
