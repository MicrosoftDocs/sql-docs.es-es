---
description: Método Resync
title: Método Resync | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset20::raw_Resync
- Fields::Resync
- Recordset20::Resync
- Fields::raw_Resync
helpviewer_keywords:
- Resync method [ADO]
ms.assetid: 73b355d4-a4c0-434b-bfc4-039b1c76b32e
author: rothja
ms.author: jroth
ms.openlocfilehash: b1c4156c56a3c6722fb856511ccb3be1b14c7f5e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170361"
---
# <a name="resync-method"></a>Método Resync
Actualiza los datos del objeto de conjunto de [registros](./recordset-object-ado.md) actual, o la colección de [campos](./fields-collection-ado.md) de un objeto de [registro](./record-object-ado.md) , de la base de datos subyacente.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Recordset.Resync AffectRecords, ResyncValues Record.Fields.Resync ResyncValues  
```  
  
#### <a name="parameters"></a>Parámetros  
 *AffectRecords*  
 Opcional. Valor de [AffectEnum](./affectenum.md) que determina el número de registros a los que afectará el método **Resync** . El valor predeterminado es **adAffectAll**. Este valor no está disponible con el método **Resync** de la colección **Fields** de un objeto **Record** .  
  
 *ResyncValues*  
 Opcional. Valor [ResyncEnum](./resyncenum.md) que especifica si se sobrescriben los valores subyacentes. El valor predeterminado es **adResyncAllValues**.  
  
## <a name="remarks"></a>Observaciones  
  
## <a name="recordset"></a>DataRecordsets  
 Use el método **Resync** para volver a sincronizar los registros del **conjunto de registros** actual con la base de datos subyacente. Esto resulta útil si usa un cursor estático o de solo avance, pero desea ver los cambios en la base de datos subyacente.  
  
 Si establece la propiedad [CursorLocation](./cursorlocation-property-ado.md) en **adUseClient**, **Resync** solo está disponible para los objetos de conjunto de **registros** que no son de solo lectura.  
  
 A diferencia del método [Requery](./requery-method.md) , el método **Resync** no vuelve a ejecutar el comando subyacente del objeto de **conjunto de registros** . Los nuevos registros de la base de datos subyacente no estarán visibles.  
  
 Si se produce un error al intentar volver a sincronizar debido a un conflicto con los datos subyacentes (por ejemplo, otro usuario ha eliminado un registro), el proveedor devuelve advertencias a la colección de [errores](./errors-collection-ado.md) y se produce un error en tiempo de ejecución. Use la propiedad [Filter](./filter-property.md) (**adFilterConflictingRecords**) y la propiedad [status](./status-property-ado-recordset.md) para buscar registros con conflictos.  
  
 Si se establecen las propiedades dinámicas [tabla única](./unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md) y [comando de resincronización](./resync-command-property-dynamic-ado.md) , y el **conjunto de registros** es el resultado de ejecutar una operación de combinación en varias tablas, el método **Resync** ejecutará el comando proporcionado en la propiedad **comando Resync** solo en la tabla denominada en la propiedad de **tabla única** .  
  
## <a name="fields"></a>Campos  
 Use el método **Resync** para volver a sincronizar los valores de la colección **Fields** de un objeto **Record** con el origen de datos subyacente. Este método no afecta a la propiedad [Count](./count-property-ado.md) .  
  
 Si *ResyncValues* se establece en **adResyncAllValues** (el valor predeterminado), se sincronizan las propiedades [UnderlyingValue](./underlyingvalue-property.md), [Value](./value-property-ado.md)y [OriginalValue](./originalvalue-property-ado.md) de los objetos de [campo](./field-object.md) de la colección. Si *ResyncValues* se establece en **adResyncUnderlyingValues**, solo se sincroniza la propiedad **UnderlyingValue** .  
  
 El valor de la propiedad **status** de cada objeto **Field** en el momento de la llamada también afecta al comportamiento de **Resync**. En el caso de los objetos de **campo** que tienen valores de **Estado** de **adFieldPendingUnknown** o **adFieldPendingInsert**, **Resync** no tiene ningún efecto. En el caso de los valores de **Estado** de **adFieldPendingChange** o **adFieldPendingDelete**, **Resync** sincroniza los valores de datos de los campos que todavía existen en el origen de datos.  
  
 **Resync** no modificará los valores de **Estado** de los objetos de **campo** a menos que se produzca un error cuando se llame a **Resync** . Por ejemplo, si el campo ya no existe, el proveedor devolverá un valor de **Estado** adecuado para el objeto de **campo** , como **adFieldDoesNotExist**. Los valores de **Estado** devueltos se pueden combinar lógicamente dentro del valor de la propiedad **status** .  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Fields (colección) (ADO)](./fields-collection-ado.md)  
    :::column-end:::
    :::column:::
        [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Consulte también  
 [Ejemplo del método Resync (VB)](./resync-method-example-vb.md)   
 [Ejemplo del método Resync (VC + +)](./resync-method-example-vc.md)   
 [Clear (método) (ADO)](./clear-method-ado.md)   
 [Propiedad UnderlyingValue](./underlyingvalue-property.md)