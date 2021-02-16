---
description: DATABASEPROPERTYEX (Transact-SQL)
title: DATABASEPROPERTYEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/23/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATABASEPROPERTYEX
- DATABASEPROPERTYEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DATABASEPROPERTYEX function
- displaying database properties
- database properties [SQL Server]
ms.assetid: 8a9e0ffb-28b5-4640-95b2-a54e3e5ad941
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ec28104c0d178c50bc468a9e9012e71c89cc21db
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100346612"
---
# <a name="databasepropertyex-transact-sql"></a>DATABASEPROPERTYEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Para una base de datos especificada en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], esta función devuelve la configuración actual de la opción o propiedad de base de datos especificada.
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DATABASEPROPERTYEX ( database , property )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*database*  
Una expresión que especifica el nombre de la base de datos para la que `DATABASEPROPERTYEX` devolverá la información de la propiedad con nombre. *database* tiene un tipo de datos **nvarchar(128)** .  

Para [!INCLUDE[ssSDS](../../includes/sssds-md.md)], `DATABASEPROPERTYEX` necesita el nombre de la base de datos actual. Devuelve NULL para todas las propiedades si se proporciona un nombre de base de datos diferente.
  
*property*  
Una expresión que especifica el nombre de la propiedad de base de datos que se va a devolver. *property* tiene un tipo de datos **varchar(128)** y admite uno de los valores de esta tabla:
  
> [!NOTE]  
>  Si la base de datos aún no se ha iniciado, las llamadas a `DATABASEPROPERTYEX` devolverán NULL si `DATABASEPROPERTYEX` recupera esos valores mediante el acceso directo de la base de datos, en lugar de hacerlo mediante la recuperación a partir de los metadatos. Si hay una base de datos con AUTO_CLOSE establecido en ON (o, de lo contrario, sin conexión), se definirá como "no iniciada".  
  
|Propiedad|Descripción|Valor devuelto|  
|---|---|---|
|Intercalación|Nombre de intercalación predeterminado para la base de datos.|Nombre de intercalación<br /><br /> NULL: La base de datos no se ha iniciado.<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|ComparisonStyle|El estilo de comparación de Windows de la intercalación. Use los siguientes valores de estilo para generar un mapa de bits para el valor ComparisonStyle terminado:<br /><br /> Omitir mayúsculas y minúsculas: 1<br /><br /> Omitir acento: 2<br /><br /> Omitir Kana: 65536<br /><br /> Omitir ancho: 131 072<br /><br /> <br /><br /> Por ejemplo, el valor predeterminado 196609 es el resultado de combinar las opciones de omitir mayúsculas y minúsculas, omitir Kana y omitir ancho.|Devuelve el estilo de comparación.<br /><br /> Devuelve 0 para todas las intercalaciones binarias.<br /><br /> Tipo de datos base: **int**|  
|Edición|El nivel de servicio o edición de la base de datos.|**Se aplica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].<br /><br /> <br /><br /> De uso general<br /><br /> Crítico para la empresa<br /><br /> Básico<br /><br /> Estándar<br /><br /> Premium<br /><br /> System (de la base de datos maestra)<br /><br /> NULL: La base de datos no se ha iniciado.<br /><br /> Tipo de datos base: **nvarchar**(64)|  
|IsAnsiNullDefault|La base de datos sigue las reglas ISO para permitir los valores NULL.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**|  
|IsAnsiNullsEnabled|Todas las comparaciones con un valor NULL tienen un resultado desconocido.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**|  
|IsAnsiPaddingEnabled|Las cadenas se rellenan a la misma longitud antes de comparar o insertar.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**|  
|IsAnsiWarningsEnabled|SQL Server emite mensajes de error o de advertencia cuando se producen condiciones de error estándar.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**|  
|IsArithmeticAbortEnabled|Las consultas se finalizan cuando hay un error de desbordamiento o división por cero durante su ejecución.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**|  
|IsAutoClose|La base de datos se cierra sin problemas y libera los recursos cuando sale el último usuario.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**|  
|IsAutoCreateStatistics|El optimizador de consultas crea estadísticas de columna única, según sea necesario, para mejorar el rendimiento de las consultas.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**|  
|IsAutoCreateStatisticsIncremental|Las estadísticas de columna única creadas automáticamente son incrementales siempre que sea posible.|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> 1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**|  
|IsAutoShrink|Los archivos de base de datos son candidatos para la reducción periódica automática.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**|  
|IsAutoUpdateStatistics|Cuando una consulta usa estadísticas existentes potencialmente obsoletas, el optimizador de consultas actualiza dichas estadísticas.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**|
|IsClone|La base de datos es una copia de solo estadísticas y esquema de una base de datos de usuario creada con DBCC CLONEDATABASE. Vea este [artículo de Soporte técnico de Microsoft](https://support.microsoft.com/help/3177838) para más información.|**Se aplica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 y versiones posteriores<br /><br /> 1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**| 
|IsCloseCursorsOnCommitEnabled|Cuando se confirme una transacción, se cerrarán todos los cursores abiertos.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**|  
|IsFulltextEnabled|La base de datos está habilitada para la indización semántica y de texto completo.|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> <br /><br /> 1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**<br /><br /> **Nota:** El valor de esta propiedad ya no tiene ningún efecto. En las bases de datos de usuario siempre está habilitada la búsqueda de texto completo. Una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] quitará esta propiedad. No use esta propiedad en nuevos trabajos de desarrollo, y modifique lo antes posible las aplicaciones que la usen actualmente.|  
|IsInStandBy|La base de datos está en línea como de solo lectura con el registro de restauración permitido.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**|  
|IsLocalCursorsDefault|El valor predeterminado de las declaraciones de cursores es LOCAL.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**|  
|IsMemoryOptimizedElevateToSnapshotEnabled|Se obtiene acceso a las tablas optimizadas para memoria mediante el aislamiento de SNAPSHOT cuando el valor de configuración de sesión TRANSACTION ISOLATION LEVEL se establece en READ COMMITTED, READ UNCOMMITTED o en un nivel de aislamiento inferior.|**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.<br /><br /> <br /><br /> 1: TRUE<br /><br /> 0: FALSE<br /><br /> Tipo de datos base: **int**|  
|IsMergePublished|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite la publicación de tablas de base de datos para la replicación de mezcla, en el caso de que la replicación esté instalada.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**|  
|IsNullConcat|La concatenación con un operando NULL da como resultado NULL.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**|  
|IsNumericRoundAbortEnabled|Se generan errores cuando se produce una pérdida de precisión en expresiones.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**|  
|IsParameterizationForced|La opción de base de datos PARAMETERIZATION es FORCED por medio del comando SET.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida|  
|IsQuotedIdentifiersEnabled|Se pueden usar comillas dobles en los identificadores.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**|  
|IsPublished|Si la replicación está instalada, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite la publicación de tablas de base de datos para la replicación transaccional o de instantáneas.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**|  
|IsRecursiveTriggersEnabled|Se habilita la activación recursiva de desencadenadores.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**|  
|IsSubscribed|La base de datos está suscrita a una publicación.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**|  
|IsSyncWithBackup|La base de datos es una base de datos publicada o una base de datos de distribución y admite una restauración que no interrumpirá la replicación transaccional.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**|  
|IsTornPageDetectionEnabled|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] detecta operaciones de E/S incompletas debido a problemas con el suministro eléctrico u otros errores del sistema.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**| 
|IsVerifiedClone|La base de datos es una copia de solo estadísticas y esquema de una base de datos de usuario, creada con la opción WITH VERIFY_CLONEDB de DBCC CLONEDATABASE. Vea este [artículo de Soporte técnico de Microsoft](https://support.microsoft.com/help/3177838) para más información.|**Se aplica a**: A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2.<br /><br /> <br /><br /> 1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **int**| 
|IsXTPSupported|Indica si la base de datos es compatible con OLTP en memoria; es decir, si permite crear y usar tablas optimizadas para memoria y módulos compilados de forma nativa.<br /><br /> Específica de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:<br /><br /> IsXTPSupported es independiente de la existencia de algún grupo de archivos MEMORY_OPTIMIZED_DATA, necesario para crear objetos de OLTP en memoria.|**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]<br /><br /> 1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: Entrada no válida, error o no aplicable<br /><br /> Tipo de datos base: **int**|  
|LastGoodCheckDbTime|La fecha y hora de la última operación DBCC CHECKDB que se ejecutó correctamente en la base de datos especificada. <sup>1</sup> Si DBCC CHECKDB no se ha ejecutado en una base de datos, se devuelve 01-01-1900 00:00:00.000.|**Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] a partir de SP2.</br>[!INCLUDE[sssql17](../../includes/sssql17-md.md)] a partir de CU9.</br>[!INCLUDE[sssql19](../../includes/sssql19-md.md)] o una versión posterior</br>Azure SQL Database.<br/><br/>Un valor de fecha y hora<br /><br /> NULL: Entrada no válida<br /><br /> Tipo de datos base: **datetime**| 
|LCID|El identificador de configuración regional (LCID) de Windows para la intercalación.|Valor de LCID (en formato decimal).<br /><br /> Tipo de datos base: **int**|  
|MaxSizeInBytes|Tamaño máximo de la base de datos (en bytes).|**Se aplica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].<br /><br />[Azure SQL Database y Azure Synapse Analytics](/azure/sql-database/sql-database-single-database-scale#dtu-based-purchasing-model): el valor se basa en SLO, a menos que se haya adquirido almacenamiento adicional.<br /><br />[núcleo virtual](/azure/sql-database/sql-database-single-database-scale#vcore-based-purchasing-model): el valor está en incrementos de 1 GB hasta el tamaño máximo.<br /><br />NULL: La base de datos no se ha iniciado.<br /><br /> Tipo de base de datos: **bigint**|  
|Recuperación|Modelo de recuperación de base de datos|FULL: Modelo de recuperación completa<br /><br /> BULK_LOGGED: Modelo optimizado para cargas masivas de registros<br /><br /> SIMPLE: Modelo de recuperación simple<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|ServiceObjective|Describe el nivel de rendimiento de la base de datos en [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] o [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].|Uno de los siguientes:<br /><br /> NULL = La base de datos no se ha iniciado<br /><br /> Compartido (para las ediciones Web o Business)<br /><br /> Básico<br /><br /> S0<br /><br /> S1<br /><br /> S2<br /><br /> S3<br /><br /> P1<br /><br /> P2<br /><br /> P3<br /><br /> ElasticPool<br /><br /> Sistema (para la base de datos maestra)<br /><br /> Tipo de datos base: **nvarchar(32)**|  
|ServiceObjectiveId|El identificador del objetivo del servicio en [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].|**uniqueidentifier** que identifica el objetivo del servicio.|  
|SQLSortOrder|Id. de orden de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compatible con versiones anteriores de SQL Server.|0: La base de datos usa intercalación de Windows<br /><br /> >0: Identificador de criterio de ordenación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]<br /><br /> NULL: Entrada no válida o la base de datos no se ha iniciado<br /><br /> Tipo de datos base: **tinyint**|  
|Estado|Estado de la base de datos.|ONLINE: La base de datos está disponible para consultas.<br /><br /> **Nota:** La función puede devolver el estado ONLINE mientras se abre la base de datos y aún no se haya recuperado. Para saber si una base de datos ONLINE puede aceptar conexiones, consulte la propiedad Collation de **DATABASEPROPERTYEX**. La base de datos ONLINE puede aceptar conexiones cuando la intercalación de base de datos devuelve un valor distinto de NULL. En el caso de las bases de datos Always On, consulte las columnas database_state o database_state_desc de `sys.dm_hadr_database_replica_states`.<br /><br /> OFFLINE: La base de datos está explícitamente sin conexión.<br /><br /> RESTORING: Se ha iniciado la restauración de la base de datos.<br /><br /> RECOVERING: Se ha iniciado la recuperación de la base de datos, que aún no está lista para consultas.<br /><br /> SUSPECT: La base de datos no se ha recuperado.<br /><br /> EMERGENCY: La base de datos está en un estado de emergencia de solo lectura. El acceso se restringe a los miembros del rol sysadmin<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|Updateability|Indica si los datos se pueden modificar.|READ_ONLY: La base de datos admite lecturas de datos, pero no modificaciones de datos.<br /><br /> READ_WRITE: La base de datos admite lecturas y modificaciones de datos.<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|UserAccess|Indica qué usuarios pueden tener acceso a la base de datos.|SINGLE_USER: Solo un usuario db_owner, dbcreator o sysadmin a la vez<br /><br /> RESTRICTED_USER: Solo miembros de los roles db_owner, dbcreator o sysadmin<br /><br /> MULTI_USER: Todos los usuarios<br /><br /> Tipo de datos base: **nvarchar(128)**|  
|Versión|Número interno de versión del código de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con el que se creó la base de datos. [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|Número de versión: La base de datos está abierta.<br /><br /> NULL: La base de datos no se ha iniciado.<br /><br /> Tipo de datos base: **int**| 

<br/>   

> [!NOTE]  
> <sup>1</sup> Para las bases de datos que forman parte de un grupo de disponibilidad, `LastGoodCheckDbTime` devolverá la fecha y hora de la última operación DBCC CHECKDB que se ejecutó correctamente en la réplica principal, con independencia de la réplica desde la que se ejecute el comando. 

## <a name="return-types"></a>Tipos de valores devueltos
**sql_variant**
  
## <a name="exceptions"></a>Excepciones  
Devuelve NULL si se produce un error o si el autor de la llamada no tiene permiso para ver el objeto.
  
En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un usuario solo puede ver los metadatos de elementos protegibles que posea o para los que se le haya concedido permiso. Esto significa que las funciones integradas de emisión de metadatos, como `OBJECT_ID`, podrían devolver NULL si el usuario no tiene permisos en el objeto. Vea [Configuración de visibilidad de los metadatos](../../relational-databases/security/metadata-visibility-configuration.md) para obtener más información.
  
## <a name="remarks"></a>Observaciones  
`DATABASEPROPERTYEX` devuelve un único valor de propiedad cada vez. Para ver varios valores de propiedad, use la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-retrieving-the-status-of-the-auto_shrink-database-option"></a>A. Recuperar el estado de la opción de base de datos AUTO_SHRINK  
Este ejemplo devuelve el estado de la opción de base de datos AUTO_SHRINK para la base de datos `AdventureWorks`.
  
```sql
SELECT DATABASEPROPERTYEX('AdventureWorks2014', 'IsAutoShrink');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)] Esto indica que AUTO_SHRINK está desactivado.
  
```
------------------  
0  
```  
  
### <a name="b-retrieving-the-default-collation-for-a-database"></a>B. Recuperar la intercalación predeterminada de una base de datos  
Este ejemplo devuelve varios atributos de la base de datos `AdventureWorks`.
  
```sql
SELECT   
    DATABASEPROPERTYEX('AdventureWorks2014', 'Collation') AS Collation,  
    DATABASEPROPERTYEX('AdventureWorks2014', 'Edition') AS Edition,  
    DATABASEPROPERTYEX('AdventureWorks2014', 'ServiceObjective') AS ServiceObjective,  
    DATABASEPROPERTYEX('AdventureWorks2014', 'MaxSizeInBytes') AS MaxSizeInBytes  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Collation                     Edition        ServiceObjective  MaxSizeInBytes  
----------------------------  -------------  ----------------  --------------  
SQL_Latin1_General_CP1_CI_AS  DataWarehouse  DW1000            5368709120  
```  
  
## <a name="see-also"></a>Consulte también
[ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)  
[Estados de base de datos](../../relational-databases/databases/database-states.md)  
[sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
[sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)  
[SERVERPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/serverproperty-transact-sql.md)
  
  
