---
title: 利用反射的 API
ms.date: 03/30/2017
ms.assetid: f9532629-6594-4a41-909f-d083f30a42f3
ms.openlocfilehash: 2c361962f4570200d63037a68ef39b0c982bd5f7
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96251134"
---
# <a name="apis-that-rely-on-reflection"></a>利用反射的 API

在某些情况下，在代码中使用反射并不明显，因此 .NET Native 工具链不会保留运行时所需的元数据。 该主题介绍了一些常见的 API 或常见编程模式，它们不被视为是反射 API 的一部分，而依赖反射成功执行。 如果在源代码中使用了它们，可以将有关它们的信息添加到运行时指令 (.rd.xml) 文件，以便对这些 API 的调用不会在运行时内引发 [MissingMetadataException](missingmetadataexception-class-net-native.md) 异常或某种其他异常。  
  
## <a name="typemakegenerictype-method"></a>Type.MakeGenericType 方法  

 你可以通过使用以下所示代码调用 `AppClass<T>` 方法来动态实例化一个泛型类型 <xref:System.Type.MakeGenericType%2A?displayProperty=nameWithType>：  
  
 [!code-csharp[ProjectN#1](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn/cs/type_makegenerictype1.cs#1)]  
  
 为使此代码在运行时间能正确运行，需要元数据的几个项。 首先是未得到实例化的泛型类型 `Browse` 的 `AppClass<T>` 元数据：  
  
```xml  
<Type Name="App1.AppClass`1" Browse="Required PublicAndInternal" />  
```  
  
 这允许 <xref:System.Type.GetType%28System.String%2CSystem.Boolean%29?displayProperty=nameWithType> 方法调用成功并返回一个有效的 <xref:System.Type> 对象。  
  
 但即使当你为未实例化的泛型类型添加元数据时，调用 <xref:System.Type.MakeGenericType%2A?displayProperty=nameWithType> 方法也会引发 [MissingMetadataException](missingmetadataexception-class-net-native.md) 异常：  
  
由于性能原因，已删除以下类型的元数据，因此无法执行此操作：  
  
`App1.AppClass`1<System.object>"。  
  
 你可以将以下运行时指令添加到运行时指令文件，从而为 `Activate` 的位于 `AppClass<T>` 上的特定实例化添加 <xref:System.Int32?displayProperty=nameWithType> 元数据。  
  
```xml  
<TypeInstantiation Name="App1.AppClass" Arguments="System.Int32"
                   Activate="Required Public" />  
```  
  
 `AppClass<T>` 上的每个不同的实例化都要求一个不同的指令（如果该实例化是使用 <xref:System.Type.MakeGenericType%2A?displayProperty=nameWithType> 方法创建的并且不静态使用）。  
  
## <a name="methodinfomakegenericmethod-method"></a>MethodInfo.MakeGenericMethod 方法  

 给定一个含有泛型方法 `Class1` 的类 `GetMethod<T>(T t)`，`GetMethod` 可以使用类似以下所示的代码通过反射得到调用：  
  
 [!code-csharp[ProjectN#2](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn/cs/makegenericmethod1.cs#2)]  
  
 要成功运行，此代码需要几个元数据项：  
  
- 你想要调用的方法的类型的 `Browse` 元数据。  
  
- 你想要调用的方法 `Browse` 元数据。  如果它是一个公共方法，为包含类型添加的公共 `Browse` 元数据也包括方法。  
  
- 要调用的方法的动态元数据，以便 .NET Native 工具链不会删除反射调用委托。 如果该方法的动态元数据丢失，当 <xref:System.Reflection.MethodInfo.MakeGenericMethod%2A?displayProperty=nameWithType> 方法得到调用时会引发以下异常：  
  
    ```output
    MakeGenericMethod() cannot create this generic method instantiation because the instantiation was not metadata-enabled: 'App1.Class1.GenMethod<Int32>(Int32)'.  
    ```  
  
 以下运行时指令能确保各种所需的元数据都可用：  
  
```xml  
<Type Name="App1.Class1" Browse="Required PublicAndInternal">  
   <MethodInstantiation Name="GenMethod" Arguments="System.Int32" Dynamic="Required"/>  
</Type>  
```  
  
 遭到动态调用的方法的每个不同实例化都需要一个 `MethodInstantiation` 指令，并且 `Arguments` 元素会得到更新以反射每个不同的实例化参数。  
  
## <a name="arraycreateinstance-and-typemaketypearray-methods"></a>Array.CreateInstance 和 Type.MakeTypeArray 方法  

 以下实例在 <xref:System.Type.MakeArrayType%2A?displayProperty=nameWithType> 类型上调用了 <xref:System.Array.CreateInstance%2A?displayProperty=nameWithType> 和 `Class1` 方法。  
  
 [!code-csharp[ProjectN#3](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn/cs/array1.cs#3)]  
  
 如果不存在阵列元数据，以下错误会导致：  
  
```output
This operation cannot be carried out as metadata for the following type was removed for performance reasons:  
  
App1.Class1[]  
  
Unfortunately, no further information is available.  
```  
  
 需要阵列类型的 `Browse` 元数据才能将其动态实例化。  以下运行时指令允许对 `Class1[]` 的进行动态实例化。  
  
```xml  
<Type Name="App1.Class1[]" Browse="Required Public" />  
```  
  
## <a name="see-also"></a>另请参阅

- [入门](getting-started-with-net-native.md)
- [运行时指令 (rd.xml) 配置文件引用](runtime-directives-rd-xml-configuration-file-reference.md)
