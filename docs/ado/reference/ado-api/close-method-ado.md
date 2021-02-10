---
description: Close (método) (ADO)
title: Close (método) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::Close
- _Stream::Close
- _Record::Close
helpviewer_keywords:
- Close method [ADO]
ms.assetid: 3cdf27d1-a180-4cff-8e42-95dec5fb1b55
author: rothja
ms.author: jroth
ms.openlocfilehash: 0e7f1edd109f9ea4fe66c3f77a732aeec27e41c2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100027083"
---
# <a name="close-method-ado"></a>Close (método) (ADO)
Cierra un objeto abierto y los objetos dependientes.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
object.Close  
```  
  
## <a name="remarks"></a>Observaciones  
 Use el método **Close** para cerrar una [conexión](./connection-object-ado.md), un [registro](./record-object-ado.md), un [conjunto de registros](./recordset-object-ado.md)o un objeto de [flujo](./stream-object-ado.md) para liberar los recursos del sistema asociados. Al cerrar un objeto no se quita de la memoria; puede cambiar la configuración de sus propiedades y volver a abrirla más tarde. Para eliminar completamente un objeto de la memoria, cierre el objeto y, a continuación, establezca la variable de objeto en *Nothing* (en Visual Basic).  
  
## <a name="connection"></a>Conexión  
 El uso del método **Close** para cerrar un objeto de **conexión** también cierra todos los objetos de **conjunto de registros** activos asociados a la conexión. Se conservará un objeto de [comando](./command-object-ado.md) asociado al objeto de **conexión** que se va a cerrar, pero ya no se asociará a un objeto de **conexión** . es decir, su propiedad [ActiveConnection](./activeconnection-property-ado.md) se establecerá en **Nothing**. Además, la colección de [parámetros](./parameters-collection-ado.md) del objeto de **comando** se borrará de los parámetros definidos por el proveedor.  
  
 Después, puede llamar al método [Open](./open-method-ado-connection.md) para volver a establecer la conexión con el mismo origen de datos u otro distinto. Mientras se cierra el objeto de **conexión** , al llamar a los métodos que requieren una conexión abierta con el origen de datos se genera un error.  
  
 Al cerrar un objeto de **conexión** mientras hay objetos de **conjunto de registros** abiertos en la conexión, se revierten los cambios pendientes en todos los objetos de **conjunto de registros** . Al cerrar explícitamente un objeto de **conexión** (llamando al método **Close** ) mientras una transacción está en curso, se genera un error. Si un objeto de **conexión** cae fuera del ámbito mientras una transacción está en curso, ADO revierte automáticamente la transacción.  
  
## <a name="recordset-record-stream"></a>Conjunto de registros, registro, secuencia  
 Al usar el método **Close** para cerrar un objeto de **conjunto de registros**, **registro** o **secuencia** , se liberan los datos asociados y cualquier acceso exclusivo que haya tenido a los datos a través de este objeto concreto. Después, puede llamar al método [Open](./open-method-ado-recordset.md) para volver a abrir el objeto con los mismos atributos, o modificados.  
  
 Mientras se cierra un objeto de **conjunto de registros** , al llamar a cualquier método que requiera un cursor activo, se genera un error.  
  
 Si una edición está en curso mientras está en modo de actualización inmediata, al llamar al método **Close** se genera un error; en su lugar, llame primero al método [Update](./update-method.md) o [CancelUpdate](./cancelupdate-method-ado.md) . Si cierra el objeto de **conjunto de registros** mientras está en modo de actualización por lotes, se perderán todos los cambios realizados desde la última llamada a [UpdateBatch](./updatebatch-method.md) .  
  
 Si usa el método [Clone](./clone-method-ado.md) para crear copias de un objeto de **conjunto de registros** abierto, el cierre del original o de un clon no afectará a ninguna de las demás copias.  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Objeto de conexión (ADO)](./connection-object-ado.md)  
        [Objeto Record (ADO)](./record-object-ado.md)  
    :::column-end:::
    :::column:::
        [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
        [Objeto de secuencia (ADO)](./stream-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Consulte también  
 [Ejemplo de los métodos Open y Close (VB)](./open-and-close-methods-example-vb.md)   
 [Ejemplo de los métodos Open y Close (VBScript)](./open-and-close-methods-example-vbscript.md)   
 [Ejemplo de los métodos Open y Close (VC + +)](./open-and-close-methods-example-vc.md)   
 [Open (método) (conexión de ADO)](./open-method-ado-connection.md)   
 [Open (método) (conjunto de registros ADO)](./open-method-ado-recordset.md)   
 [Save (método)](./save-method.md)