---
description: Método NextRecordset (ADO)
title: NextRecordset (método) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 03/20/2018
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- NextRecordset
- Recordset15::NextRecordset
- Recordset15::raw_NextRecordset
helpviewer_keywords:
- NextRecordset method [ADO]
ms.assetid: ab1fa449-a695-4987-b1ee-bc68f89418dd
author: rothja
ms.author: jroth
ms.openlocfilehash: 3388a5f37fa8d82aa951f1cc82eb9f43556d9d37
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041585"
---
# <a name="nextrecordset-method-ado"></a>Método NextRecordset (ADO)
Borra el objeto de [conjunto de registros](./recordset-object-ado.md) actual y devuelve el siguiente **conjunto de registros** avanzando por una serie de comandos.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Set recordset2 = recordset1.NextRecordset(RecordsAffected )  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Devuelve un objeto de **conjunto de registros** . En el modelo de sintaxis, *recordset1* y *Recordset2* pueden ser el mismo objeto de **conjunto de registros** , o puede usar objetos independientes. Al usar objetos de **conjunto de registros** independientes, se generará un error al restablecer la propiedad **ActiveConnection** en el **conjunto de registros** original (*recordset1*) después de llamar a **NextRecordset** .  
  
#### <a name="parameters"></a>Parámetros  
 *RecordsAffected*  
 Opcional. Variable de **tipo Long** en la que el proveedor devuelve el número de registros afectados por la operación actual.  
  
> [!NOTE]
>  Este parámetro solo devuelve el número de registros afectados por una operación. no devuelve un recuento de registros de una instrucción SELECT utilizada para generar el **conjunto de registros**.  
  
## <a name="remarks"></a>Observaciones  
 Use el método **NextRecordset** para devolver los resultados del comando siguiente en una instrucción de comando compuesta o de un procedimiento almacenado que devuelve varios resultados. Si abre un objeto de **conjunto de registros** basado en una instrucción de comando compuesta (por ejemplo, "SELECT \* from Table1; SELECT \* from Tabla2 ") mediante el [método Execute](./execute-method-ado-command.md) en [un comando](./command-object-ado.md) o el método [Open](./open-method-ado-recordset.md) en un conjunto de **registros**, ADO solo ejecuta el primer comando y devuelve los resultados al *conjunto de registros*. Para tener acceso a los resultados de los comandos subsiguientes en la instrucción, llame al método **NextRecordset** .  
  
 Siempre y cuando haya resultados adicionales y el **conjunto de registros** que contenga las instrucciones compuestas no esté desconectado o calculado por los límites del proceso, el método **NextRecordset** seguirá devolviendo objetos de **conjunto de registros** . Si un comando de devolución de filas se ejecuta correctamente pero no devuelve ningún registro, el objeto de **conjunto de registros** devuelto estará abierto pero vacío. Para probar este caso, compruebe que las propiedades [BOF](./bof-eof-properties-ado.md) y [EOF](./bof-eof-properties-ado.md) son **verdaderas**. Si un comando que no devuelve filas se ejecuta correctamente, el objeto de **conjunto de registros** devuelto se cerrará, lo que se puede comprobar probando la propiedad [State](./state-property-ado.md) en el **conjunto de registros**. Cuando no hay más resultados, *Recordset* se establecerá en *Nothing*.  
  
 El método **NextRecordset** no está disponible en un objeto de **conjunto de registros** desconectado, donde [ActiveConnection](./activeconnection-property-ado.md) se ha establecido en **Nothing** (en Microsoft Visual Basic) o null (en otros lenguajes).  
  
 Si una edición está en curso mientras está en modo de actualización inmediata, al llamar al método **NextRecordset** se genera un error; Llame primero al método [Update](./update-method.md) o [CancelUpdate](./cancelupdate-method-ado.md) .  
  
 Para pasar parámetros para más de un comando en la instrucción compuesta rellenando la colección de [parámetros](./parameters-collection-ado.md) o pasando una matriz con la llamada a **Open** o **Execute** original, los parámetros deben estar en el mismo orden en la colección o matriz que sus respectivos comandos en la serie de comandos. Debe terminar de leer todos los resultados antes de leer los valores de los parámetros de salida.  
  
 El proveedor de OLE DB determina cuándo se ejecuta cada comando en una instrucción compuesta. Por ejemplo, el [proveedor de Microsoft OLE DB para SQL Server](../../guide/appendixes/microsoft-ole-db-provider-for-sql-server.md), ejecuta todos los comandos de un lote al recibir la instrucción compuesta. Los **conjuntos de registros** resultantes se devuelven simplemente cuando se llama a **NextRecordset**.  
  
 Sin embargo, es posible que otros proveedores ejecuten el siguiente comando en una instrucción solo después de que se llame a NextRecordset. Para estos proveedores, si cierra explícitamente el objeto de **conjunto de registros** antes de recorrer la instrucción de comando completa, ADO nunca ejecuta los comandos restantes.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo del método NextRecordset (VB)](./nextrecordset-method-example-vb.md)   
 [Ejemplo del método NextRecordset (VC ++)](./nextrecordset-method-example-vc.md)