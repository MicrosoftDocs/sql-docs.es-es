---
description: sp_lock (Transact-SQL)
title: sp_lock (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_lock_TSQL
- sp_lock
dev_langs:
- TSQL
helpviewer_keywords:
- sp_lock
ms.assetid: 9eaa0ec2-2ad9-457c-ae48-8da92a03dcb0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 09e753e8b821c3ad8029a7ed4d81bbe183160279
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185388"
---
# <a name="sp_lock-transact-sql"></a>sp_lock (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Genera información acerca de los bloqueos.  
  
> [!IMPORTANT]  
> [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Para obtener información acerca de los bloqueos en [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] , utilice la [Sys.dm_tran_locks](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md) vista de administración dinámica.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sp_lock [ [ @spid1 = ] 'session ID1' ] [ , [@spid2 = ] 'session ID2' ]  
[ ; ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @spid1 = ] 'session ID1'` Es un [!INCLUDE[ssDE](../../includes/ssde-md.md)] número de ID. de sesión de **Sys.dm_exec_sessions** para el que el usuario desea información de bloqueo. la *sesión ID1* es de **tipo int** y su valor predeterminado es NULL. Ejecute **sp_who** para obtener información del proceso sobre la sesión. Si no se especifica *Session ID1* , se muestra información sobre todos los bloqueos.  
  
`[ @spid2 = ] 'session ID2'` Es otro [!INCLUDE[ssDE](../../includes/ssde-md.md)] número de ID. de sesión de **Sys.dm_exec_sessions** que puede tener un bloqueo al mismo tiempo que *ID1 de sesión* y sobre el que el usuario también desea obtener información. la *sesión ID2* es de **tipo int** y su valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 El conjunto de resultados **sp_lock** contiene una fila por cada bloqueo mantenido por las sesiones especificadas en los parámetros **\@ spid1** y **\@ spid2** . Si no se especifica **\@ spid1** ni **\@ spid2** , el conjunto de resultados informa de los bloqueos de todas las sesiones activas actualmente en la instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**spid**|**smallint**|Número de Id. de sesión del [!INCLUDE[ssDE](../../includes/ssde-md.md)] del proceso que solicita el bloqueo.|  
|**DBID**|**smallint**|El número de identificación de la base de datos en la que se mantiene el bloqueo. Puede utilizar la función DB_NAME() para identificar la base de datos.|  
|**ObjId**|**int**|El número de identificación del objeto en el que se mantiene el bloqueo. Puede utilizar la función OBJECT_NAME() en la base de datos relacionada para identificar el objeto. El valor 99 constituye un caso especial que indica un bloqueo en una de las páginas del sistema utilizadas para registrar la asignación de páginas en una base de datos.|  
|**IndId**|**smallint**|Número de identificación del índice en el que se mantiene el bloqueo.|  
|**Type**|**NCHAR (4)**|Tipo de bloqueo.<br /><br /> RID = Bloqueo en una única fila de una tabla identificada por un identificador de fila (RID).<br /><br /> KEY = Bloqueo en un índice que protege un intervalo de claves en transacciones serializables.<br /><br /> PAG = Bloqueo en una página de datos o de índices.<br /><br /> EXT = Bloqueo en una extensión.<br /><br /> TAB = Bloqueo en toda una tabla, incluidos todos los datos y los índices.<br /><br /> DB = Bloqueo en una base de datos.<br /><br /> FIL = Bloqueo en un archivo de base de datos.<br /><br /> APP = Bloqueo en un recurso especificado por la aplicación.<br /><br /> MD = Bloqueos de metadatos o información de catálogo.<br /><br /> HBT = bloqueo en un montón o árbol B (HoBT). Esta información está incompleta en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> AU = Bloqueo en una unidad de asignación. Esta información está incompleta en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**Recurso**|**NCHAR (32)**|El valor que identifica el recurso bloqueado. El formato del valor depende del tipo de recurso identificado en la columna de **tipo** :<br /><br /> **Tipo** de Valor: valor de **recurso**<br /><br /> RID: un identificador con formato fileid:pagenumber:rid, donde fileid identifica el archivo que contiene la página, pagenumber identifica la página que contiene la fila y rid identifica la fila específica en la página. fileid coincide con la columna **file_id** de la vista de catálogo **Sys.database_files** .<br /><br /> KEY: un número hexadecimal utilizado internamente por [!INCLUDE[ssDE](../../includes/ssde-md.md)].<br /><br /> PAG: un número con formato idDeArchivo:númeroDePágina, donde idDeArchivo identifica el archivo que contiene la página y númeroDePágina identifica la página.<br /><br /> EXT: un número que identifica la primera página de la extensión. El número tiene un formato idDeArchivo:númeroDePágina.<br /><br /> PESTAÑA: no se proporciona información porque la tabla ya está identificada en la columna **ObjId** .<br /><br /> DB: no se proporciona información porque la base de datos ya está identificada en la columna **DBID** .<br /><br /> FIL: el identificador del archivo, que coincide con la columna **file_id** de la vista de catálogo **Sys.database_files** .<br /><br /> APP: un identificador único para el recurso de la aplicación que se está bloqueando. En el formato DbPrincipleId: \<first two to 16 characters of the resource string> \<hashed value> .<br /><br /> MD: varía por tipo de recurso. Para obtener más información, vea la descripción de la columna **resource_description** en [Sys.dm_tran_locks &#40;&#41;de Transact-SQL](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).<br /><br /> HBT: no se proporciona información. En su lugar, use la vista de administración dinámica de **Sys.dm_tran_locks** .<br /><br /> AU: no se proporciona información. En su lugar, use la vista de administración dinámica de **Sys.dm_tran_locks** .|  
|**Modo**|**nvarchar (8)**|El modo de bloqueo solicitado. Puede ser:<br /><br /> NULL = No se concede acceso al recurso. Sirve como marcador de posición.<br /><br /> Sch-S = Estabilidad del esquema. Garantiza que un elemento de un esquema, como una tabla o un índice, no se quite mientras una sesión mantenga un bloqueo de estabilidad del esquema sobre él.<br /><br /> Sch-M = Modificación del esquema. Debe mantenerlo cualquier sesión que desee cambiar el esquema del recurso especificado. Garantiza que ninguna otra sesión se refiera al objeto indicado.<br /><br /> S = Compartido. La sesión que lo mantiene recibe acceso compartido al recurso.<br /><br /> U = Actualizar. Indica que se ha obtenido un bloqueo de actualización sobre recursos que finalmente se pueden actualizar. Se utiliza para evitar una forma común de interbloqueo que tiene lugar cuando varias sesiones bloquean recursos para una posible actualización más adelante.<br /><br /> X = Exclusivo. La sesión que lo mantiene recibe acceso exclusivo al recurso.<br /><br /> IS = Intención compartida. Indica la intención de establecer bloqueos S en algún recurso subordinado de la jerarquía de bloqueos.<br /><br /> IU = Actualizar intención. Indica la intención de establecer bloqueos U en algún recurso subordinado de la jerarquía de bloqueos.<br /><br /> IX = intención exclusiva. Indica la intención de colocar bloqueos X en algunos recursos subordinados en la jerarquía de bloqueos.<br /><br /> SIU = Actualizar intención compartida. Indica el acceso compartido a un recurso con la intención de obtener bloqueos de actualización sobre recursos subordinados en la jerarquía de bloqueos.<br /><br /> SIX = Intención compartida exclusiva. Indica acceso compartido a un recurso con la intención de obtener bloqueos exclusivos sobre recursos subordinados de la jerarquía de bloqueos.<br /><br /> UIX = Actualizar intención exclusiva. Indica un bloqueo de actualización en un recurso con la intención de adquirir bloqueos exclusivos sobre recursos subordinados en la jerarquía de bloqueos.<br /><br /> BU = Actualización masiva. Utilizado en las operaciones masivas.<br /><br /> RangeS_S = Intervalo de claves compartido y bloqueo de recurso compartido. Indica recorrido de intervalo serializable.<br /><br /> RangeS_U = Intervalo de claves compartido y bloqueo de recurso de actualización. Indica recorrido de actualización serializable.<br /><br /> RangeI_N = Insertar intervalo de claves y bloqueo de recurso Null. Se utiliza para probar los intervalos antes de insertar una clave nueva en un índice.<br /><br /> RangeI_S = Bloqueo de conversión de intervalo de claves. Creado por una superposición de bloqueos RangeI_N y S.<br /><br /> RangeI_U = Bloqueo de conversión de intervalo de claves creado por una superposición de bloqueos RangeI_N y U.<br /><br /> RangeI_X = Bloqueo de conversión de intervalo de claves creado por una superposición de bloqueos RangeI_N y X.<br /><br /> RangeX_S = Bloqueo de conversión de rango de claves creado por una superposición de bloqueos RangeI_N y RangeS_S pestillo.<br /><br /> RangeX_U = Bloqueo de conversión de intervalo de claves creado por una superposición de bloqueos RangeI_N y RangeS_U.<br /><br /> RangeX_X = Intervalo de claves exclusivo y bloqueo de recurso exclusivo. Es un bloqueo de conversión que se utiliza cuando se actualiza una clave de un intervalo.|  
|**Estado**|**tipo**|Estado de solicitud de bloqueo:<br /><br /> CNVRT: el bloqueo se está convirtiendo desde otro modo, pero la conversión es bloqueada por otro proceso que mantiene un bloqueo con un modo en conflicto.<br /><br /> GRANT: se ha obtenido el bloqueo.<br /><br /> WAIT: el bloqueo es bloqueado por otro proceso que mantiene un bloqueo con un modo en conflicto.|  
  
## <a name="remarks"></a>Observaciones  
 Los usuarios pueden controlar el bloqueo de las operaciones de lectura:  
  
-   Si utilizan SET TRANSACTION ISOLATION LEVEL para especificar el nivel de bloqueo de una sesión. Para obtener más sintaxis y restricciones, vea [SET TRANSACTION ISOlation LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
  
-   Si utilizan sugerencias de tabla de bloqueo para especificar el nivel de bloqueo de una referencia individual de una tabla en una cláusula FROM. Para obtener más sintaxis y restricciones, vea [sugerencias de tabla &#40;&#41;de Transact-SQL ](../../t-sql/queries/hints-transact-sql-table.md).  
  
 Todas las transacciones distribuidas no asociadas a una sesión son transacciones huérfanas. El [!INCLUDE[ssDE](../../includes/ssde-md.md)] asigna a todas las transacciones distribuidas huérfanas el valor de SPID -2, lo que facilita al usuario la identificación de las transacciones distribuidas de bloqueo. Para obtener más información, vea [Usar transacciones marcadas para recuperar bases de datos relacionadas sistemáticamente &#40;modelo de recuperación completa&#41;](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md).  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso VIEW SERVER STATE.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-listing-all-locks"></a>A. Mostrar todos los bloqueos  
 En el ejemplo siguiente se muestra información acerca de todos los bloqueos mantenidos en una instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
```sql  
USE master;  
GO  
EXEC sp_lock;  
GO  
```  
  
### <a name="b-listing-a-lock-from-a-single-server-process"></a>B. Mostrar un bloqueo de un proceso de servidor único  
 En el ejemplo siguiente se muestra información, incluidos los bloqueos, acerca del proceso con Id. `53`.  
  
```sql  
USE master;  
GO  
EXEC sp_lock 53;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)   
 [DB_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/db-name-transact-sql.md)   
 [KILL &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-transact-sql.md)   
 [OBJECT_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/object-name-transact-sql.md)   
 [sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.dm_os_tasks &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md)   
 [sys.dm_os_threads &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-threads-transact-sql.md)  
  
  
