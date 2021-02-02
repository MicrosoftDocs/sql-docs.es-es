---
description: sys.dm_db_index_physical_stats (Transact-SQL)
title: sys.dm_db_index_physical_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_db_index_physical_stats
- sys.dm_db_index_physical_stats_TSQL
- sys.dm_db_index_physical_stats
- dm_db_index_physical_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_index_physical_stats dynamic management function
- fragmentation [SQL Server]
ms.assetid: d294dd8e-82d5-4628-aa2d-e57702230613
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b8957c55955ba24131f6cc5692e17b86cd2154a4
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236158"
---
# <a name="sysdm_db_index_physical_stats-transact-sql"></a>sys.dm_db_index_physical_stats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve información de tamaño y fragmentación de los datos y los índices de la tabla o vista especificada en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En el caso de un índice, se devuelve una fila por cada nivel de árbol b en cada partición. En el caso de un montón, se devuelve una fila para la unidad de asignación IN_ROW_DATA en cada partición. En el caso de datos de objetos grandes (LOB), se devuelve una fila para la unidad de asignación LOB_DATA en cada partición. Si en la tabla hay datos de desbordamiento de fila, se devuelve una fila para la unidad de asignación ROW_OVERFLOW_DATA en cada partición. No devuelve información sobre los índices de almacén de columnas optimizadas en memoria xVelocity.  
  
> [!IMPORTANT]
> Si consulta **Sys.dm_db_index_physical_stats** en una instancia de servidor que hospeda una [réplica secundaria legible](../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)Always On, es posible que se produzca un problema de bloqueo de puesta al día. Esto se debe a que esta vista de administración dinámica adquiere un bloqueo IS en la tabla de usuario especificada o la vista que puede bloquear las solicitudes de un subproceso de REDO durante un bloqueo X en esa tabla o vista de usuario.  
  
 **Sys.dm_db_index_physical_stats** no devuelve información acerca de los índices optimizados para memoria. Para obtener información sobre el uso de índices optimizados para memoria, vea [sys.dm_db_xtp_index_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-index-stats-transact-sql.md).  
  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sys.dm_db_index_physical_stats (   
    { database_id | NULL | 0 | DEFAULT }  
  , { object_id | NULL | 0 | DEFAULT }  
  , { index_id | NULL | 0 | -1 | DEFAULT }  
  , { partition_number | NULL | 0 | DEFAULT }  
  , { mode | NULL | DEFAULT }  
)  
```  
  
## <a name="arguments"></a>Argumentos  
 *database_id* \| \| \| Valor predeterminado de NULL 0  
 Es el ID. de la base de datos. *database_id* es **smallint**. Las entradas válidas son el número de identificador de una base de datos, NULL, 0 y DEFAULT. El valor predeterminado es 0. NULL, 0 y DEFAULT son valores equivalentes en este contexto.  
  
 Especifique NULL para devolver información de todas las bases de datos en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si especifica NULL para *database_id*, también debe especificar null para *object_id*, *index_id* y *partition_number*.  
  
 Se puede especificar la función integrada [DB_ID](../../t-sql/functions/db-id-transact-sql.md). Al usar DB_ID sin especificar ningún nombre de base de datos, el nivel de compatibilidad de la base de datos actual debe ser 90 o superior.  
  
 *object_id* \| \| \| Valor predeterminado de NULL 0  
 Es el identificador de objeto de la tabla o vista donde está activado el índice. *object_id* es de **tipo int**.  
  
 Las entradas válidas son el número de identificador de una tabla o vista, NULL, 0 y DEFAULT. El valor predeterminado es 0. NULL, 0 y DEFAULT son valores equivalentes en este contexto. A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] , las entradas válidas también incluyen el nombre de la cola de Service Broker o el nombre de la tabla interna de la cola. Cuando se aplican los parámetros predeterminados (es decir, todos los objetos, todos los índices, etc.), la información de fragmentación de todas las colas se incluye en el conjunto de resultados.  
  
 Especifique NULL para devolver información de todas las tablas y vistas de la base de datos especificada. Si especifica NULL para *object_id*, también debe especificar null para *index_id* y *partition_number*.  
  
 *index_id* \| 0 \| null \| -1 \| predeterminado  
 Es el identificador del índice. *index_id* es de **tipo int**. Las entradas válidas son el número de identificación de un índice, 0 si *object_id* es un montón, null,-1 o default. El valor predeterminado es -1. NULL,-1 y DEFAULT son valores equivalentes en este contexto.  
  
 Especifique NULL para devolver información de todos los índices de una tabla o vista base. Si especifica NULL para *index_id*, también debe especificar null para *partition_number*.  
  
 *partition_number* \| \| \| Valor predeterminado de NULL 0  
 Es el número de partición en el objeto. *partition_number* es de **tipo int**. Las entradas válidas son las *partion_number* de un índice o montón, null, 0 o default. El valor predeterminado es 0. NULL, 0 y DEFAULT son valores equivalentes en este contexto.  
  
 Especifique NULL para obtener información sobre todas las particiones del objeto propietario.  
  
 *partition_number* se basa en 1. Un índice o montón sin particiones tiene *partition_number* establecida en 1.  
  
 *modo* \| de NULL ( \| valor predeterminado)  
 Es el nombre del modo. el *modo* especifica el nivel de examen que se utiliza para obtener las estadísticas. *mode* es **sysname**. Las entradas válidas son DEFAULT, NULL, LIMITED, SAMPLED o DETAILED. El valor predeterminado (NULL) es LIMITED.  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|database_id|**smallint**|Identificador de base de datos de la tabla o vista.|  
|object_id|**int**|Identificador de objeto de la tabla o vista en la que se encuentra el índice.|  
|index_id|**int**|Identificador de índice.<br /><br /> 0 = Montón.|  
|partition_number|**int**|Número de partición de base 1 en el objeto propietario, una tabla, vista o índice.<br /><br /> 1 = Montón o índice sin particiones.|  
|index_type_desc|**nvarchar(60)**|Descripción del tipo de índice:<br /><br /> HEAP<br /><br /> CLUSTERED INDEX<br /><br /> NONCLUSTERED INDEX<br /><br /> PRIMARY XML INDEX<br /><br /> EXTENDED INDEX<br /><br /> XML INDEX<br /><br /> Índice de asignación de almacén de columnas (interno)<br /><br /> Índice DELETEBUFFER de almacén de columnas (interno)<br /><br /> Índice DELETEBITMAP de almacén de columnas (interno)|  
|hobt_id|**bigint**|IDENTIFICADOR de árbol B o de montón del índice o de la partición.<br /><br /> Además de devolver el hobt_id de los índices definidos por el usuario, también devuelve el hobt_id de los índices de almacén de columnas internos.|  
|alloc_unit_type_desc|**nvarchar(60)**|Descripción del tipo de unidad de asignación:<br /><br /> IN_ROW_DATA<br /><br /> LOB_DATA<br /><br /> ROW_OVERFLOW_DATA<br /><br /> La unidad de asignación LOB_DATA contiene los datos que se almacenan en columnas de tipo **Text**, **ntext**, **Image**, **VARCHAR (Max)**, **nvarchar (Max)**, **varbinary (Max)** y **XML**. Para obtener más información, vea [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).<br /><br /> La unidad de asignación ROW_OVERFLOW_DATA contiene los datos que se almacenan en columnas de tipo **VARCHAR (n)**, **nvarchar (n)**, **varbinary (n)** y **sql_variant** que se han insertado de forma no consecutiva.|  
|index_depth|**tinyint**|Número de niveles del índice.<br /><br /> 1 = Montón, o unidad de asignación LOB_DATA o ROW_OVERFLOW_DATA.|  
|index_level|**tinyint**|Nivel actual del índice.<br /><br /> 0 para niveles hoja del índice, montones y unidades de asignación LOB_DATA o ROW_OVERFLOW_DATA.<br /><br /> Mayor que 0 para niveles de índice no hoja. *index_level* será el valor más alto en el nivel raíz de un índice.<br /><br /> Los niveles no hoja de los índices solo se procesan cuando *mode* = Detailed.|  
|avg_fragmentation_in_percent|**float**|Fragmentación lógica para índices o fragmentación de extensión para montones en la unidad de asignación IN_ROW_DATA.<br /><br /> El valor se mide como porcentaje y tiene en cuenta varios archivos. Para obtener definiciones de fragmentación lógica y de extensión, vea la sección Comentarios.<br /><br /> 0 para unidades de asignación LOB_DATA y ROW_OVERFLOW_DATA.<br /><br /> NULL para montones cuando *mode* = sampled.|  
|fragment_count|**bigint**|Número de fragmentos en el nivel hoja de una unidad de asignación IN_ROW_DATA. Para obtener más información sobre los fragmentos, vea la sección Comentarios.<br /><br /> NULL para niveles no hoja de un índice y unidades de asignación LOB_DATA o ROW_OVERFLOW_DATA.<br /><br /> NULL para montones cuando *mode* = sampled.|  
|avg_fragment_size_in_pages|**float**|Promedio de páginas en un fragmento en el nivel hoja de una unidad de asignación IN_ROW_DATA.<br /><br /> NULL para niveles no hoja de un índice y unidades de asignación LOB_DATA o ROW_OVERFLOW_DATA.<br /><br /> NULL para montones cuando *mode* = sampled.|  
|page_count|**bigint**|Número total de páginas de datos o de índice.<br /><br /> En el caso de un índice, el número total de páginas de índice en el nivel actual de un árbol b en la unidad de asignación IN_ROW_DATA.<br /><br /> En el caso de un montón, el número total de páginas de datos en la unidad de asignación IN_ROW_DATA.<br /><br /> En el caso de unidades de asignación LOB_DATA o ROW_OVERFLOW_DATA, el número total de páginas en la unidad de asignación.|  
|avg_page_space_used_in_percent|**float**|Porcentaje medio del espacio de almacenamiento de datos disponible utilizado en todas las páginas.<br /><br /> En el caso de un índice, el promedio se aplica al nivel actual del árbol b en la unidad de asignación IN_ROW_DATA.<br /><br /> En el caso de un montón, se trata del promedio de todas las páginas de datos en la unidad de asignación IN_ROW_DATA.<br /><br /> En el caso de unidades de asignación LOB_DATA o ROW_OVERFLOW_DATA, se trata del promedio de todas las páginas en la unidad de asignación.<br /><br /> NULL cuando *mode* = Limited.|  
|record_count|**bigint**|Número total de registros.<br /><br /> En el caso de un índice, el número total de registros se aplica al nivel actual del árbol b en la unidad de asignación IN_ROW_DATA.<br /><br /> En el caso de un montón, el número total de registros en la unidad de asignación IN_ROW_DATA.<br /><br /> **Nota:** En el caso de un montón, es posible que el número de registros devueltos por esta función no coincida con el número de filas que se devuelven al ejecutar SELECT COUNT ( \* ) en el montón. Esto es debido a que una fila puede contener varios registros. Por ejemplo, en algunas situaciones de una actualización, una única fila del montón puede tener un registro de reenvío y un registro reenviado como resultado de la actualización. Asimismo, la mayoría de las filas LOB de gran tamaño se dividen en varios registros en almacenamiento de LOB_DATA.<br /><br /> En el caso de unidades de asignación LOB_DATA o ROW_OVERFLOW_DATA, el número total de registros en toda la unidad de asignación.<br /><br /> NULL cuando *mode* = Limited.|  
|ghost_record_count|**bigint**|Número de registros fantasma preparados para su eliminación mediante la tarea de limpieza de registros fantasma en la unidad de asignación.<br /><br /> 0 para niveles no hoja de un índice en la unidad de asignación IN_ROW_DATA.<br /><br /> NULL cuando *mode* = Limited.|  
|version_ghost_record_count|**bigint**|Número de registros fantasma retenidos por una transacción de aislamiento de instantánea pendiente en una unidad de asignación.<br /><br /> 0 para niveles no hoja de un índice en la unidad de asignación IN_ROW_DATA.<br /><br /> NULL cuando *mode* = Limited.|  
|min_record_size_in_bytes|**int**|Tamaño mínimo del registro en bytes.<br /><br /> En el caso de un índice, el tamaño mínimo del registro se aplica al nivel actual del árbol b en la unidad de asignación IN_ROW_DATA.<br /><br /> En el caso de un montón, el tamaño mínimo del registro en la unidad de asignación IN_ROW_DATA.<br /><br /> En el caso de unidades de asignación LOB_DATA o ROW_OVERFLOW_DATA, el tamaño mínimo del registro en toda la unidad de asignación.<br /><br /> NULL cuando *mode* = Limited.|  
|max_record_size_in_bytes|**int**|Tamaño máximo del registro en bytes.<br /><br /> En el caso de un índice, el tamaño máximo del registro se aplica al nivel actual del árbol b en la unidad de asignación IN_ROW_DATA.<br /><br /> En el caso de un montón, el tamaño máximo del registro en la unidad de asignación IN_ROW_DATA.<br /><br /> En el caso de unidades de asignación LOB_DATA o ROW_OVERFLOW_DATA, el tamaño máximo del registro en toda la unidad de asignación.<br /><br /> NULL cuando *mode* = Limited.|  
|avg_record_size_in_bytes|**float**|Promedio de tamaño del registro en bytes.<br /><br /> En el caso de un índice, el promedio de tamaño del registro se aplica al nivel actual del árbol b en la unidad de asignación IN_ROW_DATA.<br /><br /> En el caso de un montón, el promedio de tamaño del registro en la unidad de asignación IN_ROW_DATA.<br /><br /> En el caso de unidades de asignación LOB_DATA o ROW_OVERFLOW_DATA, el promedio de tamaño del registro en toda la unidad de asignación.<br /><br /> NULL cuando *mode* = Limited.|  
|forwarded_record_count|**bigint**|Número de registros de un montón que han reenviado punteros a otra ubicación de datos. Este estado se produce durante una actualización, cuando no existe suficiente espacio para almacenar la nueva fila en la ubicación original.<br /><br /> NULL para todas las unidades de asignación salvo IN_ROW_DATA para un montón.<br /><br /> NULL para montones cuando *mode* = Limited.|  
|compressed_page_count|**bigint**|Número de páginas comprimidas.<br /><br /> En el caso de los montones, las nuevas páginas asignadas no usan la compresión de página. Un montón usa la compresión de página bajo dos condiciones especiales: cuando los datos se importan de forma masiva o cuando vuelve a generarse un montón. Las operaciones DML que producen las asignaciones de página no usarán la compresión de página. Vuelva a generar un montón cuando el valor compressed_page_count sea mayor que el umbral que desea.<br /><br /> En las tablas que tienen un índice clúster, el valor compressed_page_count indica la eficacia de la compresión de página.|  
|hobt_id|bigint|Solo para los índices de almacén de columnas, es el identificador de un conjunto de filas que realiza un seguimiento de los datos internos de almacén de columnas para una partición. Los conjuntos de filas se almacenan como montones de datos o árboles binarios. Tienen el mismo identificador de índice que el índice de almacén de columnas principal. Para obtener más información, vea [sys.internal_partitions &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-internal-partitions-transact-sql.md).<br /><br /> NULL si <br /><br /> **Se aplica a**: SQL Server 2016 y versiones posteriores, Azure SQL Database, Azure SQL instancia administrada|  
|column_store_delete_buffer_state|TINYINT| 0 = NOT_APPLICABLE<br /><br /> 1 = OPEN<br /><br /> 2 = PURGAR<br /><br /> 3 = VACIADO<br /><br /> 4 = RETIRADA<br /><br /> 5 = LISTO<br /><br />**Se aplica a**: SQL Server 2016 y versiones posteriores, Azure SQL Database, Azure SQL instancia administrada|  
|column_store_delete_buff_state_desc|| NO válido: el índice primario no es un índice de almacén de columnas.<br /><br /> Los analizadores y los eliminadores abiertos utilizan este.<br /><br /> PURGAr: los eliminadores se están purgando pero los analizadores aún lo usan.<br /><br /> VACIAdo: el búfer está cerrado y las filas del búfer se escriben en el mapa de bits de eliminación.<br /><br /> RETIRADA: las filas del búfer de eliminación cerrado se han escrito en el mapa de bits de eliminación, pero el búfer no se ha truncado porque los escáneres aún lo están usando. No es necesario que los nuevos escáneres usen el búfer de retirada porque el búfer abierto es suficiente.<br /><br /> LISTO: este búfer de eliminación está listo para su uso. <br /><br /> **Se aplica a**: SQL Server 2016 y versiones posteriores, Azure SQL Database, Azure SQL instancia administrada|  
|version_record_count|**bigint**|Es el recuento de los registros de versión de fila que se mantienen en este índice.  Estas versiones de fila se mantienen mediante la característica de [recuperación de base de datos acelerada](../../relational-databases/accelerated-database-recovery-concepts.md) . <br /><br /> [!INCLUDE[SQL2019](../../includes/applies-to-version/sqlserver2019.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] |  
|inrow_version_record_count|**bigint**|Recuento de registros de versión de ADR conservados en la fila de datos para una recuperación rápida. <br /><br />  [!INCLUDE[SQL2019](../../includes/applies-to-version/sqlserver2019.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e|  
|inrow_diff_version_record_count|**bigint**| Recuento de registros de versión de ADR conservados en forma de diferencias con respecto a la versión base. <br /><br /> [!INCLUDE[SQL2019](../../includes/applies-to-version/sqlserver2019.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|total_inrow_version_payload_size_in_bytes|**bigint**|Tamaño total en bytes de los registros de versión de controles para este índice. <br /><br /> [!INCLUDE[SQL2019](../../includes/applies-to-version/sqlserver2019.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|offrow_regular_version_record_count|**bigint**|Recuento de registros de versión que se mantienen fuera de la fila de datos original. <br /><br /> [!INCLUDE[SQL2019](../../includes/applies-to-version/sqlserver2019.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|offrow_long_term_version_record_count|**bigint**|Recuento de registros de versión considerados a largo plazo. <br /><br /> [!INCLUDE[SQL2019](../../includes/applies-to-version/sqlserver2019.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] |  

## <a name="remarks"></a>Notas  
 La función de administración dinámica sys.dm_db_index_physical_stats sustituye a la instrucción DBCC SHOWCONTIG.  
  
## <a name="scanning-modes"></a>Modos de recorrido  
 El modo en que se ejecuta la función determina el nivel de recorrido realizado para obtener los datos estadísticos que utiliza la función. el *modo* se especifica como Limited, sampled o Detailed. La función recorre las cadenas de páginas buscando las unidades de asignación que producen las particiones especificadas de la tabla o el índice. sys.dm_db_index_physical_stats solo requiere un bloqueo de tabla Intent-Shared (IS), independientemente del modo en que se ejecute.  
  
 El modo LIMITED es el modo más rápido y examina el menor número de páginas. Para un índice, solamente se examinan las páginas del nivel primario del árbol b (es decir, las páginas sobre el nivel hoja). Para un montón, se examinan las páginas PFS e IAM asociadas y las páginas de datos de un montón se exploran en modo LIMITED.  
  
 Con el modo LIMITED, compressed_page_count es NULL porque el [!INCLUDE[ssDE](../../includes/ssde-md.md)] solamente examina las páginas no hoja del árbol b y las páginas IAM y PFS del montón. Use el modo SAMPLEd para obtener un valor estimado para compressed_page_count y use el modo Detailed para obtener el valor real de compressed_page_count. El modo SAMPLED devuelve estadísticas basadas en una muestra de un uno por ciento de todas las páginas del índice o montón. Los resultados en el modo SAMPLED se deben considerar como aproximados. Si el índice o montón tiene menos de 10.000 páginas, se utiliza el modo DETAILED en lugar del modo SAMPLED.  
  
 El modo DETAILED recorre todas las páginas y devuelve todas las estadísticas.  
  
 Los modos son progresivamente más lentos de LIMITED a DETAILED, ya que se va realizando más trabajo en cada nodo. Para analizar rápidamente el nivel de fragmentación o tamaño de una tabla o índice, utilice el modo LIMITED. Es el modo más rápido y no obtendrá una fila por cada nivel no hoja en la unidad de asignación IN_ROW_DATA del índice.  
  
## <a name="using-system-functions-to-specify-parameter-values"></a>Usar funciones del sistema para especificar valores de parámetros  
 Puede usar las [!INCLUDE[tsql](../../includes/tsql-md.md)] funciones [DB_ID](../../t-sql/functions/db-id-transact-sql.md) y [OBJECT_ID](../../t-sql/functions/object-id-transact-sql.md) para especificar un valor para los parámetros *database_id* y *object_id* . Sin embargo, el envío de valores no válidos a estas funciones puede provocar resultados no deseados. Por ejemplo, si no se encuentra un nombre de objeto o base de datos, porque no existe o se ha escrito incorrectamente, ambas funciones devolverán NULL. La función sys.dm_db_index_physical_stats interpreta NULL como un valor comodín que especifica todas las bases de datos o todos los objetos.  
  
 Además, la función de OBJECT_ID se procesa antes de llamar a la función de sys.dm_db_index_physical_stats y, por tanto, se evalúa en el contexto de la base de datos actual, no en la base de datos especificada en *database_id*. Este comportamiento puede hacer que la función OBJECT_ID devuelva un valor NULL; o bien, si el nombre de objeto existe en el contexto de la base de datos actual y en el de la base de datos especificada, podría devolverse un mensaje de error. En los siguientes ejemplos se ilustran estos resultados no deseados.  
  
```  
USE master;  
GO  
-- In this example, OBJECT_ID is evaluated in the context of the master database.   
-- Because Person.Address does not exist in master, the function returns NULL.  
-- When NULL is specified as an object_id, all objects in the database are returned.  
-- The same results are returned when an object that is not valid is specified.  
SELECT * FROM sys.dm_db_index_physical_stats  
    (DB_ID(N'AdventureWorks'), OBJECT_ID(N'Person.Address'), NULL, NULL , 'DETAILED');  
GO  
-- This example demonstrates the results of specifying a valid object name  
-- that exists in both the current database context and  
-- in the database specified in the database_id parameter of the   
-- sys.dm_db_index_physical_stats function.  
-- An error is returned because the ID value returned by OBJECT_ID does not  
-- match the ID value of the object in the specified database.  
CREATE DATABASE Test;  
GO  
USE Test;  
GO  
CREATE SCHEMA Person;  
GO  
CREATE Table Person.Address(c1 int);  
GO  
USE AdventureWorks2012;  
GO  
SELECT * FROM sys.dm_db_index_physical_stats  
    (DB_ID(N'Test'), OBJECT_ID(N'Person.Address'), NULL, NULL , 'DETAILED');  
GO  
-- Clean up temporary database.  
DROP DATABASE Test;  
GO  
```  
  
### <a name="best-practice"></a>Procedimiento recomendado  
 Asegúrese de que se devuelva un identificador válido cuando utilice DB_ID u OBJECT_ID. Por ejemplo, al usar OBJECT_ID, especifique un nombre de tres partes como `OBJECT_ID(N'AdventureWorks2012.Person.Address')` , o bien Pruebe el valor devuelto por las funciones antes de usarlos en la función sys.dm_db_index_physical_stats. En los ejemplos A y B siguientes se ilustra una forma segura de especificar identificadores de objetos y bases de datos.  
  
## <a name="detecting-fragmentation"></a>Detectar la fragmentación  
 La fragmentación es consecuencia de los procesos de modificación de los datos (instrucciones INSERT, UPDATE y DELETE) efectuados en la tabla y en los índices definidos en la tabla. Como dichas modificaciones no suelen estar distribuidas de forma equilibrada entre las filas de la tabla y los índices, el llenado de cada página puede variar con el paso del tiempo. Para las consultas que examinan parcial o totalmente los índices de una tabla, este tipo de fragmentación puede producir lecturas de páginas adicionales. Esto impide el examen paralelo de los datos.  
  
 El nivel de fragmentación de un índice o montón aparece en la columna avg_fragmentation_in_percent. En el caso de montones, el valor representa la fragmentación de extensión del montón. En el caso de índices, el valor representa la fragmentación lógica del índice. A diferencia de DBCC SHOWCONTIG, los algoritmos de cálculo de fragmentación en ambos casos tienen en cuenta un almacenamiento que abarca varios archivos y, por lo tanto, son precisos.  
  
 **Fragmentación lógica**  
  
 Se trata del porcentaje de páginas sin orden en las páginas hoja de un índice. Una página no ordenada es aquella en la que la siguiente página física asignada al índice no es la que señala el puntero de *página siguiente* en la página hoja actual.  
  
 **Fragmentación de extensión**  
  
 Se trata del porcentaje de extensiones sin orden en las páginas hoja de un montón. Una extensión sin orden es aquella para la que la extensión que contiene la página actual de un montón no es físicamente la extensión siguiente a la que contiene la página anterior.  
  
 El valor de avg_fragmentation_in_percent debe ser lo más cercano posible a cero para obtener un rendimiento máximo. No obstante, los valores de 0% a 10% son aceptables. Puede utilizar todos los métodos de reducción de la fragmentación (por ejemplo, volver a generar, reorganizar o volver a crear) para disminuir estos valores. Para obtener más información sobre cómo analizar el grado de fragmentación en un índice, vea [reorganizar y volver a generar índices](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
## <a name="reducing-fragmentation-in-an-index"></a>Reducir la fragmentación en un índice  
 Cuando un índice está fragmentado de tal forma que el rendimiento de las consultas se ve afectado, hay tres opciones para reducir la fragmentación:  
  
-   Quite y vuelva a crear el índice clúster.  
  
     La reconstrucción de un índice clúster redistribuye los datos, lo que ocasiona que las páginas de datos se llenen. El grado de llenado puede configurarse con la opción FILLFACTOR de CREATE INDEX. Los inconvenientes de este método son que el índice está sin conexión durante el proceso de eliminación y creación, y que la operación es atómica. Si se interrumpe la creación del índice, no vuelve a crearse. Para obtener más información, vea [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
-   Utilice ALTER INDEX REORGANIZE, el sustituto de DBCC INDEXDEFRAG, para volver a ordenar las páginas de nivel hoja del índice en un orden lógico. Se trata de una operación en línea, por lo que el índice está disponible mientras se ejecuta la instrucción. La operación también puede interrumpirse sin perder el trabajo ya completado. Los inconvenientes de este método son que no es una forma tan buena de reorganizar los datos como la operación de volver a crear un índice, y que no actualiza las estadísticas.  
  
-   Utilice ALTER INDEX REBUILD, el sustituto de DBCC DBREINDEX, para volver a generar el índice en línea o sin conexión. Para obtener más información, vea [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
 La fragmentación por sí sola no es una razón suficiente para reorganizar o volver a generar un índice. El principal efecto de la fragmentación es una disminución del rendimiento de la lectura anticipada de páginas durante los recorridos de índice. Esto se traduce en tiempos de respuesta más lentos. Si la carga de consultas en un índice o tabla fragmentado no implica recorridos, debido a que la carga incluye principalmente búsquedas singleton, es posible que la eliminación de la fragmentación no tenga ninguna consecuencia.
  
> [!NOTE]  
>  Al ejecutar DBCC SHRINKFILE o DBCC SHRINKDATABASE, puede producirse una fragmentación si un índice se mueve parcial o totalmente durante la operación de reducción. Por esta razón, si tiene que realizar una operación de reducción, debe realizarla antes de quitar la fragmentación.  
  
## <a name="reducing-fragmentation-in-a-heap"></a>Reducir la fragmentación en un montón  
 Para reducir la fragmentación de extensión de un montón, cree un índice clúster en la tabla y, a continuación, quítelo. Con esta acción se redistribuyen los datos mientras se crea el índice clúster. Esto también optimiza la distribución del espacio disponible en la base de datos. Cuando el índice clúster se quita para volver a crear el montón, los datos no se mueven y permanecen de forma óptima en su posición. Para obtener información acerca de cómo realizar estas operaciones, vea [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md) y [DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md).  
  
> [!CAUTION]  
>  Al crear y quitar un índice clúster en una tabla, se recompilan dos veces todos los índices no clúster de esa tabla.  
  
## <a name="compacting-large-object-data"></a>Compactar datos de objetos grandes  
 De forma predeterminada, la instrucción ALTER INDEX REORGANIZE compacta las páginas que contienen datos de objetos grandes (LOB). Dado que las páginas LOB no cancelan su asignación cuando están vacías, el hecho de compactar estos datos puede mejorar el uso del espacio en disco si se eliminan muchos datos LOB o si se quita una columna LOB.  
  
 Si reorganiza un índice clúster específico, se compactan todas las columnas LOB incluidas en el índice clúster. Si reorganiza un índice no clúster, se compactan todas las columnas LOB sin clave incluidas en el índice. Cuando se especifica ALL en la instrucción, todos los índices asociados a la tabla o vista especificada se reorganizan. Asimismo, se compactan todas las columnas LOB asociadas al índice clúster, la tabla subyacente o el índice no clúster que contiene columnas.  
  
## <a name="evaluating-disk-space-use"></a>Evaluar el uso del espacio en disco  
 La columna avg_page_space_used_in_percent indica el llenado de la página. Para lograr un uso óptimo del espacio en disco, este valor debe estar próximo al 100% para un índice que no tenga muchas inserciones aleatorias. No obstante, un índice que tenga muchas inserciones aleatorias y páginas muy llenas tendrá más divisiones de páginas. Esto causa más fragmentación. Por lo tanto, para reducir las divisiones de páginas, el valor debe ser inferior a 100%. Si vuelve a generar un índice con la opción FILLFACTOR especificada, podrá cambiar el llenado de la página para ajustarse al patrón de consulta en el índice. Para obtener más información acerca del factor de relleno, vea [especificar el factor de relleno para un índice](../../relational-databases/indexes/specify-fill-factor-for-an-index.md). Asimismo, ALTER INDEX REORGANIZE compactará un índice intentando rellenar las páginas según el último valor de FILLFACTOR especificado. Esto aumenta el valor de avg_space_used_in_percent. Tenga en cuenta que ALTER INDEX REORGANIZE no puede reducir el llenado de página. Para ello, deberá volver a generar el índice.  
  
## <a name="evaluating-index-fragments"></a>Evaluar fragmentos de índice  
 Un fragmento se compone de páginas hoja consecutivas físicamente en el mismo archivo para una unidad de asignación. Un índice tiene al menos un fragmento. El número máximo de fragmentos que puede tener un índice es igual al número de páginas en el nivel hoja de un índice. El uso de fragmentos mayores indica que se necesita menos E/S de disco para leer el mismo número de páginas. Por lo tanto, cuanto mayor sea el valor de avg_fragment_size_in_pages, mejor será el rendimiento del recorrido de intervalos. Los valores avg_fragment_size_in_pages y avg_fragmentation_in_percent son inversamente proporcionales entre sí. Por lo tanto, si vuelve a generar o reorganiza un índice, debe reducir la cantidad de fragmentación y aumentar el tamaño de los fragmentos.  
  
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
 No devuelve datos para los índices de almacén de columnas en clúster.  
  
## <a name="permissions"></a>Permisos  
 Necesita los siguientes permisos:  
  
-   El permiso CONTROL en el objeto especificado en la base de datos.  
  
-   Permiso VIEW DATABASE STATE para devolver información sobre todos los objetos de la base de datos especificada, mediante el uso del carácter comodín de objeto @*object_id*= null.  
  
-   Permiso VIEW SERVER STATE para devolver información sobre todas las bases de datos mediante el carácter comodín de base de datos @*database_id* = null.  
  
 El permiso VIEW DATABASE STATE permite devolver todos los objetos de la base de datos, independientemente de los permisos CONTROL denegados en objetos específicos.  
  
 Si se deniega el permiso VIEW DATABASE STATE, no se puede devolver ningún objeto de la base de datos, independientemente de que se hayan concedido permisos CONTROL a objetos específicos. Además, cuando se especifica el carácter comodín de base de datos @*database_id*= null, se omite la base de datos.  
  
 Para obtener más información, vea [funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-returning-information-about-a-specified-table"></a>A. Devolver información acerca de una tabla especificada  
 El ejemplo siguiente devuelve estadísticas de fragmentación y tamaño de todos los índices y particiones de la tabla `Person.Address`. El modo de recorrido se establece en `'LIMITED'` para obtener el mayor rendimiento posible y limitar las estadísticas devueltas. Para ejecutar esta consulta, es necesario, como mínimo, el permiso CONTROL en la tabla `Person.Address`.  
  
```  
DECLARE @db_id SMALLINT;  
DECLARE @object_id INT;  
  
SET @db_id = DB_ID(N'AdventureWorks2012');  
SET @object_id = OBJECT_ID(N'AdventureWorks2012.Person.Address');  
  
IF @db_id IS NULL  
BEGIN;  
    PRINT N'Invalid database';  
END;  
ELSE IF @object_id IS NULL  
BEGIN;  
    PRINT N'Invalid object';  
END;  
ELSE  
BEGIN;  
    SELECT * FROM sys.dm_db_index_physical_stats(@db_id, @object_id, NULL, NULL , 'LIMITED');  
END;  
GO  
  
```  
  
### <a name="b-returning-information-about-a-heap"></a>B. Devolver información acerca de un montón  
 El ejemplo siguiente devuelve todas las estadísticas para el montón `dbo.DatabaseLog` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Dado que la tabla contiene datos LOB, se devuelve una fila para la unidad de asignación `LOB_DATA` y una fila para el valor `IN_ROW_ALLOCATION_UNIT` que está almacenando las páginas de datos del montón. Para ejecutar esta consulta, es necesario, como mínimo, el permiso CONTROL en la tabla `dbo.DatabaseLog`.  
  
```  
DECLARE @db_id SMALLINT;  
DECLARE @object_id INT;  
SET @db_id = DB_ID(N'AdventureWorks2012');  
SET @object_id = OBJECT_ID(N'AdventureWorks2012.dbo.DatabaseLog');  
IF @object_id IS NULL   
BEGIN;  
    PRINT N'Invalid object';  
END;  
ELSE  
BEGIN;  
    SELECT * FROM sys.dm_db_index_physical_stats(@db_id, @object_id, 0, NULL , 'DETAILED');  
END;  
GO  
  
```  
  
### <a name="c-returning-information-for-all-databases"></a>C. Devolver información de todas las bases de datos  
 En el ejemplo siguiente se devuelven todas las estadísticas de todas las tablas e índices de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]; para ello, se especifica el comodín `NULL` en todos los parámetros. Para ejecutar esta consulta necesita el permiso VIEW SERVER STATE.  
  
```  
SELECT * FROM sys.dm_db_index_physical_stats (NULL, NULL, NULL, NULL, NULL);  
GO  
  
```  
  
### <a name="d-using-sysdm_db_index_physical_stats-in-a-script-to-rebuild-or-reorganize-indexes"></a>D. Usar sys.dm_db_index_physical_stats en un script para volver a generar o reorganizar índices  
 El ejemplo siguiente reorganiza o vuelve a generar automáticamente todas las particiones de una base de datos que tienen un promedio de fragmentación superior al 10%. Para ejecutar esta consulta necesita el permiso VIEW DATABASE STATE. Este ejemplo especifica `DB_ID` como primer parámetro, sin especificar un nombre de base de datos. Se generará un error si la base de datos actual tiene un nivel de compatibilidad de 80 o inferior. Para solucionar el error, reemplace `DB_ID()` por un nombre de base de datos válido. Para obtener más información sobre los niveles de compatibilidad de bases de datos, vea [nivel de compatibilidad de Alter database &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
```  
-- Ensure a USE <databasename> statement has been executed first.  
SET NOCOUNT ON;  
DECLARE @objectid int;  
DECLARE @indexid int;  
DECLARE @partitioncount bigint;  
DECLARE @schemaname nvarchar(130);   
DECLARE @objectname nvarchar(130);   
DECLARE @indexname nvarchar(130);   
DECLARE @partitionnum bigint;  
DECLARE @partitions bigint;  
DECLARE @frag float;  
DECLARE @command nvarchar(4000);   
-- Conditionally select tables and indexes from the sys.dm_db_index_physical_stats function   
-- and convert object and index IDs to names.  
SELECT  
    object_id AS objectid,  
    index_id AS indexid,  
    partition_number AS partitionnum,  
    avg_fragmentation_in_percent AS frag  
INTO #work_to_do  
FROM sys.dm_db_index_physical_stats (DB_ID(), NULL, NULL , NULL, 'LIMITED')  
WHERE avg_fragmentation_in_percent > 10.0 AND index_id > 0;  
  
-- Declare the cursor for the list of partitions to be processed.  
DECLARE partitions CURSOR FOR SELECT * FROM #work_to_do;  
  
-- Open the cursor.  
OPEN partitions;  
  
-- Loop through the partitions.  
WHILE (1=1)  
    BEGIN;  
        FETCH NEXT  
           FROM partitions  
           INTO @objectid, @indexid, @partitionnum, @frag;  
        IF @@FETCH_STATUS < 0 BREAK;  
        SELECT @objectname = QUOTENAME(o.name), @schemaname = QUOTENAME(s.name)  
        FROM sys.objects AS o  
        JOIN sys.schemas as s ON s.schema_id = o.schema_id  
        WHERE o.object_id = @objectid;  
        SELECT @indexname = QUOTENAME(name)  
        FROM sys.indexes  
        WHERE  object_id = @objectid AND index_id = @indexid;  
        SELECT @partitioncount = count (*)  
        FROM sys.partitions  
        WHERE object_id = @objectid AND index_id = @indexid;  
  
-- 30 is an arbitrary decision point at which to switch between reorganizing and rebuilding.  
        IF @frag < 30.0  
            SET @command = N'ALTER INDEX ' + @indexname + N' ON ' + @schemaname + N'.' + @objectname + N' REORGANIZE';  
        IF @frag >= 30.0  
            SET @command = N'ALTER INDEX ' + @indexname + N' ON ' + @schemaname + N'.' + @objectname + N' REBUILD';  
        IF @partitioncount > 1  
            SET @command = @command + N' PARTITION=' + CAST(@partitionnum AS nvarchar(10));  
        EXEC (@command);  
        PRINT N'Executed: ' + @command;  
    END;  
  
-- Close and deallocate the cursor.  
CLOSE partitions;  
DEALLOCATE partitions;  
  
-- Drop the temporary table.  
DROP TABLE #work_to_do;  
GO  
  
```  
  
### <a name="e-using-sysdm_db_index_physical_stats-to-show-the-number-of-page-compressed-pages"></a>E. Utilizar sys.dm_db_index_physical_stats para mostrar el número de páginas que usan la compresión de página  
 En el ejemplo siguiente se muestra cómo mostrar y comparar el número total de páginas con las páginas sometidas a compresión de filas y páginas. Esta información se puede utilizar para determinar el beneficio que aporta la compresión a un índice o una tabla.  
  
```  
SELECT o.name,  
    ips.partition_number,  
    ips.index_type_desc,  
    ips.record_count, ips.avg_record_size_in_bytes,  
    ips.min_record_size_in_bytes,  
    ips.max_record_size_in_bytes,  
    ips.page_count, ips.compressed_page_count  
FROM sys.dm_db_index_physical_stats ( DB_ID(), NULL, NULL, NULL, 'DETAILED') ips  
JOIN sys.objects o on o.object_id = ips.object_id  
ORDER BY record_count DESC;  
```  
  
### <a name="f-using-sysdm_db_index_physical_stats-in-sampled-mode"></a>F. Usar sys.dm_db_index_physical_stats en el modo SAMPLED  
 El ejemplo siguiente muestra cómo el modo SAMPLED devuelve un valor aproximado diferente a los resultados del modo DETAILED.  
  
```  
CREATE TABLE t3 (col1 int PRIMARY KEY, col2 varchar(500)) WITH(DATA_COMPRESSION = PAGE);  
GO  
BEGIN TRAN  
DECLARE @idx int = 0;  
WHILE @idx < 1000000  
BEGIN  
    INSERT INTO t3 (col1, col2)   
    VALUES (@idx,   
    REPLICATE ('a', 100) + CAST (@idx as varchar(10)) + REPLICATE ('a', 380))  
    SET @idx = @idx + 1  
END  
COMMIT;  
GO  
SELECT page_count, compressed_page_count, forwarded_record_count, *   
FROM sys.dm_db_index_physical_stats (db_id(),   
    object_id ('t3'), null, null, 'SAMPLED');  
SELECT page_count, compressed_page_count, forwarded_record_count, *   
FROM sys.dm_db_index_physical_stats (db_id(),   
    object_id ('t3'), null, null, 'DETAILED');  
```  
  
### <a name="g-querying-service-broker-queues-for-index-fragmentation"></a>G. Consultando las colas de Service Broker para la fragmentación de índices  
  
||  
|-|  
|**Se aplica a**: desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
 En los siguientes ejemplos se muestra cómo consultar las colas del agente de servidor para la fragmentación.  
  
```  
--Using queue internal table name   
select * from sys.dm_db_index_physical_stats (db_id(), object_id ('sys.queue_messages_549576996'), default, default, default)   
  
--Using queue name directly  
select * from sys.dm_db_index_physical_stats (db_id(), object_id ('ExpenseQueue'), default, default, default)  
  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funciones y vistas de administración dinámica relacionadas con índices &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_db_index_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)   
 [sys.dm_db_index_usage_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md)   
 [sys.dm_db_partition_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md)   
 [sys.allocation_units &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)   
 [Vistas del sistema &#40;Transact-SQL&#41;](../../t-sql/language-reference.md)  
  
