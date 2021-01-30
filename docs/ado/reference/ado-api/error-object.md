---
description: Objeto de error
title: Error (objeto) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Error
helpviewer_keywords:
- error object [ADO]
ms.assetid: a175d453-fa55-4f49-9ede-a26d83177919
author: rothja
ms.author: jroth
ms.openlocfilehash: eba5b63bc766161556779dd261393b0dfe06822d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171195"
---
# <a name="error-object"></a>Objeto de error
Contiene detalles sobre los errores de acceso a datos que pertenecen a una única operación que implica al proveedor.  
  
## <a name="remarks"></a>Observaciones  
 Cualquier operación relacionada con objetos ADO puede generar uno o más errores de proveedor. Cuando se produce cada error, se colocan uno o varios objetos de **error** en la colección de [errores](../../../ado/reference/ado-api/errors-collection-ado.md) del objeto de [conexión](../../../ado/reference/ado-api/connection-object-ado.md) . Cuando otra operación de ADO genera un error, la colección de **errores** se borra y el nuevo conjunto de objetos de **error** se coloca en la colección de **errores** .  
  
> [!NOTE]
>  Cada objeto de **error** representa un error de proveedor concreto, no un error de ADO. Los errores de ADO se exponen al mecanismo de control de excepciones en tiempo de ejecución. Por ejemplo, en Microsoft Visual Basic, la aparición de un error específico de ADO desencadenará un evento **on error** y aparecerá en el objeto **error** . Para obtener una lista completa de los errores de ADO, vea el tema [ErrorValueEnum](../../../ado/reference/ado-api/errorvalueenum.md) .  
  
 Puede leer las propiedades de un objeto de **error** para obtener detalles específicos de cada error, incluido lo siguiente:  
  
-   La propiedad [Description](../../../ado/reference/ado-api/description-property.md) , que contiene el texto del error. Esta es la propiedad predeterminada.  
  
-   La propiedad [Number](../../../ado/reference/ado-api/number-property-ado.md) , que contiene el valor entero **largo** de la constante de error.  
  
-   Propiedad de [origen](../../../ado/reference/ado-api/source-property-ado-error.md) , que identifica el objeto que ha generado el error. Esto es especialmente útil cuando hay varios objetos de **error** en la colección de **errores** después de una solicitud a un origen de datos.  
  
-   Las propiedades [SQLSTATE](../../../ado/reference/ado-api/sqlstate-property.md) y [NativeError](../../../ado/reference/ado-api/nativeerror-property-ado.md) , que proporcionan información de los orígenes de datos SQL.  
  
 Cuando se produce un error de proveedor, se coloca en la colección de **errores** del objeto de **conexión** . ADO admite la devolución de varios errores mediante una única operación de ADO para permitir información de error específica del proveedor. Para obtener esta información de error enriquecida en un controlador de errores, utilice las características de detección de errores adecuadas del lenguaje o entorno con el que está trabajando y, a continuación, use bucles anidados para enumerar las propiedades de cada objeto de **error** en la colección de **errores** .  
  
> [!NOTE]
>  **Usuarios de Microsoft Visual Basic y VBScript** Si no hay ningún objeto de **conexión** válido, tendrá que recuperar la información de error del objeto de **error** .  
  
 Del mismo modo que los proveedores, ADO borra el objeto de **información de error OLE** antes de realizar una llamada que podría generar un error de proveedor nuevo. Sin embargo, la colección de **errores** en el objeto de **conexión** se borra y rellena solo cuando el proveedor genera un nuevo error o cuando se llama al método [Clear](../../../ado/reference/ado-api/clear-method-ado.md) .  
  
 Algunas propiedades y métodos devuelven advertencias que aparecen como objetos de **error** en la colección de **errores** , pero no detienen la ejecución de un programa. Antes de llamar a los métodos [Resync](../../../ado/reference/ado-api/resync-method.md), [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md)o [CancelBatch](../../../ado/reference/ado-api/cancelbatch-method-ado.md) en un objeto de [conjunto de registros](../../../ado/reference/ado-api/recordset-object-ado.md) ; el método [Open](../../../ado/reference/ado-api/open-method-ado-connection.md) en un objeto **Connection** ; o bien, establezca la propiedad [Filter](../../../ado/reference/ado-api/filter-property.md) en un objeto de **conjunto de registros** , llame al método **Clear** en la colección de **errores** . De este modo, puede leer la propiedad [Count](../../../ado/reference/ado-api/count-property-ado.md) de la colección de errores para probar si **hay** advertencias devueltas.  
  
 El objeto de **error** no es seguro para el scripting.  
  
 Esta sección contiene el siguiente tema.  
  
-   [Eventos, métodos y propiedades del objeto Error](../../../ado/reference/ado-api/error-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades Description, HelpContext, HelpFile, NativeError, Number, Source y SQLState (VB)](../../../ado/reference/ado-api/description-helpcontext-helpfile-nativeerror-number-source-example-vb.md)   
 [Ejemplo de las propiedades Description, HelpContext, HelpFile, NativeError, Number, Source y SQLState (VC + +)](../../../ado/reference/ado-api/description-helpcontext-helpfile-nativeerror-number-source-example-vc.md)   
 [Connection (objeto) (ADO)](../../../ado/reference/ado-api/connection-object-ado.md)   
 [Colección de errores (ADO)](../../../ado/reference/ado-api/errors-collection-ado.md)   
 [Apéndice A: Proveedores](../../../ado/guide/appendixes/appendix-a-providers.md)
