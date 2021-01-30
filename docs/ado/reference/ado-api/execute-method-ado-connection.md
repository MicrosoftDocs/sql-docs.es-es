---
description: Execute (método) (conexión de ADO)
title: Método execute (conexión ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::Execute
- Connection15::raw_Execute
helpviewer_keywords:
- Execute method [ADO]
ms.assetid: 03c69320-96b2-4d85-8d49-a13b13e31578
author: rothja
ms.author: jroth
ms.openlocfilehash: 2fc73324160dd46a6d626c0db889a4213c9fc438
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167492"
---
# <a name="execute-method-ado-connection"></a>Execute (método) (conexión de ADO)
Ejecuta la consulta, la instrucción SQL, el procedimiento almacenado o el texto específico del proveedor especificados.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Set recordset = connection.Execute (CommandText, RecordsAffected, Options)  
Set recordset = connection.Execute (CommandText, RecordsAffected, Options)  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Devuelve una referencia de objeto de [objeto de conjunto de registros (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md) .  
  
#### <a name="parameters"></a>Parámetros  
 *CommandText*  
 Valor de **cadena** que contiene la instrucción SQL, el procedimiento almacenado, una dirección URL o un texto específico del proveedor que se va a ejecutar. **Opcionalmente**, se pueden usar nombres de tabla, pero solo si el proveedor es compatible con SQL. Por ejemplo, si se usa un nombre de tabla "Customers", ADO agregará automáticamente la sintaxis de Select de SQL estándar para formar y pasar "SELECT * FROM Customers" como una [!INCLUDE[tsql](../../../includes/tsql-md.md)] instrucción al proveedor.  
  
 *RecordsAffected*  
 Opcional. Variable **larga** a la que el proveedor devuelve el número de registros a los que afecta la operación.  
  
 *Opciones*  
 Opcional. Un valor **Long** que indica cómo el proveedor debe evaluar el argumento CommandText. Puede ser una máscara de máscara de uno o más valores [CommandTypeEnum](../../../ado/reference/ado-api/commandtypeenum.md) o [ExecuteOptionEnum](../../../ado/reference/ado-api/executeoptionenum.md) .  
  
 **Nota:** Use el valor **adExecuteNoRecords** de **ExecuteOptionEnum** para mejorar el rendimiento al minimizar el procesamiento interno y las aplicaciones que va a migrar desde Visual Basic 6,0.  
  
 No utilice **adExecuteStream** con el método **Execute** de un objeto **Connection** .  
  
 No use los valores CommandTypeEnum de adCmdFile o adCmdTableDirect con Execute. Estos valores solo se pueden usar como opciones con el [método Open (conjunto de registros ADO)](../../../ado/reference/ado-api/open-method-ado-recordset.md) y los métodos de [método Requery](../../../ado/reference/ado-api/requery-method.md) de un **conjunto de registros**.  
  
## <a name="remarks"></a>Observaciones  
 El uso del método **Execute** en un objeto de [objeto de conexión (ADO)](../../../ado/reference/ado-api/connection-object-ado.md) ejecuta la consulta que se pasa al método en el argumento CommandText en la conexión especificada. Si el argumento CommandText especifica una consulta que devuelve filas, los resultados generados por la ejecución se almacenan en un nuevo objeto de **conjunto de registros** . Si el comando no está pensado para devolver resultados (por ejemplo, una consulta de actualización de SQL), el proveedor no devuelve **nada** mientras se especifique la opción **adExecuteNoRecords** ; de lo contrario, Execute devuelve un **conjunto de registros** cerrado.  
  
 El objeto de **conjunto de registros** devuelto siempre es un cursor de solo lectura y de solo avance. Si necesita un objeto de **conjunto de registros** con más funcionalidad, cree primero un objeto de conjunto de **registros** con los valores de propiedad deseados y, a continuación, use el método [Open Method (ADO Recordset)](../../../ado/reference/ado-api/open-method-ado-recordset.md) del objeto de **conjunto de registros** para ejecutar la consulta y devolver el tipo de cursor deseado.  
  
 El contenido del argumento *CommandText* es específico del proveedor y puede ser una sintaxis SQL estándar o cualquier formato de comando especial que admita el proveedor.  
  
 Cuando finalice esta operación, se emitirá un evento ExecuteComplete.  
  
> [!NOTE]
>  Las direcciones URL que usan el esquema http invocarán automáticamente el [proveedor de Microsoft OLE DB para la publicación en Internet](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Para obtener más información, consulte [direcciones URL absolutas y relativas](../../../ado/guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conexión (ADO)](../../../ado/reference/ado-api/connection-object-ado.md)
