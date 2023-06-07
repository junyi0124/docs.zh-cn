---
title: 使用自定义活动
ms.date: 03/30/2017
ms.assetid: 8f356419-681a-4175-ae93-878eee970249
ms.openlocfilehash: 43addd4e69b7f7ac3ac5cd8e5bcd41397d8f0a67
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96293320"
---
# <a name="using-a-custom-activity"></a>使用自定义活动

派生自 <xref:System.Activities.Activity> 或其子类的活动可以组合到更大的工作流中，或以代码直接创建。 此主题说明如何使用工作流中以代码或通过设计器创建的自定义活动。  
  
> [!NOTE]
> 自定义活动可以在定义它们的同一个项目中使用。只要自定义活动和使用它的活动都进行了编译 (例如，通过生成过程生成的实例化类型进行加载) 如果引用活动是动态加载的 (例如，使用 ActivityXAMLServices) ，则所引用的程序集应放置在不同的项目中，否则需要手动编辑设计器生成的 XAML 来启用此操作。  
  
#### <a name="using-a-custom-activity-to-a-workflow-project"></a>将自定义活动用于工作流项目  
  
1. 添加从主机项目指向包含自定义活动的活动库项目的引用。  
  
2. 生成解决方案。  
  
3. 如要在设计器中使用自定义活动，须先在工具箱中找到自定义活动，并将该活动拖放到设计器图面上。  
  
4. 如要在代码中使用自定义活动，须添加 Using 语句，使该语句引用自定义活动项目，并将该活动的一个新实例传递给 <xref:System.Activities.WorkflowInvoker.Invoke%2A>。
