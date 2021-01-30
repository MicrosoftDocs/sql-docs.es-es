---
description: FieldStatusEnum
title: FieldStatusEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- FieldStatusEnum
helpviewer_keywords:
- FieldStatusEnum enumeration [ADO]
ms.assetid: e06da1e2-303f-41b2-a3b0-61e233da152c
author: rothja
ms.author: jroth
ms.openlocfilehash: 968a4205a12e9f6ff36c3bff512cf41ae60ca469
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167351"
---
# <a name="fieldstatusenum"></a>FieldStatusEnum
Especifica el [Estado](./status-property-ado-field.md) de un [objeto de campo](./field-object.md).  
  
 Los valores ** \* adFieldPending* _ indican la operación que provocó el establecimiento del estado y pueden combinarse con otros valores de estado.  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|_ *adFieldAlreadyExists**|26|Indica que el campo especificado ya existe.|  
|**adFieldBadStatus**|12|Indica que se ha enviado un valor de estado no válido desde ADO al proveedor de OLE DB. Entre las posibles causas se incluyen un proveedor OLE DB 1,0 o 1,1, o una combinación incorrecta de [valor](./value-property-ado.md) y [Estado](./status-property-ado-field.md).|  
|**adFieldCannotComplete**|20|Indica que el servidor de la dirección URL especificada por el [origen](./source-property-ado-record.md) no pudo completar la operación.|  
|**adFieldCannotDeleteSource**|23|Indica que durante una operación de movimiento, se ha movido un árbol o un subárbol a una nueva ubicación, pero no se ha podido eliminar el origen.|  
|**adFieldCantConvertValue**|2|Indica que el campo no se puede recuperar ni almacenar sin pérdida de datos.|  
|**adFieldCantCreate**|7|Indica que no se pudo agregar el campo porque el proveedor superó una limitación (como el número de campos permitidos).|  
|**adFieldDataOverflow**|6|Indica que los datos devueltos del proveedor han desbordado el tipo de datos del campo.|  
|**adFieldDefault**|13|Indica que se utilizó el valor predeterminado para el campo al establecer los datos.|  
|**adFieldDoesNotExist**|16|Indica que el campo especificado no existe.|  
|**adFieldIgnore**|15|Indica que este campo se omitió al establecer los valores de datos en el origen. El proveedor no establece ningún valor.|  
|**adFieldIntegrityViolation**|10|Indica que el campo no se puede modificar porque es una entidad calculada o derivada.|  
|**adFieldInvalidURL**|17|Indica que la dirección URL del origen de datos contiene caracteres no válidos.|  
|**adFieldIsNull**|3|Indica que el proveedor devolvió un valor VARIANT de tipo VT_NULL y que el campo no está vacío.|  
|**adFieldOK**|0|Predeterminada. Indica que el campo se agregó o eliminó correctamente.|  
|**adFieldOutOfSpace**|22|Indica que el proveedor no puede obtener suficiente espacio de almacenamiento para completar una operación de movimiento o copia.|  
|**adFieldPendingChange**|0x40000|Indica que el campo se ha eliminado y, a continuación, se ha vuelto a agregar, quizás con un tipo de datos diferente, o que el valor del campo que tenía anteriormente un estado **adFieldOK** ha cambiado. La forma final del campo modificará la colección [Fields](./fields-collection-ado.md) después de que se llame al método [Update](./update-method.md) .|  
|**adFieldPendingDelete**|0x20000|Indica que la operación de **eliminación** provocó el establecimiento del estado. El campo se ha marcado para su eliminación de la colección **Fields** después de que se llame al método **Update** .|  
|**adFieldPendingInsert**|0x10000|Indica que la operación de **anexado** provocó el establecimiento del estado. El **campo** se ha marcado para agregarse a la colección **Fields** después de que se llame al método **Update** .|  
|**adFieldPendingUnknown**|0x80000|Indica que el proveedor no puede determinar qué operación causó el establecimiento del estado del campo.|  
|**adFieldPendingUnknownDelete**|0x100000|Indica que el proveedor no puede determinar qué operación causó el establecimiento del estado del campo y que el campo se eliminará de la colección **Fields** después de que se llame al método **Update** .|  
|**adFieldPermissionDenied**|9|Indica que el campo no se puede modificar porque está definido como de solo lectura.|  
|**adFieldReadOnly**|24|Indica que el campo del origen de datos se define como de solo lectura.|  
|**adFieldResourceExists**|19|Indica que el proveedor no pudo realizar la operación porque ya existe un objeto en la dirección URL de destino y no puede sobrescribir el objeto.|  
|**adFieldResourceLocked**|18|Indica que el proveedor no pudo realizar la operación porque el origen de datos está bloqueado por una o varias aplicaciones o procesos.|  
|**adFieldResourceOutOfScope**|25|Indica que una dirección URL de origen o de destino está fuera del ámbito del registro actual.|  
|**adFieldSchemaViolation**|11|Indica que el valor infringió la restricción de esquema de origen de datos para el campo.|  
|**adFieldSignMismatch**|5|Indica que el valor de los datos devueltos por el proveedor estaba firmado, pero el tipo de datos del valor del campo ADO no estaba firmado.|  
|**adFieldTruncated**|4|Indica que los datos de longitud variable se truncaron al leer desde el origen de datos.|  
|**adFieldUnavailable**|8|Indica que el proveedor no pudo determinar el valor al leer desde el origen de datos. Por ejemplo, la fila se acaba de crear, el valor predeterminado de la columna no estaba disponible y aún no se ha especificado un nuevo valor.|  
|**adFieldVolumeNotFound**|21|Indica que el proveedor no puede localizar el volumen de almacenamiento indicado por la dirección URL.|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Estas constantes no tienen equivalentes de ADO/WFC.  
  
## <a name="applies-to"></a>Se aplica a  
 [Propiedad Status (Field ADO)](./status-property-ado-field.md)