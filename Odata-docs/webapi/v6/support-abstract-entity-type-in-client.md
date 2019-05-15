---
title: "Abstract entity type support in .NET client"
description: ""

author: Khairunj
ms.author: Khairunj
ms.date: 02/19/2019
ms.topic: article
ms.service: multiple
---

OData Client for .NET supports abstract entity type without key from ODataLib 6.11.0.

## Create model with abstract entity type

``` csharp
var abstractType = new EdmEntityType("DefaultNS", "AbstractEntity", null, true, false);
model.AddElement(abstractType);

var orderType = new EdmEntityType("DefaultNS", "Order", abstractType);
var orderIdProperty = new EdmStructuralProperty(orderType, "OrderID", EdmCoreModel.Instance.GetInt32(false));
orderType.AddProperty(orderIdProperty);
orderType.AddKeys(orderIdProperty);
model.AddElement(orderType);
```

## Output model

    <EntityType Name="AbstractEntity" Abstract="true" />
    <EntityType Name="Order" BaseType="DefaultNS.AbstractEntity">
      <Key>
        <PropertyRef Name="OrderID" />
      </Key>
    </EntityType>
      
## Client generated proxy file
T4 would auto-generate code for abstract entity type like:

``` csharp
[global::Microsoft.OData.Client.EntityType()]
public abstract partial class AbstractEntity : global::Microsoft.OData.Client.BaseEntityType, global::System.ComponentModel.INotifyPropertyChanged
{
 ...
}

[global::Microsoft.OData.Client.Key("OrderID")]
[global::Microsoft.OData.Client.EntitySet("Orders")]
public partial class Order : AbstractEntity
{
 ...
}
```