---
description: Objeto Field
title: Objeto de campo | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field
helpviewer_keywords:
- Field object [ADO]
ms.assetid: b10a72fc-3c4b-4186-a70b-993dc9f7a092
author: rothja
ms.author: jroth
ms.openlocfilehash: 04e7faf5525c676ecc9c20bb312414742e17fc4e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034095"
---
# <a name="field-object"></a>Objeto Field
Representa una columna de datos con un tipo de datos común.  
  
## <a name="remarks"></a>Observaciones  
 Cada objeto de **campo** corresponde a una columna del [conjunto de registros](../../../ado/reference/ado-api/recordset-object-ado.md). La propiedad [valor](../../../ado/reference/ado-api/value-property-ado.md) de los objetos de **campo** se utiliza para establecer o devolver datos del registro actual. Dependiendo de la funcionalidad que expone el proveedor, puede que algunas colecciones, métodos o propiedades de un objeto de **campo** no estén disponibles.  
  
 Con las colecciones, los métodos y las propiedades de un objeto de **campo** , puede hacer lo siguiente:  
  
-   Devuelve el nombre de un campo con la propiedad [nombre](../../../ado/reference/ado-api/name-property-ado.md) .  
  
-   Ver o cambiar los datos del campo con la propiedad **valor** . **Value** es la propiedad predeterminada del objeto **Field** .  
  
-   Devuelve las características básicas de un campo con las propiedades [Type](../../../ado/reference/ado-api/type-property-ado.md), [Precision](../../../ado/reference/ado-api/precision-property-ado.md)y [NumericScale](../../../ado/reference/ado-api/numericscale-property-ado.md) .  
  
-   Devuelve el tamaño declarado de un campo con la propiedad [DefinedSize](../../../ado/reference/ado-api/definedsize-property.md) .  
  
-   Devuelve el tamaño real de los datos de un campo determinado con la propiedad [ActualSize](../../../ado/reference/ado-api/actualsize-property-ado.md) .  
  
-   Determine qué tipos de funcionalidad se admiten para un campo determinado con la propiedad [attributes](../../../ado/reference/ado-api/attributes-property-ado.md) y la colección [Properties](../../../ado/reference/ado-api/properties-collection-ado.md) .  
  
-   Manipular los valores de los campos que contienen datos binarios largos o de caracteres largos con los métodos [AppendChunk](../../../ado/reference/ado-api/appendchunk-method-ado.md) y [GetChunk](../../../ado/reference/ado-api/getchunk-method-ado.md) .  
  
-   Si el proveedor admite las actualizaciones por lotes, resuelva las discrepancias en los valores de campo durante la actualización por lotes con las propiedades [OriginalValue](../../../ado/reference/ado-api/originalvalue-property-ado.md) y [UnderlyingValue](../../../ado/reference/ado-api/underlyingvalue-property.md) .  
  
 Todas las propiedades de metadatos (**Name**, **Type**, **DefinedSize**, **Precision** y **NumericScale**) están disponibles antes de abrir el conjunto de **registros** del objeto de **campo** . Establecerlos en ese momento es útil para la creación dinámica de formularios.  
  
 Esta sección contiene el siguiente tema.  
  
-   [Propiedades, métodos y eventos del objeto Field](../../../ado/reference/ado-api/field-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Fields (colección) (ADO)](../../../ado/reference/ado-api/fields-collection-ado.md)   
 [Colección Properties (ADO)](../../../ado/reference/ado-api/properties-collection-ado.md)   
 [Objeto de conjunto de registros (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)
