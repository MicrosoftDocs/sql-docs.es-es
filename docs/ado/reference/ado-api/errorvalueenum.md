---
description: ErrorValueEnum
title: ErrorValueEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ErrorValueEnum
helpviewer_keywords:
- ErrorValueEnum enumeration [ADO]
ms.assetid: 9469ba3a-5e4f-4a10-bbb8-a51a6c9660ea
author: rothja
ms.author: jroth
ms.openlocfilehash: 238c8bf3d0c21625b9317690f70adbe16bd5075f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171163"
---
# <a name="errorvalueenum"></a>ErrorValueEnum
Especifica el tipo de error en tiempo de ejecución de ADO.  
  
 Se muestran tres formas del número de error:  
  
-   Decimal positivo: los dos bytes inferiores del número completo en formato decimal. Este número se muestra en el cuadro de diálogo Visual Basic mensaje de error predeterminado. Por ejemplo, error en tiempo de ejecución ' 3707 '.  
  
-   Decimal negativo: traducción decimal del número de error completo.  
  
-   Hexadecimal-The representación hexadecimal del número de error completo. El código de la instalación de Windows está en el cuarto dígito. El código de instalación de los números de *error de ADO es.* Por ejemplo: 0x800 ***A** _0E7B.  
  
> [!NOTE]
>  Se pueden pasar OLE DB errores a la aplicación ADO. Normalmente, se pueden identificar mediante un código de servicio de Windows de _4 *. Por ejemplo, 0x800 * * * 4* *_.  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|_ *adErrBoundToCommand**|3707-2146824581 0x800A0E7B|No se puede cambiar la propiedad **ActiveConnection** de un objeto de **conjunto de registros** que tiene un objeto **Command** como su origen.|  
|**adErrCannotComplete**|3732-2146824556 0x800A0E94|El servidor no puede completar la operación.|  
|**adErrCantChangeConnection**|3748-2146824540 0x800A0EA4|Se denegó la conexión. La nueva conexión que solicitó tiene características diferentes de la que ya se está usando.|  
|**adErrCantChangeProvider**|3220-2146825068 0X800A0C94|El proveedor proporcionado difiere del que ya se está usando.|  
|**adErrCantConvertvalue**|3724-2146824564 0x800A0E8C|No se puede convertir el valor de los datos por motivos distintos a un error de coincidencia de signos o desbordamiento de datos. Por ejemplo, la conversión tendría datos truncados.|  
|**adErrCantCreate**|3725-2146824563 0x800A0E8D|No se puede establecer ni recuperar el valor de los datos porque el tipo de datos del campo era desconocido o el proveedor no tenía suficientes recursos para realizar la operación.|  
|**adErrCatalogNotSet**|3747-2146824541 0x800A0EA3|La operación requiere un **ParentCatalog** válido.|  
|**adErrColumnNotOnThisRow**|3726-2146824562 0x800A0E8E|El registro no contiene este campo.|  
|**adErrDataConversion**|3421-2146824867 0x800A0D5D|La aplicación usa un valor de tipo incorrecto para la operación actual.|  
|**adErrDataOverflow**|3721-2146824567 0x800A0E89|El valor de los datos es demasiado grande para representarlo en el tipo de datos del campo.|  
|**adErrDelResOutOfScope**|3738-2146824550 0x800A0E9A|La dirección URL del objeto que se va a eliminar está fuera del ámbito del registro actual.|  
|**adErrDenyNotSupported**|3750-2146824538 0x800A0EA6|El proveedor no admite restricciones de uso compartido.|  
|**adErrDenyTypeNotSupported**|3751-2146824537 0x800A0EA7|El proveedor no admite el tipo solicitado de restricción de uso compartido.|  
|**adErrFeatureNotAvailable**|3251-2146825037 0x800A0CB3|El objeto o proveedor no puede realizar la operación solicitada.|  
|**adErrFieldsUpdateFailed**|3749-2146824539 0x800A0EA5|Error al actualizar los campos. Para obtener más información, examine la propiedad **status** de los objetos de campo individuales.|  
|**adErrIllegalOperation**|3219-2146825069 0x800A0C93|La operación no se permite en este contexto.|  
|**adErrIntegrityViolation**|3719-2146824569 0x800A0E87|El valor de los datos entra en conflicto con las restricciones de integridad del campo.|  
|**adErrInTransaction**|3246-2146825042 0x800A0CAE|El objeto de **conexión** no se puede cerrar explícitamente mientras se está en una transacción.|  
|**adErrInvalidArgument**|3001-2146825287 0x800A0BB9|Los argumentos son del tipo incorrecto, están fuera del intervalo aceptable o están en conflicto entre sí.|  
|**adErrInvalidConnection**|3709-2146824579 0x800A0E7D|No se puede usar la conexión para realizar esta operación. Está cerrado o no es válido en este contexto.|  
|**adErrInvalidParamInfo**|3708-2146824580 0x800A0E7C|El objeto de **parámetro** no está definido correctamente. Se proporcionó información incoherente o incompleta.|  
|**adErrInvalidTransaction**|3714-2146824574 0x800A0E82|La coordinación de la transacción no es válida o no se ha iniciado.|  
|**adErrInvalidURL**|3729-2146824559 0x800A0E91|La dirección URL contiene caracteres no válidos. Asegúrese de que la dirección URL está escrita correctamente.|  
|**adErrItemNotFound**|3265-2146825023 0x800A0CC1|No se encuentra el elemento en la colección que corresponde al nombre o el ordinal solicitados.|  
|**adErrNoCurrentRecord**|3021-2146825267 0x800A0BCD|**BOF** o **EOF** es true, o se ha eliminado el registro actual. La operación solicitada requiere un registro actual.|  
|**adErrNotExecuting**|3715-2146824573 0x800A0E83|No se puede realizar la operación mientras no se ejecuta.|  
|**adErrNotReentrant**|3710-2146824578 0x800A0E7E|No se puede realizar la operación al procesar el evento.|  
|**adErrObjectClosed**|3704-2146824584 0x800A0E78|No se permite la operación cuando el objeto está cerrado.|  
|**adErrObjectInCollection**|3367-2146824921 0x800A0D27|El objeto ya está en la colección. No se puede anexar.|  
|**adErrObjectNotSet**|3420-2146824868 0x800A0D5C|El objeto ya no es válido.|  
|**adErrObjectOpen**|3705-2146824583 0x800A0E79|La operación no se permite cuando el objeto está abierto.|  
|**adErrOpeningFile**|3002-2146825286 0x800A0BBA|No se pudo abrir el archivo.|  
|**adErrOperationCancelled**|3712-2146824576 0x800A0E80|El usuario ha cancelado la operación.|  
|**adErrOutOfSpace**|3734-2146824554 0x800A0E96|No se puede realizar la operación. El proveedor no puede obtener suficiente espacio de almacenamiento.|  
|**adErrPermissionDenied**|3720-2146824568 0x800A0E88|Los permisos insuficientes evitan escribir en el campo.|  
|**adErrProviderFailed**|3000-2146825288 0x800A0BB8|El proveedor no ha realizado la operación solicitada.|  
|**adErrProviderNotFound**|3706-2146824582 0x800A0E7A|No se encuentra el proveedor. Puede que no esté instalado correctamente.|  
|**adErrReadFile**|3003-2146825285 0x800A0BBB|No se pudo leer el archivo.|  
|**adErrResourceExists**|3731-2146824557 0x800A0E93|No se puede realizar la operación de copia. El objeto denominado por la dirección URL de destino ya existe. Especifique **adCopyOverwrite** para reemplazar el objeto.|  
|**adErrResourceLocked**|3730-2146824558 0x800A0E92|El objeto representado por la dirección URL especificada está bloqueado por uno o más procesos. Espere hasta que el proceso haya finalizado e intente la operación de nuevo.|  
|**adErrResourceOutOfScope**|3735-2146824553 0x800A0E97|La dirección URL de origen o de destino está fuera del ámbito del registro actual.|  
|**adErrSchemaViolation**|3722-2146824566 0x800A0E8A|El valor de los datos entra en conflicto con el tipo de datos o con las restricciones del campo.|  
|**adErrSignMismatch**|3723-2146824565 0x800A0E8B|Error de conversión porque el valor de los datos tenía signo y el tipo de datos del campo utilizado por el proveedor no tenía signo.|  
|**adErrStillConnecting**|3713-2146824575 0x800A0E81|No se puede realizar la operación mientras se conecta de forma asincrónica.|  
|**adErrStillExecuting**|3711-2146824577 0x800A0E7F|No se puede realizar la operación mientras se ejecuta de forma asincrónica.|  
|**adErrTreePermissionDenied**|3728-2146824560 0x800A0E90|Los permisos son insuficientes para tener acceso al árbol o al subárbol.|  
|**adErrUnavailable**|3736-2146824552 0x800A0E98|La operación no se completó y el estado no está disponible. El campo puede no estar disponible o no se intentó realizar la operación.|  
|**adErrUnsafeOperation**|3716-2146824572 0x800A0E84|La configuración de seguridad de este equipo impide el acceso a un origen de datos de otro dominio.|  
|**adErrURLDoesNotExist**|3727-2146824561 0x800A0E8F|La dirección URL de origen o el elemento primario de la dirección URL de destino no existen.|  
|**adErrURLNamedRowDoesNotExist**|3737-2146824551 0x800A0E99|El registro nombrado por esta dirección URL no existe.|  
|**adErrVolumeNotFound**|3733-2146824555 0x800A0E95|El proveedor no encuentra el dispositivo de almacenamiento indicado por la dirección URL. Asegúrese de que la dirección URL está escrita correctamente.|  
|**adErrWriteFile**|3004-2146825284 0x800A0BBC|No se pudo escribir en el archivo.|  
|**adWrnSecurityDialog**|3717-2146824571 0x800A0E85|Solo para uso interno. No debe usarse.|  
|**adWrnSecurityDialogHeader**|3718-2146824570 0x800A0E86|Solo para uso interno. No debe usarse.|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Paquete: **com. ms. wfc. Data**  
  
 Solo se definen los siguientes subconjuntos de equivalentes de ADO/WFC.  
  
|Constante|  
|--------------|  
|AdoEnums. ErrorValue. BOUNDTOCOMMAND|  
|AdoEnums. ErrorValue. Subversion|  
|AdoEnums. ErrorValue. FEATURENOTAVAILABLE|  
|AdoEnums. ErrorValue. ILLEGALOPERATION|  
|AdoEnums. ErrorValue. intransaction|  
|AdoEnums. ErrorValue. INVALIDARGUMENT|  
|AdoEnums. ErrorValue. INVALIDCONNECTION|  
|AdoEnums. ErrorValue. INVALIDPARAMINFO|  
|AdoEnums. ErrorValue. ITEMNOTFOUND|  
|AdoEnums. ErrorValue. NOCURRENTRECORD|  
|AdoEnums. ErrorValue. NOTEXECUTING|  
|AdoEnums. ErrorValue. NOTREENTRANT|  
|AdoEnums. ErrorValue. OBJECTCLOSED|  
|AdoEnums. ErrorValue. OBJECTINCOLLECTION|  
|AdoEnums. ErrorValue. OBJECTNOTSET|  
|AdoEnums. ErrorValue. OBJECTOPEN|  
|AdoEnums. ErrorValue. OPERATIONCANCELLED|  
|AdoEnums. ErrorValue. PROVIDERNOTFOUND|  
|AdoEnums. ErrorValue. STILLCONNECTING|  
|AdoEnums. ErrorValue. STILLEXECUTING|  
|AdoEnums. ErrorValue. UNSAFEOPERATION|  
  
## <a name="applies-to"></a>Se aplica a  
 [Propiedad Number (ADO)](../../../ado/reference/ado-api/number-property-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Captura de códigos de Error de ADO](../../../ado/guide/appendixes/ado-error-codes.md)
