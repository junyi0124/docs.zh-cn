---
title: 数据协定代理项
ms.date: 03/30/2017
helpviewer_keywords:
- data contracts [WCF], surrogates
ms.assetid: 8c31134c-46c5-4ed7-94af-bab0ac0dfce5
ms.openlocfilehash: e33a487c03bbf87666d517040e00131f5482be6f
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96251511"
---
# <a name="data-contract-surrogates"></a>数据协定代理项

数据协定 *代理项* 是一种基于数据协定模型构建的高级功能。 在设计上，此功能用于在用户希望更改对类型进行序列化、反序列化或将其设计成元数据的方式时，进行类型自定义和替换。 在以下情况下可以使用代理项：还没有为类型指定数据协定；字段和属性 (Property) 没有用 <xref:System.Runtime.Serialization.DataMemberAttribute> 属性 (Attribute) 进行标记；用户希望动态创建架构变体。  
  
 序列化和反序列化是在使用 <xref:System.Runtime.Serialization.DataContractSerializer> 将 .NET Framework 转换为合适的格式（如 XML）时，借助于数据协定代理项来完成的。 在生成元数据表示形式（如 XML 架构文档 (XSD)）时，也可以使用数据协定代理项来修改为类型导出的元数据。 在导入时将从元数据创建代码，在这种情况下也可以使用代理项来自定义所生成的代码。  
  
## <a name="how-the-surrogate-works"></a>代理项的工作方式  

 代理项通过将一个类型（“原始”类型）映射到另一个类型（“代理项”类型）来工作。 下面的示例演示了原始类型 `Inventory` 和一个新代理项类型 `InventorySurrogated`。 `Inventory` 类型是不可序列化的，但是 `InventorySurrogated` 类型是可序列化的：  
  
 [!code-csharp[C_IDataContractSurrogate#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_idatacontractsurrogate/cs/source.cs#1)]  
  
 由于尚未针对该类定义数据协定，因此请使用数据协定将该类转换为代理项类。 下面的示例演示了这个代理项类：  
  
 [!code-csharp[C_IDataContractSurrogate#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_idatacontractsurrogate/cs/source.cs#2)]  
  
## <a name="implementing-the-idatacontractsurrogate"></a>实现 IDataContractSurrogate  

 若要使用数据协定代理项，请实现 <xref:System.Runtime.Serialization.IDataContractSurrogate> 接口。  
  
 下面概述了 <xref:System.Runtime.Serialization.IDataContractSurrogate> 的每种方法及其可能的实现。  
  
### <a name="getdatacontracttype"></a>GetDataContractType  

 <xref:System.Runtime.Serialization.IDataContractSurrogate.GetDataContractType%2A> 方法将一种类型映射到另一种类型。 此方法是序列化、反序列化、导入和导出所必需的。  
  
 第一项任务是定义要将哪些类型映射到其他类型。 例如：  
  
 [!code-csharp[C_IDataContractSurrogate#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_idatacontractsurrogate/cs/source.cs#3)]  
  
- 在序列化时，随后将使用 GetDataContractType 方法所返回的映射，通过调用 <xref:System.Runtime.Serialization.IDataContractSurrogate.GetObjectToSerialize%2A> 方法来将原始实例转换为代理项实例。  
  
- 在反序列化时，序列化程序将使用 GetDataContractType 方法所返回的映射来反序列化为代理项类型的实例。 序列化程序随后将通过调用 <xref:System.Runtime.Serialization.IDataContractSurrogate.GetDeserializedObject%2A> 来将代理项实例转换为原始类型的实例。  
  
- 在导出时，将反射 GetDataContractType 方法所返回的代理项类型以获得要用于生成元数据的数据协定。  
  
- 在导入时，原始类型会更改为代理项类型，代理项类型将进行反射以获得要用于各种目的（如引用支持）的数据协定。  
  
 <xref:System.Type> 参数是正在序列化、反序列化、导入或导出的对象的类型。 如果该类型没有由代理项处理，则 <xref:System.Runtime.Serialization.IDataContractSurrogate.GetDataContractType%2A> 方法必须返回输入类型； 否则将返回相应的代理项类型。 如果存在多个代理项类型，则可以在该方法中定义许多映射。  
  
 <xref:System.Runtime.Serialization.IDataContractSurrogate.GetDataContractType%2A> 方法不是针对内置的数据协定基元（如 <xref:System.Int32> 或 <xref:System.String>）调用的。 对于其他类型（如数组、用户定义的类型）和其他数据结构，将针对每种类型调用此方法。  
  
 在上面的示例中，此方法检查 `type` 参数与 `Inventory` 是否具有可比性。 如果具有可比性，此方法会将该参数映射到 `InventorySurrogated`。 每当调用序列化、反序列化、导入架构或导出架构时，都会首先调用此函数来确定类型之间的映射。  
  
### <a name="getobjecttoserialize-method"></a>GetObjectToSerialize 方法  

 <xref:System.Runtime.Serialization.IDataContractSurrogate.GetObjectToSerialize%2A> 方法用来将原始类型的实例转换为代理项类型的实例。 此方法是序列化所必需的。  
  
 下一步是定义如何通过实现 <xref:System.Runtime.Serialization.IDataContractSurrogate.GetObjectToSerialize%2A> 方法来将物理数据从原始实例映射到代理项。 例如：  
  
 [!code-csharp[C_IDataContractSurrogate#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_idatacontractsurrogate/cs/source.cs#4)]  
  
 当对对象进行序列化时，会调用 <xref:System.Runtime.Serialization.IDataContractSurrogate.GetObjectToSerialize%2A> 方法。 此方法会将数据从原始类型传输到代理项类型的字段。 可以直接将字段映射到代理项字段，也可以将对原始数据的操作存储在代理项中。 下面是一些可能的用法：直接映射字段；针对存储在代理项字段中的数据执行操作；将原始类型的 XML 存储在代理项字段中。  
  
 `targetType` 参数指的是成员的声明类型。 此参数是由 <xref:System.Runtime.Serialization.IDataContractSurrogate.GetDataContractType%2A> 方法返回的代理项类型。 序列化程序不强求使所返回的对象可对该类型赋值。 `obj`参数是要序列化的对象，并将在必要时转换为其代理项。 如果该对象没有由代理项处理，则此方法必须返回输入对象； 否则，将返回新的代理项对象。 如果此对象为 Null，则将不调用该代理项。 可以在该方法中为不同的实例定义许多代理项映射。  
  
 在创建 <xref:System.Runtime.Serialization.DataContractSerializer> 时，可以命令它保留对象引用。  (有关详细信息，请参阅 [序列化和反序列化](../feature-details/serialization-and-deserialization.md)。 ) 这是通过将 `preserveObjectReferences` 其构造函数中的参数设置为来完成 `true` 的。 在这种情况下，只对每个对象调用一次该代理项，因为以后所有的序列化都只是将引用写入相应的流中。 如果 `preserveObjectReferences` 设置为 `false`，那么，每遇到一个实例，都会调用该代理项。  
  
 如果已序列化实例的类型不同于所声明的类型，则类型信息（例如，`xsi:type`）将写入相应的流中，以便允许在另一端对该实例进行反序列化。 无论该对象是否为代理项对象，都会发生此过程。  
  
 上面的示例将 `Inventory` 实例的数据转换为 `InventorySurrogated` 的数据。 它检查对象的类型并对其执行必要的操作，以便将它转换为代理项类型。 在本例中是用 `Inventory` 类的字段直接覆盖 `InventorySurrogated` 类的字段。  
  
### <a name="getdeserializedobject-method"></a>GetDeserializedObject 方法  

 <xref:System.Runtime.Serialization.IDataContractSurrogate.GetDeserializedObject%2A> 方法用来将代理项类型的实例转换为原始类型的实例。 它是反序列化所必需的。  
  
 下一项任务是定义如何将物理数据从代理项实例映射到原始实例。 例如：  
  
 [!code-csharp[C_IDataContractSurrogate#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_idatacontractsurrogate/cs/source.cs#5)]  
  
 只有在对对象进行反序列化时才调用此方法。 此方法提供了用来从代理项类型反序列化到其原始类型的反向数据映射。 此方法与 `GetObjectToSerialize` 方法的可能用法相似：直接交换字段数据、针对数据执行操作以及存储 XML 数据。 在反序列化时，由于数据转换中的一些操作，您可能无法始终从原始实例获取确切的数据值。  
  
 `targetType` 参数指的是成员的声明类型。 此参数是由 `GetDataContractType` 方法返回的代理项类型。 `obj`参数引用已反序列化的对象。 如果这是代理项对象，则可以将其重新转换为其原始类型。 如果该对象没有由代理项处理，则此方法将返回输入对象； 否则，将在转换完成之后立即返回反序列化的对象。 如果存在多个代理项类型，则可以通过指示每个类型及其转换来提供从代理项到主类型的数据转换。  
  
 在返回对象时，内部对象表会用该代理项所返回的对象进行更新。 以后无论何时引用实例，都将从内部对象表中获取其代理项实例。  
  
 上面的示例将 `InventorySurrogated` 类型的对象重新转换为其初始类型 `Inventory`。 在该例中，数据直接从 `InventorySurrogated` 传输到其在 `Inventory` 中的相应字段。 由于没有对数据执行任何操作，因此每个成员字段所包含的值与序列化之前相同。  
  
### <a name="getcustomdatatoexport-method"></a>GetCustomDataToExport 方法  

 在导出架构时，<xref:System.Runtime.Serialization.IDataContractSurrogate.GetCustomDataToExport%2A> 方法是可选的。 此方法用来在所导出的架构中插入其他数据或提示。 可以在成员级别或类型级别插入其他数据。 例如：  
  
 [!code-csharp[C_IDataContractSurrogate#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_idatacontractsurrogate/cs/source.cs#6)]  
  
 此方法（连同两个重载）允许在成员级别或类型级别将额外的信息包括到元数据中。 可以包括有关某个成员是公共成员还是私有成员的提示，以及将在导出和导入架构的整个过程中保留的注释。 如果没有此方法，类似的信息将会丢失。 此方法不会导致插入或删除成员或类型，而是会在成员级别或类型级别向架构中添加额外的数据。  
  
 此方法是重载方法，可以采用 `Type`（`clrtype` 参数）或 <xref:System.Reflection.MemberInfo>（`memberInfo` 参数）。 第二个参数始终为 `Type`（`dataContractType` 参数）。 将针对代理类型 `dataContractType` 的每个成员和类型调用此方法。  
  
 其中的任一重载都必须返回 `null` 或一个可序列化的对象。 非 Null 对象将作为批注序列化到导出的架构中。 对于 `Type` 重载，导出到架构的每个类型都会通过第一个参数，连同作为 `dataContractType` 参数的代理项类型一起发送到此方法。 对于 `MemberInfo` 重载，导出到架构的每个成员的成员信息都会连同第二个参数中的代理项类型，作为 `memberInfo` 参数来发送。  
  
#### <a name="getcustomdatatoexport-method-type-type"></a>GetCustomDataToExport 方法 (Type, Type)  

 在针对每个类型定义导出架构的过程中，会调用 <xref:System.Runtime.Serialization.IDataContractSurrogate.GetCustomDataToExport%28System.Type%2CSystem.Type%29?displayProperty=nameWithType> 方法。 在导出时，此方法会向架构中的类型添加信息。 所定义的每个类型都发送到此方法，以便确定是否需要在架构中包括任何额外的数据。  
  
#### <a name="getcustomdatatoexport-method-memberinfo-type"></a>GetCustomDataToExport 方法 (MemberInfo, Type)  

 在针对已导出类型中的每个成员导出架构的过程中，会调用 <xref:System.Runtime.Serialization.IDataContractSurrogate.GetCustomDataToExport%28System.Reflection.MemberInfo%2CSystem.Type%29?displayProperty=nameWithType>。 使用此函数，可以针对在导出时将要包括在架构中的成员自定义任何注释。 该类中每个成员的信息都发送到此方法，以便检查是否需要在架构中添加任何额外的数据。  
  
 上面的示例通过每个代理项成员的 `dataContractType` 进行搜索。 它返回与每个字段相对应的访问修饰符。 如果没有该自定义项，则访问修饰符的默认值为 public（公共）。 因此，在借助于所导出的架构生成的代码中，所有的成员都将定义为 public（公共），而与它们的实际访问限制无关。 在未使用该实现时，成员 `numpens` 将在导出的架构中为 public（公共），即使它在代理项中定义为 private（私有）也是如此。 通过使用此方法，可以将所导出架构中的访问修饰符生成为 private（私有）。  
  
### <a name="getreferencedtypeonimport-method"></a>GetReferencedTypeOnImport 方法  

 此方法将代理项的 <xref:System.Type> 映射到原始类型。 此方法对于架构导入是可选的。  
  
 在创建用来导入架构并为其生成代码的代理项时，下一项任务就是将代理项实例的类型定义为其原始类型。  
  
 如果所生成的代码需要引用现有的用户类型，则这可以通过实现 <xref:System.Runtime.Serialization.IDataContractSurrogate.GetReferencedTypeOnImport%2A> 方法来完成。  
  
 在导入架构时，会针对每个类型声明都调用此方法，以便将代理项数据协定映射到某个类型。 字符串参数 `typeName` 和 `typeNamespace` 用来定义代理项类型的名称和命名空间。 <xref:System.Runtime.Serialization.IDataContractSurrogate.GetReferencedTypeOnImport%2A> 的返回值用来确定是否需要生成新类型。 此方法必须返回一个有效类型或 Null。 如果返回的是有效类型，所返回的类型将用作在生成的代码中引用的类型。 如果返回的是 Null，则将不引用任何类型，而且必须创建一个新类型。 如果存在多个代理项，则可以执行映射，使每个代理项类型重新转换为其初始类型。  
  
 `customData` 参数是最初从 <xref:System.Runtime.Serialization.IDataContractSurrogate.GetCustomDataToExport%2A> 返回的对象。 当代理项作者希望向导入过程中用来生成代码的元数据中插入额外的数据/提示时，可以使用这个 `customData` 参数。  
  
### <a name="processimportedtype-method"></a>ProcessImportedType 方法  

 <xref:System.Runtime.Serialization.IDataContractSurrogate.ProcessImportedType%2A> 方法对架构导入过程中创建的任何类型进行自定义。 此方法是可选的。  
  
 在导入架构时，此方法允许对所导入的任何类型和编译信息进行自定义。 例如：  
  
 [!code-csharp[C_IDataContractSurrogate#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_idatacontractsurrogate/cs/source.cs#7)]  
  
 在导入过程中，会针对所生成的每个类型调用此方法。 更改指定的 <xref:System.CodeDom.CodeTypeDeclaration> 或者修改 <xref:System.CodeDom.CodeCompileUnit>。 这包括更改 `CodeTypeDeclaration` 的名称、成员、属性 (Attribute) 以及许多其他属性 (Property)。 通过处理 `CodeCompileUnit`，可以修改指令、命名空间、所引用的程序集以及几个其他方面的内容。  
  
 `CodeTypeDeclaration` 参数包含对代码 DOM 类型的声明。 `CodeCompileUnit` 参数允许为了处理代码而进行修改。  如果返回的是 `null`，则将导致丢弃类型声明。 相反，如果返回的是 `CodeTypeDeclaration`，则将保留所做的修改。  
  
 如果在导出元数据的过程中插入了自定义数据，则需要在导入过程中将自定义数据提供给用户，使其可供用户使用。 使用这个自定义数据可以对模型提示或其他注释进行编程。 每个 `CodeTypeDeclaration` 和 <xref:System.CodeDom.CodeTypeMember> 实例都包括 <xref:System.CodeDom.CodeObject.UserData%2A> 属性形式的自定义数据，以及到 `IDataContractSurrogate` 类型的强制转换。  
  
 上面的示例针对导入的架构执行了一些更改。 该代码通过使用一个代理项保留了原始类型的私有成员。 在导入架构时，默认的访问修饰符为 `public`。 因此，除非进行过修改，否则该代理项架构的所有成员都将是公共成员，如上面的示例中所示。 在导出过程中，会在元数据中插入有关哪些成员是私有成员的自定义数据。 上面的示例查找自定义数据，检查访问修饰符是否为 private（私有），然后通过设置属性将相应的成员修改为 private（私有）。 如果没有这个自定义项，`numpens` 成员将定义为 public（公共），而不是 private（私有）。  
  
### <a name="getknowncustomdatatypes-method"></a>GetKnownCustomDataTypes 方法  

 此方法从架构中获取定义的自定义数据类型。 此方法对于架构导入是可选的。  
  
 此方法是在开始导出和导入架构时调用的。 此方法返回用在导出/导入架构中的自定义数据类型。 系统会向此方法传递一个 <xref:System.Collections.ObjectModel.Collection%601>（`customDataTypes` 参数），该参数是一个类型集合。 此方法应当向该集合中添加其他已知类型。 已知的自定义数据类型是借助于 <xref:System.Runtime.Serialization.DataContractSerializer> 来启用对自定义数据的序列化和反序列化所必需的。 有关详细信息，请参阅 [数据协定已知类型](../feature-details/data-contract-known-types.md)。  
  
## <a name="implementing-a-surrogate"></a>实现代理项  

 若要在 WCF 中使用数据协定代理项，必须执行一些特殊的过程。  
  
### <a name="to-use-a-surrogate-for-serialization-and-deserialization"></a>使用代理项进行序列化和反序列化  

 使用 <xref:System.Runtime.Serialization.DataContractSerializer> 可以通过代理项对数据执行序列化和反序列化。 <xref:System.Runtime.Serialization.DataContractSerializer> 是由 <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior> 创建的。 还必须指定代理项。  
  
##### <a name="to-implement-serialization-and-deserialization"></a>实现序列化和反序列化  
  
1. 为您的服务创建一个 <xref:System.ServiceModel.ServiceHost> 实例。 有关完整说明，请参阅 [基本 WCF 编程](../basic-wcf-programming.md)。  
  
2. 对于指定服务主机的每个 <xref:System.ServiceModel.Description.ServiceEndpoint>，查找它的 <xref:System.ServiceModel.Description.OperationDescription>。  
  
3. 通过操作行为进行搜索，确定是否找到了 <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior> 的实例。  
  
4. 如果找到了一个 <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior>，请将它的 <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior.DataContractSurrogate%2A> 属性设置为该代理项的新实例。 如果没有找到 <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior>，请创建一个新实例，并将新行为的 <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior.DataContractSurrogate%2A> 成员设置为该代理项的新实例。  
  
5. 最后，将这个新行为添加到当前的操作行为中，如下面的示例中所示：  
  
     [!code-csharp[C_IDataContractSurrogate#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_idatacontractsurrogate/cs/source.cs#8)]  
  
### <a name="to-use-a-surrogate-for-metadata-import"></a>使用代理项来导入元数据  

 在导入元数据（如 WSDL 和 XSD）以生成客户端代码时，需要将该代理项添加到负责从 XSD 架构生成代码的组件 <xref:System.Runtime.Serialization.XsdDataContractImporter> 中。 为此，请直接修改用来导入元数据的 <xref:System.ServiceModel.Description.WsdlImporter>。  
  
##### <a name="to-implement-a-surrogate-for-metadata-importation"></a>实现用来导入元数据的代理项  
  
1. 使用 <xref:System.ServiceModel.Description.WsdlImporter> 类导入元数据。  
  
2. 使用 <xref:System.Collections.Generic.Dictionary%602.TryGetValue%2A> 方法检查是否已经定义了 <xref:System.Runtime.Serialization.XsdDataContractImporter>。  
  
3. 如果 <xref:System.Collections.Generic.Dictionary%602.TryGetValue%2A> 方法返回 `false`，请创建一个新的 <xref:System.Runtime.Serialization.XsdDataContractImporter> 并将它的 <xref:System.Runtime.Serialization.XsdDataContractImporter.Options%2A> 属性设置为 <xref:System.Runtime.Serialization.ImportOptions> 类的新实例。 否则，请使用由 `out` 方法的 <xref:System.Collections.Generic.Dictionary%602.TryGetValue%2A> 参数所返回的导入程序。  
  
4. 如果没有为 <xref:System.Runtime.Serialization.XsdDataContractImporter> 定义 <xref:System.Runtime.Serialization.ImportOptions>，请将该属性设置为 <xref:System.Runtime.Serialization.ImportOptions> 类的新实例。  
  
5. 对于 <xref:System.Runtime.Serialization.ImportOptions.DataContractSurrogate%2A> 的 <xref:System.Runtime.Serialization.ImportOptions>，请将 <xref:System.Runtime.Serialization.XsdDataContractImporter> 属性设置为该代理项的新实例。  
  
6. 将 <xref:System.Runtime.Serialization.XsdDataContractImporter> 添加到由 <xref:System.ServiceModel.Description.MetadataExporter.State%2A>（继承自 <xref:System.ServiceModel.Description.WsdlImporter> 类）的 <xref:System.ServiceModel.Description.MetadataExporter> 属性返回的集合中。  
  
7. 使用 <xref:System.ServiceModel.Description.WsdlImporter.ImportAllContracts%2A> 的 <xref:System.ServiceModel.Description.WsdlImporter> 方法导入架构中所有的数据协定。 在最后一步中，将从通过调用该代理项而加载的架构中生成代码。  
  
     [!code-csharp[C_IDataContractSurrogate#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_idatacontractsurrogate/cs/source.cs#9)]  
  
### <a name="to-use-a-surrogate-for-metadata-export"></a>使用代理项来导出元数据  

 默认情况下，当从服务的 WCF 中导出元数据时，需要生成 WSDL 和 XSD 架构。 该代理项需要添加到负责为数据协定类型生成 XSD 架构的组件 <xref:System.Runtime.Serialization.XsdDataContractExporter> 中。 为此，要么使用可实现 <xref:System.ServiceModel.Description.IWsdlExportExtension> 的行为来修改 <xref:System.ServiceModel.Description.WsdlExporter>，要么直接修改用来导出元数据的 <xref:System.ServiceModel.Description.WsdlExporter>。  
  
##### <a name="to-use-a-surrogate-for-metadata-export"></a>使用代理项来导出元数据  
  
1. 创建一个新的 <xref:System.ServiceModel.Description.WsdlExporter>，或者使用传递到 `wsdlExporter` 方法的 <xref:System.ServiceModel.Description.IWsdlExportExtension.ExportContract%2A> 参数。  
  
2. 使用 <xref:System.Collections.Generic.Dictionary%602.TryGetValue%2A> 函数来检查是否已经定义了 <xref:System.Runtime.Serialization.XsdDataContractExporter>。  
  
3. 如果 <xref:System.Collections.Generic.Dictionary%602.TryGetValue%2A> 返回 `false`，请使用从 <xref:System.Runtime.Serialization.XsdDataContractExporter> 生成的 XML 架构创建一个新的 <xref:System.ServiceModel.Description.WsdlExporter>，并将它添加到由 <xref:System.ServiceModel.Description.MetadataExporter.State%2A> 的 <xref:System.ServiceModel.Description.WsdlExporter> 属性返回的集合中。 否则，请使用由 `out` 方法的 <xref:System.Collections.Generic.Dictionary%602.TryGetValue%2A> 参数返回的导出程序。  
  
4. 如果没有为 <xref:System.Runtime.Serialization.XsdDataContractExporter> 定义 <xref:System.Runtime.Serialization.ExportOptions>，请将 <xref:System.Runtime.Serialization.XsdDataContractExporter.Options%2A> 属性设置为 <xref:System.Runtime.Serialization.ExportOptions> 类的新实例。  
  
5. 对于 <xref:System.Runtime.Serialization.ExportOptions.DataContractSurrogate%2A> 的 <xref:System.Runtime.Serialization.ExportOptions>，请将 <xref:System.Runtime.Serialization.XsdDataContractExporter> 属性设置为该代理项的新实例。 在以后的元数据导出步骤中，将不需要进行任何更改。  
  
     [!code-csharp[C_IDataContractSurrogate#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_idatacontractsurrogate/cs/source.cs#10)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.Runtime.Serialization.DataContractSerializer>
- <xref:System.Runtime.Serialization.IDataContractSurrogate>
- <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior>
- <xref:System.Runtime.Serialization.ImportOptions>
- <xref:System.Runtime.Serialization.ExportOptions>
- [使用数据协定](../feature-details/using-data-contracts.md)
