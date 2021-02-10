---
description: DeleteRecord (método, ADO)
title: DeleteRecord (método) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Record::raw_DeleteRecord
- _Record::DeleteRecord
helpviewer_keywords:
- DeleteRecord method [ADO]
ms.assetid: 2726498c-dbd8-4266-983b-ae7d62c39142
author: rothja
ms.author: jroth
ms.openlocfilehash: cb6439e59a34beb9b4e8b505f4244c48c173b552
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025487"
---
# <a name="deleterecord-method-ado"></a>DeleteRecord (método, ADO)
Elimina una entidad representada por un [registro](../../../ado/reference/ado-api/record-object-ado.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Record.DeleteRecord Source, Async  
```  
  
#### <a name="parameters"></a>Parámetros  
 *Origen*  
 Opcional. Valor de **cadena** que contiene una dirección URL que identifica la entidad (por ejemplo, el archivo o el directorio) que se va a eliminar. Si se omite *source* o especifica una cadena vacía, se elimina la entidad representada por el [registro](../../../ado/reference/ado-api/record-object-ado.md) actual. Si el registro es un registro de colección ([RecordType](../../../ado/reference/ado-api/recordtype-property-ado.md) de **adCollectionRecord**, como un directorio), todos los elementos secundarios (por ejemplo, subdirectorios) también se eliminarán.  
  
 *Asincrónico*  
 Opcional. Valor **booleano** que, cuando es **true**, especifica que la operación de eliminación es asincrónica.  
  
## <a name="remarks"></a>Observaciones  
 Se puede producir un error en las operaciones en el objeto representado por este **registro** una vez que se completa este método. Después de llamar a **DeleteRecord**, el **registro** debe cerrarse porque el comportamiento del **registro** puede ser impredecible dependiendo de Cuándo el proveedor actualice el **registro** con el origen de datos.  
  
 Si este **registro** se obtuvo de un [conjunto de registros](../../../ado/reference/ado-api/recordset-object-ado.md), los resultados de esta operación no se reflejarán inmediatamente en el **conjunto de registros**. Actualice el **conjunto de registros** . para ello, cierre y vuelva a abrirlo, o bien ejecute el método de [reconsulta](../../../ado/reference/ado-api/requery-method.md) del **conjunto de registros** , el método [Update](../../../ado/reference/ado-api/update-method.md) o el método [Resync](../../../ado/reference/ado-api/resync-method.md) .  
  
> [!NOTE]
>  Las direcciones URL que usan el esquema http invocarán automáticamente el [proveedor de Microsoft OLE DB para la publicación en Internet](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Para obtener más información, consulte [direcciones URL absolutas y relativas](../../../ado/guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto Record (ADO)](../../../ado/reference/ado-api/record-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Método Delete (colección Fields de ADO)](../../../ado/reference/ado-api/delete-method-ado-fields-collection.md)   
 [Método Delete (colección de parámetros de ADO)](../../../ado/reference/ado-api/delete-method-ado-parameters-collection.md)   
 [Delete (método) (conjunto de registros ADO)](../../../ado/reference/ado-api/delete-method-ado-recordset.md)
