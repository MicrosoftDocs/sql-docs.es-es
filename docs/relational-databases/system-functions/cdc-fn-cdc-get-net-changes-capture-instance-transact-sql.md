---
description: cdc.fn_cdc_get_net_changes_&lt;capture_instance&gt; (Transact-SQL)
title: cdc.fn_cdc_get_net_changes_ &lt; capture_instance &gt; (TRANSACT-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- fn_cdc_get_net_changes_<capture_instance>
- change data capture [SQL Server], querying metadata
- cdc.fn_cdc_get_net_changes_<capture_instance>
ms.assetid: 43ab0d1b-ead4-471c-85f3-f6c4b9372aab
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1b16d1da947970652be4879880ce1cfccf7ca796
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196207"
---
# <a name="cdcfn_cdc_get_net_changes_ltcapture_instancegt-transact-sql"></a>cdc.fn_cdc_get_net_changes_&lt;capture_instance&gt; (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila de cambio neto por cada fila de origen modificada en el intervalo de números de secuencia de registro (LSN) especificado.  
  
 **Espere, ¿qué es un LSN?** Cada registro del [registro de transacciones SQL Server](../logs/the-transaction-log-sql-server.md) se identifica de forma única mediante un número de secuencia de registro (LSN). Los LSN se ordenan de manera que si LSN2 es mayor que LSN1, el cambio descrito por la entrada de registro a la que hace referencia LSN2 se produce **después** del cambio descrito por el LSN de la entrada de registro.  
  
 El LSN de una entrada de registro en la que se ha producido un evento significativo puede ser útil para construir secuencias de restauración correctas. Dado que LSN se ordenan, puede compararlos para determinar si son iguales y desigualdades (es decir, \<, > , =, \<=, > =). Estas comparaciones son útiles para generar secuencias de restauración.  
  
 Cuando una fila de origen tiene varios cambios durante el intervalo de LSN, la función de enumeración que se describe a continuación devuelve una sola fila que refleja el contenido final de la fila. Por ejemplo, si una transacción inserta una fila en la tabla de origen y una transacción subsiguiente en el intervalo de LSN actualiza una o más columnas de esa fila, la función devuelve solo **una** fila, que incluye los valores de columna actualizados.  
  
 Esta función de enumeración se crea cuando se habilita una tabla de origen para la captura de datos modificados y se especifica seguimientos de cambios en la red. Para habilitar el seguimiento de cambios netos, la tabla de origen debe tener una clave principal o índice único. Se deriva el nombre de la función y se usa el formato cdc.fn_cdc_get_net_changes_ *capture_instance*, donde *capture_instance* es el valor especificado para la instancia de captura cuando la tabla de origen se habilitó para la captura de datos modificados. Para obtener más información, vea [sys.sp_cdc_enable_table &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
cdc.fn_cdc_get_net_changes_capture_instance ( from_lsn , to_lsn , '<row_filter_option>' )  
  
<row_filter_option> ::=  
{ all  
 | all with mask  
 | all with merge  
}  
```  
  
## <a name="arguments"></a>Argumentos  
 *from_lsn*  
 El LSN que representa el extremo inferior del rango de LSN que se va a incluir en el conjunto de resultados. *from_lsn* es **binario (10)**.  
  
 Solo se incluyen en el conjunto de resultados las filas de la tabla [CDC. &#91;capture_instance&#93;_CT](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md) cambio con un valor en _ _ $ start_lsn mayor o igual que *from_lsn* .  
  
 *to_lsn*  
 El LSN que representa el extremo superior del rango de LSN que se va a incluir en el conjunto de resultados. *to_lsn* es **binario (10)**.  
  
 Solo se incluyen en el conjunto de resultados las filas del [capture_instance CDC. &#91;&#93;_CT](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md) tabla de cambios con un valor en _ _ $ start_lsn menor o igual que *from_lsn* o igual a *to_lsn* .  
  
 *<row_filter_option>* :: = {ALL | ALL with Mask | All with Merge}  
 Una opción que rige el contenido de las columnas de metadatos y las filas devueltas en el conjunto de resultados. Puede ser una de las siguientes opciones:  
  
 all  
 Devuelve el LSN del último cambio de la fila y la operación necesaria para aplicar la fila en las columnas de metadatos _ _ $ start_lsn y \_ \_ $Operation. La columna \_ \_ $Update _mask siempre es NULL.  
  
 all with mask  
 Devuelve el LSN del último cambio de la fila y la operación necesaria para aplicar la fila en las columnas de metadatos _ _ $ start_lsn y \_ \_ $Operation. Además, cuando una operación de actualización devuelve ( \_ \_ $Operation = 4) las columnas capturadas modificadas en la actualización se marcan en el valor devuelto en \_ \_ $Update _mask.  
  
 todos con combinación  
 Devuelve el LSN del último cambio realizado en la fila en las columnas de metadatos __$start_lsn. La columna \_ \_ $Operation tendrá uno de dos valores: 1 para eliminar y 5 para indicar que la operación necesaria para aplicar el cambio es una inserción o una actualización. La columna \_ \_ $Update _mask siempre es NULL.  
  
 Debido a que la lógica para determinar la operación precisa de un cambio determinado agrega mayor complejidad a la consulta, esta opción se ha diseñado para mejorar el rendimiento de las consultas cuando es suficiente indicar que la operación necesaria para aplicar los datos de cambio es una inserción o una actualización, pero no es necesario distinguir explícitamente entre las dos. Esta opción resulta muy útil en entornos de destino donde las operaciones Combinar están directamente disponibles, como en un entorno [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|__$start_lsn|**binary(10)**|Número de secuencia de registro (LSN) asociado con la transacción de confirmación para el cambio.<br /><br /> Todos los cambios confirmados en la misma transacción comparten el mismo LSN de confirmación. Por ejemplo, si una operación de actualización en la tabla de origen modifica dos columnas en dos filas, la tabla de cambios contendrá cuatro filas, cada una con el mismo _ _ $ start_lsnvalue.|  
|__$operation|**int**|Identifica la operación del lenguaje de manipulación de datos (DML) necesaria para aplicar la fila de datos modificados al origen de datos de destino.<br /><br /> Si el valor del parámetro row_filter_option es todos o todos con máscara, el valor de esta columna puede ser uno de los siguientes:<br /><br /> 1 = eliminar<br /><br /> 2 = insertar<br /><br /> 4 = actualizar<br /><br /> Si el valor del parámetro row_filter_option es todos con combinación, el valor de esta columna puede ser uno de los siguientes:<br /><br /> 1 = eliminar|  
|__$update_mask|**varbinary(128)**|Máscara de bits con un bit que corresponde a cada columna capturada identificada para la instancia de captura. Este valor tiene todos los bits definidos establecidos en 1 si __$operation = 1 o 2. Cuando \_ \_ $Operation = 3 o 4, solo los bits que corresponden a las columnas que han cambiado se establecen en 1.|  
|*\<captured source table columns>*|Varía|Las columnas restantes devueltas por la función son las columnas de la tabla de origen que se identificaron como columnas capturadas cuando se creó la instancia de captura. Si no se especificó ninguna columna en la lista de columnas capturadas, se devuelven todas las columnas de la tabla de origen.|  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol fijo de servidor sysadmin o al rol fijo de base de datos db_owner. Para el resto de usuarios, requiere el permiso SELECT en todas las columnas capturadas en la tabla de origen y, si se ha definido un rol de acceso para la instancia de captura, la pertenencia a ese rol de base de datos. Cuando el autor de la llamada no tiene permiso para ver los datos de origen, la función devuelve el error 208 (Nombre de objeto no válido).  
  
## <a name="remarks"></a>Observaciones  
 Si el intervalo del LSN especificado no se encuentra dentro de la escala temporal del seguimiento de cambios de la instancia de captura, la función devuelve el error 208 (Nombre de objeto no válido).

 Las modificaciones en el identificador único de una fila harán que fn_cdc_get_net_changes muestren en su lugar el comando de actualización inicial con un comando DELETE y, a continuación, INSERT.  Este comportamiento es necesario para realizar el seguimiento de la clave antes y después del cambio.
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se utiliza la función `cdc.fn_cdc_get_net_changes_HR_Department` para notificar los cambios netos realizados en la tabla de origen `HumanResources.Department` durante un intervalo de tiempo específico.  
  
 En primer lugar, se usa la función `GETDATE` para marcar el inicio del intervalo de tiempo. Después de aplicar varias instrucciones DML a la tabla de origen, se llama de nuevo a la función `GETDATE` para identificar el final del intervalo de tiempo. A continuación, se usa la función [Sys.fn_cdc_map_time_to_lsn](../../relational-databases/system-functions/sys-fn-cdc-map-time-to-lsn-transact-sql.md) para asignar el intervalo de tiempo a un intervalo de consulta de captura de datos modificados limitado por valores LSN. Por último, se consulta la función `cdc.fn_cdc_get_net_changes_HR_Department` para obtener los cambios de la red realizados en la tabla de origen durante el intervalo de tiempo. Observe que la fila que se inserta y, a continuación, se elimina no aparece en el conjunto de resultados devuelto por la función. Esto se debe a que una fila que primero se agrega y luego se elimina dentro de una ventana de consulta no genera ningún cambio de la red en la tabla de origen para el intervalo. Antes de ejecutar este ejemplo, primero debe ejecutar el ejemplo B en [sys.sp_cdc_enable_table &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md).  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @begin_time datetime, @end_time datetime, @from_lsn binary(10), @to_lsn binary(10);  
-- Obtain the beginning of the time interval.  
SET @begin_time = GETDATE() -1;  
-- DML statements to produce changes in the HumanResources.Department table.  
INSERT INTO HumanResources.Department (Name, GroupName)  
VALUES (N'MyDept', N'MyNewGroup');  
  
UPDATE HumanResources.Department  
SET GroupName = N'Resource Control'  
WHERE GroupName = N'Inventory Management';  
  
DELETE FROM HumanResources.Department  
WHERE Name = N'MyDept';  
  
-- Obtain the end of the time interval.  
SET @end_time = GETDATE();  
-- Map the time interval to a change data capture query range.  
SET @from_lsn = sys.fn_cdc_map_time_to_lsn('smallest greater than or equal', @begin_time);  
SET @to_lsn = sys.fn_cdc_map_time_to_lsn('largest less than or equal', @end_time);  
  
-- Return the net changes occurring within the query window.  
SELECT * FROM cdc.fn_cdc_get_net_changes_HR_Department(@from_lsn, @to_lsn, 'all');  
```  
  
## <a name="see-also"></a>Consulte también  
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)   
 [sys.fn_cdc_map_time_to_lsn &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/sys-fn-cdc-map-time-to-lsn-transact-sql.md)   
 [sys.sp_cdc_enable_table &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md)   
 [sys.sp_cdc_help_change_data_capture &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)   
 [Acerca de la captura de datos modificados &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
  
