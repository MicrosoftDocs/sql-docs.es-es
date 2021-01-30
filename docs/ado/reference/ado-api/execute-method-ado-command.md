---
description: Método Execute (Command ADO)
title: Método execute (comando de ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command15::Execute
- Command15::raw_Execute
helpviewer_keywords:
- Execute method [ADO]
ms.assetid: f84a5ff3-0528-4ad7-9bea-9a15103378dd
author: rothja
ms.author: jroth
ms.openlocfilehash: 1bc27de85c9c8e1e47ec3fb89ec8fadce07713c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171160"
---
# <a name="execute-method-ado-command"></a>Método Execute (Command ADO)
Ejecuta la consulta, la instrucción SQL o el procedimiento almacenado especificado en la propiedad [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md) o [CommandStream](../../../ado/reference/ado-api/commandstream-property-ado.md) del [objeto de comando](../../../ado/reference/ado-api/command-object-ado.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Set recordset = command.Execute( RecordsAffected, Parameters, Options )  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Devuelve una referencia de objeto de [conjunto de registros](../../../ado/reference/ado-api/recordset-object-ado.md) , una secuencia o **nada**.  
  
#### <a name="parameters"></a>Parámetros  
 *RecordsAffected*  
 Opcional. Variable **larga** a la que el proveedor devuelve el número de registros a los que afecta la operación. El parámetro *RecordsAffected* solo se aplica a las consultas de acción o los procedimientos almacenados. *RecordsAffected* no devuelve el número de registros devueltos por un procedimiento almacenado o una consulta que devuelve resultados. Para obtener esta información, use la propiedad [RecordCount](../../../ado/reference/ado-api/recordcount-property-ado.md) . El método **Execute** no devolverá la información correcta cuando se usa con **adAsyncExecute**, simplemente porque cuando un comando se ejecuta de forma asincrónica, es posible que aún no se conozca el número de registros afectados en el momento en que el método vuelve.  
  
 *Parámetros*  
 Opcional. Matriz **de** valores de parámetros que se usa junto con la cadena de entrada o el flujo especificado en **CommandText** o **CommandStream**. (Los parámetros de salida no devolverán los valores correctos cuando se pasan en este argumento).  
  
 *Opciones*  
 Opcional. Un valor **largo** que indica cómo el proveedor debe evaluar la propiedad [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md) o [CommandStream](../../../ado/reference/ado-api/commandstream-property-ado.md) del objeto de [comando](../../../ado/reference/ado-api/command-object-ado.md) . Puede ser un valor de máscara de máscara con valores [CommandTypeEnum](../../../ado/reference/ado-api/commandtypeenum.md) y/o [ExecuteOptionEnum](../../../ado/reference/ado-api/executeoptionenum.md) . Por ejemplo, puede usar **adCmdText** y **adExecuteNoRecords** en combinación si desea que ADO evalúe el valor de la propiedad **CommandText** como texto e indique que el comando debe descartar y no devolver ningún registro que pueda generarse cuando se ejecute el texto de comando.  
  
> [!NOTE]
>  Use el valor **adExecuteNoRecords** de **ExecuteOptionEnum** para mejorar el rendimiento al minimizar el procesamiento interno. Si se especificó **adExecuteStream** , se omiten las opciones **adAsyncFetch** y **adAsynchFetchNonBlocking** . No use los valores **CommandTypeEnum** de **adCmdFile** o **adCmdTableDirect** con **Execute**. Estos valores solo se pueden usar como opciones con los métodos [Open](../../../ado/reference/ado-api/open-method-ado-recordset.md) y [Requery](../../../ado/reference/ado-api/requery-method.md) de un **conjunto de registros**.  
  
## <a name="remarks"></a>Observaciones  
 El uso del método **Execute** en un objeto **Command** ejecuta la consulta especificada en la propiedad **CommandText** o en la propiedad **CommandStream** del objeto.  
  
 Los resultados se devuelven en un **conjunto de registros** (de forma predeterminada) o como un flujo de información binaria. Para obtener una secuencia binaria, especifique **adExecuteStream** en *Opciones* y, a continuación, proporcione un flujo estableciendo **Command. Properties ("flujo de salida")**. Se puede especificar un objeto de **secuencia** de ADO para recibir los resultados, o bien se puede especificar otro objeto de flujo, como el objeto de respuesta de IIS. Si no se especificó ningún flujo antes de llamar a **Execute** con **adExecuteStream**, se produce un error. La posición de la secuencia en la devolución de **Execute** es específica del proveedor.  
  
 Si el comando no está pensado para devolver resultados (por ejemplo, una consulta de actualización de SQL), el proveedor no devuelve **nada** mientras se especifique la opción **adExecuteNoRecords** ; de lo contrario, Execute devuelve un **conjunto de registros** cerrado. Algunos lenguajes de aplicación permiten omitir este valor devuelto si no se desea ningún **conjunto de registros** .  
  
 **Execute** genera un error si el usuario especifica un valor para **CommandStream** cuando **CommandType** es **adCmdStoredProc**, **adCmdTable** o **adCmdTableDirect**.  
  
 Si la consulta tiene parámetros, se usan los valores actuales de los parámetros del objeto de **comando** , a menos que se reemplacen con los valores de parámetro pasados con la llamada a **Execute** . Puede invalidar un subconjunto de los parámetros omitiendo nuevos valores para algunos de los parámetros al llamar al método **Execute** . El orden en el que se especifican los parámetros es el mismo orden en el que el método los pasa. Por ejemplo, si hay cuatro (o más) parámetros y desea pasar valores nuevos solo para los parámetros primero y cuarto, pasaría `Array(var1,,,var4)` como el argumento *Parameters* .  
  
> [!NOTE]
>  Los parámetros de salida no devolverán los valores correctos cuando se pasan en el argumento *Parameters* .  
  
 Cuando finalice esta operación, se emitirá un evento [ExecuteComplete](../../../ado/reference/ado-api/executecomplete-event-ado.md) .  
  
> [!NOTE]
>  Cuando se emiten comandos que contienen direcciones URL, las que usan el esquema http invocarán automáticamente el [proveedor de Microsoft OLE DB para la publicación en Internet](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Para obtener más información, consulte [direcciones URL absolutas y relativas](../../../ado/guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto Command (ADO)](../../../ado/reference/ado-api/command-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de los métodos Execute, Requery y Clear (VB)](../../../ado/reference/ado-api/execute-requery-and-clear-methods-example-vb.md)   
 [Ejemplo de métodos Execute, Requery y Clear (VBScript)](../../../ado/reference/ado-api/execute-requery-and-clear-methods-example-vbscript.md)   
 [Ejemplo de los métodos Execute, Requery y Clear (VC + +)](../../../ado/reference/ado-api/execute-requery-and-clear-methods-example-vc.md)   
 [CommandStream (propiedad, ADO)](../../../ado/reference/ado-api/commandstream-property-ado.md)   
 [CommandText (propiedad, ADO)](../../../ado/reference/ado-api/commandtext-property-ado.md)   
 [CommandTypeEnum](../../../ado/reference/ado-api/commandtypeenum.md)   
 [Método execute (conexión ADO)](../../../ado/reference/ado-api/execute-method-ado-connection.md)   
 [Evento ExecuteComplete (ADO)](../../../ado/reference/ado-api/executecomplete-event-ado.md)
