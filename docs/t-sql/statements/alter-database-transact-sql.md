---
description: ALTER DATABASE (Transact-SQL)
title: ALTER DATABASE (Transact-SQL)| Microsoft Docs
ms.custom: references_regions
ms.date: 10/30/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_DATABASE_TSQL
- ALTER DATABASE
dev_langs:
- TSQL
helpviewer_keywords:
- databases [SQL Server], modifying
- ALTER DATABASE statement
- databases [SQL Server], renaming
- renaming databases
- database modifications [SQL Server]
- ALTER DATABASE statement, syntax described
- database names [SQL Server], ALTER DATABASE
- modifying databases
- collations [SQL Server], modifying
- database mirroring [SQL Server], Transact-SQL
ms.assetid: 15f8affd-8f39-4021-b092-0379fc6983da
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-current||=azuresqldb-mi-current||=azure-sqldw-latest||>=aps-pdw-2016'
ms.openlocfilehash: a5b5a4174a8faae5c57ed6844e96f52b8f271311
ms.sourcegitcommit: 2bdf1f1ee88f4fe3e872227d025e965e95d1b2b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2021
ms.locfileid: "98712001"
---
# <a name="alter-database-transact-sql"></a>ALTER DATABASE (Transact-SQL)

Modifica ciertas opciones de configuración de una base de datos.

En este artículo se proporciona la sintaxis, argumentos, comentarios, permisos y ejemplos para cualquier producto SQL que elija.

Para obtener más información sobre las convenciones de sintaxis, vea [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL Database](alter-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-sql-server"></a>Introducción: SQL Server

En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], esta instrucción modifica una base de datos, o los archivos y grupos de archivos asociados a la base de datos. Agrega o quita archivos y grupos de archivos en una base de datos, cambia los atributos de una base de datos o de sus archivos y grupos de archivos, cambia la intercalación de base de datos y establece las opciones de base de datos. Las instantáneas de base de datos no se pueden modificar. Para modificar las opciones de base de datos asociadas a la replicación, utilice [sp_replicationdboption](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md).

Debido a su longitud, la sintaxis `ALTER DATABASE` se divide en varios artículos.

ALTER DATABASE   
En el artículo actual se proporciona la sintaxis e información relacionada para cambiar el nombre y la intercalación de una base de datos.

[Opciones File y Filegroup de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)   
Proporciona la sintaxis e información relacionada para agregar y eliminar archivos y grupos de archivos de una base de datos y para cambiar los atributos de archivos y grupos de archivos.

[Opciones SET de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
Proporciona la sintaxis e información relacionada para cambiar los atributos de una base de datos usando las opciones SET de ALTER DATABASE.

[Creación de reflejo de la base de datos de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)   
Proporciona la sintaxis e información relacionada de las opciones SET de ALTER DATABASE relacionadas con la creación de reflejo de la base de datos.

[ALTER DATABASE SET HADR](../../t-sql/statements/alter-database-transact-sql-set-hadr.md)   
Proporciona la sintaxis e información relacionada de las opciones [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] de ALTER DATABASE para configurar una base de datos secundaria en una réplica secundaria de un grupo de disponibilidad AlwaysOn.

[Nivel de compatibilidad de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)   
Proporciona la sintaxis e información relacionada de las opciones SET de ALTER DATABASE relacionadas con los niveles de compatibilidad de la base de datos.

[ALTER DATABASE SCOPED CONFIGURATION](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)   
Proporciona la sintaxis relacionada con las configuraciones con ámbito de base de datos utilizadas para la configuración del nivel de base de datos individual, como los comportamientos relacionados con la optimización y la ejecución de consultas.

## <a name="syntax"></a>Sintaxis

```syntaxsql
-- SQL Server Syntax
ALTER DATABASE { database_name | CURRENT }
{
    MODIFY NAME = new_database_name
  | COLLATE collation_name
  | <file_and_filegroup_options>
  | SET <option_spec> [ ,...n ] [ WITH <termination> ]
}
[;]

<file_and_filegroup_options>::=
  <add_or_modify_files>::=
  <filespec>::=
  <add_or_modify_filegroups>::=
  <filegroup_updatability_option>::=

<option_spec>::=
{
  | <auto_option>
  | <change_tracking_option>
  | <cursor_option>
  | <database_mirroring_option>
  | <date_correlation_optimization_option>
  | <db_encryption_option>
  | <db_state_option>
  | <db_update_option>
  | <db_user_access_option><delayed_durability_option>
  | <external_access_option>
  | <FILESTREAM_options>
  | <HADR_options>
  | <parameterization_option>
  | <query_store_options>
  | <recovery_option>
  | <service_broker_option>
  | <snapshot_option>
  | <sql_option>
  | <termination>
  | <temporal_history_retention>
  | <data_retention_policy>
  | <compatibility_level>
      { 150 | 140 | 130 | 120 | 110 | 100 | 90 }
}
```

## <a name="arguments"></a>Argumentos

*database_name* Es el nombre de la base de datos que se va a modificar.

> [!NOTE]
> Esta opción no está disponible en las bases de datos independientes.

CURRENT   
**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.

Designa que la base de datos actual en uso se debe modificar.

MODIFY NAME **=** _new_database_name_   
Reemplaza el nombre de la base de datos por el nombre especificado como *new_database_name*.

COLLATE *collation_name*   
Especifica la intercalación de la base de datos. *collation_name* puede ser un nombre de intercalación de Windows o un nombre de intercalación de SQL. Si no se especifica, se asigna a la base de datos la intercalación de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

> [!NOTE]
> No se puede cambiar la intercalación después de crear la base de datos en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Al crear bases de datos con una intercalación diferente de la predeterminada, los datos de la base de datos siempre respetan la intercalación especificada. Para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], al crear una base de datos independiente, la información de catálogo interno se mantiene mediante la intercalación predeterminada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], **Latin1_General_100_CI_AS_WS_KS_SC**.

Para más información sobre los nombres de intercalación de Windows y de SQL, consulte [COLLATE](~/t-sql/statements/collations.md).

**\<delayed_durability_option> ::=**    
**Válido para** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.

Para obtener más información, vea [Opciones de ALTER DATABASE SET](../../t-sql/statements/alter-database-transact-sql-set-options.md) y [Controlar la durabilidad de las transacciones](../../relational-databases/logs/control-transaction-durability.md).

**\<file_and_filegroup_options>::=**    
Para más información, consulte [Opciones File y Filegroup de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).

## <a name="remarks"></a>Observaciones

Para quitar una base de datos, use [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md).

Para reducir el tamaño de una base de datos, use [DBCC SHRINKDATABASE](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md).

La instrucción `ALTER DATABASE` se debe ejecutar en modo de confirmación automática (modo predeterminado de administración de transacciones) y no se permite en una transacción explícita o implícita.

El estado de un archivo de base de datos (por ejemplo, en línea o sin conexión) se mantiene con independencia del estado de la base de datos. Para más información, vea [Estados de los archivos](../../relational-databases/databases/file-states.md). El estado de los archivos de un grupo de archivos determina la disponibilidad de todo el grupo de archivos. Para que un grupo de archivos esté disponible, todos los archivos del grupo de archivos deben estar en línea. Si un grupo de archivos se encuentra en modo sin conexión, todos los intentos de acceso al grupo de archivos por parte de una instrucción SQL generan un error. Al generar un plan de consulta para las instrucciones SELECT, el optimizador de consultas evita los índices no clúster y las vistas indizadas que residen en los grupos de archivos sin conexión. Esto permite que las instrucciones se ejecuten correctamente. No obstante, si el grupo de archivos sin conexión contiene el montón o el índice clúster de la tabla de destino, las instrucciones SELECT no funcionarán. Asimismo, cualquier instrucción `INSERT`, `UPDATE` o `DELETE` que modifique una tabla con cualquier índice en un grupo de archivos sin conexión no funcionará.

Si una base de datos se encuentra en estado RESTORING, se producirán errores en la mayoría de las instrucciones `ALTER DATABASE`. La excepción es el establecimiento de opciones de creación de reflejo de la base de datos. Es posible que una base de datos se encuentre en estado RESTORING durante una operación de restauración activa o cuando se produce un error en una operación de restauración de una base de datos o de un archivo de registro, debido a un archivo de copia de seguridad dañado.

La memoria caché de planes para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se borra si se establece alguna de las opciones siguientes.

- COLLATE
- MODIFY FILEGROUP DEFAULT
- MODIFY FILEGROUP READ_ONLY
- MODIFY FILEGROUP READ_WRITE
- MODIFY_NAME
- OFFLINE
- ONLINE
- PAGE_VERIFY
- READ_ONLY
- READ_WRITE

Al borrar la memoria caché de planes, se provoca una nueva compilación de todos los planes de ejecución posteriores y puede ocasionar una disminución repentina y temporal del rendimiento de las consultas. Para cada almacén de caché borrado de la caché de planes, el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contiene el siguiente mensaje informativo: `SQL Server has encountered %d occurrence(s) of cachestore flush for the '%s' cachestore (part of plan cache) due to some database maintenance or reconfigure operations`. Este mensaje se registra cada cinco minutos siempre que se vacíe la memoria caché dentro de ese intervalo de tiempo.

La caché de planes también se vacía en los escenarios siguientes:

- Una base de datos tiene la opción de base de datos `AUTO_CLOSE` establecida en ON. Cuando ninguna conexión de usuario hace referencia a la base de datos ni la usa, la tarea de segundo plano intenta cerrar la base de datos y apagarla de modo automático.
- Ejecuta varias consultas con una base de datos que tiene opciones predeterminadas. Después, la base de datos se quita.
- Se quita una instantánea de base de datos para una base de datos de origen.
- Volvió a generar correctamente el registro de transacciones para una base de datos.
- Restaura una copia de seguridad de una base de datos
- Separa una base de datos.

## <a name="changing-the-database-collation"></a>Cambiar la intercalación de la base de datos

Antes de aplicar otra intercalación a una base de datos, asegúrese de que se cumplen las siguientes condiciones:

- Es el único usuario que utiliza actualmente la base de datos.
- Ningún objeto enlazado a un esquema depende de la intercalación de la base de datos.

Si los objetos siguientes, que dependen de la intercalación de la base de datos, existen en la base de datos, la instrucción ALTER DATABASE *database_name* COLLATE producirá un error. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devolverá un mensaje de error para cada objeto que bloquee la acción de `ALTER`:

- Vistas y funciones definidas por el usuario creadas con SCHEMABINDING
- Columnas calculadas
- CHECK, restricciones
- Funciones con valores de tabla que devuelven tablas con columnas de caracteres con intercalaciones heredadas de la intercalación predeterminada de la base de datos
  
La información de dependencia de las entidades no vinculadas a esquemas se actualiza automáticamente si se cambia la intercalación de la base de datos.

Cambiar la intercalación de la base de datos no crea duplicados entre los nombres del sistema para los objetos de base de datos. Si se producen nombres duplicados en la intercalación cambiada, los siguientes espacios de nombres pueden provocar errores en el cambio de la intercalación de la base de datos:

- Nombres de objetos, como un procedimiento, una tabla, un desencadenador o una vista
- Nombres de esquemas
- Entidades de seguridad, como un grupo, rol o usuario
- Nombres de tipo escalar, como los tipos definidos por el usuario y por el sistema
- Nombres de catálogos de texto completo
- Nombres de columnas o parámetros en un objeto
- Nombres de índices en una tabla

Los nombres duplicados resultantes de la nueva intercalación provocarán que la acción de cambio no se ejecute correctamente y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devolverá un mensaje de error que especifica el espacio de nombres donde se ha encontrado el duplicado.

## <a name="viewing-database-information"></a>Ver la información de la base de datos

Se pueden utilizar vistas de catálogo, funciones del sistema y procedimientos almacenados del sistema para devolver información sobre bases de datos, archivos y grupos de archivos.

## <a name="permissions"></a>Permisos

Debe tener el permiso `ALTER` para la base de datos.

## <a name="examples"></a>Ejemplos

### <a name="a-changing-the-name-of-a-database"></a>A. Cambiar el nombre de una base de datos

En el ejemplo siguiente se cambia el nombre de la base de datos `AdventureWorks2012` a `Northwind`.

```sql
USE master;
GO
ALTER DATABASE AdventureWorks2012
Modify Name = Northwind ;
GO
```

### <a name="b-changing-the-collation-of-a-database"></a>B. Cambiar la intercalación de una base de datos

En el siguiente ejemplo se crea una base de datos denominada `testdb` con la intercalación `SQL_Latin1_General_CP1_CI_A`S, y luego se cambia la intercalación de la base de datos `testdb` a `COLLATE French_CI_AI`.

**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.

```sql
USE master;
GO

CREATE DATABASE testdb
COLLATE SQL_Latin1_General_CP1_CI_AS ;
GO

ALTER DATABASE testDB
COLLATE French_CI_AI ;
GO
```

## <a name="see-also"></a>Consulte también

- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)
- [sys.database_mirroring_witnesses](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)
- [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)
- [Bases de datos del sistema](../../relational-databases/databases/system-databases.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Database \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-sql-database"></a>Introducción: SQL Database

En Azure SQL Database, use esta instrucción para modificar una base de datos. Use esta instrucción para cambiar el nombre de una base de datos, cambiar el objetivo de edición y servicio de la base de datos, unir la base de datos a un grupo elástico o quitarla de uno, establecer las opciones de base de datos, agregar o quitar la base de datos como una base de datos secundaria en una relación de replicación geográfica y establecer el nivel de compatibilidad de base de datos.

Debido a su longitud, la sintaxis `ALTER DATABASE` se divide en varios artículos.

ALTER DATABASE   
En el artículo actual se proporciona la sintaxis e información relacionada para cambiar el nombre y la intercalación de una base de datos.

[Opciones SET de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md?view=azuresqldb-current&preserve-view=true)    
Proporciona la sintaxis e información relacionada para cambiar los atributos de una base de datos usando las opciones SET de ALTER DATABASE.

[Nivel de compatibilidad de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md?view=azuresqldb-current&preserve-view=true)   
Proporciona la sintaxis e información relacionada de las opciones SET de ALTER DATABASE relacionadas con los niveles de compatibilidad de la base de datos.

## <a name="syntax"></a>Sintaxis

```syntaxsql
-- Azure SQL Database Syntax
ALTER DATABASE { database_name | CURRENT }
{
    MODIFY NAME = new_database_name
  | MODIFY ( <edition_options> [, ... n] )
  | MODIFY BACKUP_STORAGE_REDUNDANCY = { 'LOCAL' | 'ZONE' | 'GEO' }
  | SET { <option_spec> [ ,... n ] WITH <termination>}
  | ADD SECONDARY ON SERVER <partner_server_name>
    [WITH ( <add-secondary-option>::=[, ... n] ) ]
  | REMOVE SECONDARY ON SERVER <partner_server_name>
  | FAILOVER
  | FORCE_FAILOVER_ALLOW_DATA_LOSS
}
[;]

<edition_options> ::=
{

  MAXSIZE = { 100 MB | 250 MB | 500 MB | 1 ... 1024 ... 4096 GB }
  | EDITION = { 'Basic' | 'Standard' | 'Premium' | 'GeneralPurpose' | 'BusinessCritical' | 'Hyperscale'}
  | SERVICE_OBJECTIVE =
       { <service-objective>
       | { ELASTIC_POOL (name = <elastic_pool_name>) }
       }
}

<add-secondary-option> ::=
   {
      ALLOW_CONNECTIONS = { ALL | NO }
     | SERVICE_OBJECTIVE =
       { <service-objective>
       | { ELASTIC_POOL ( name = <elastic_pool_name>) }
       }
   }

<service-objective> ::={ 'Basic' |'S0' | 'S1' | 'S2' | 'S3'| 'S4'| 'S6'| 'S7'| 'S9'| 'S12'
       | 'P1' | 'P2' | 'P4'| 'P6' | 'P11' | 'P15'
      | 'GP_Gen4_1' | 'GP_Gen4_2' | 'GP_Gen4_3' | 'GP_Gen4_4' | 'GP_Gen4_5' | 'GP_Gen4_6'
      | 'GP_Gen4_7' | 'GP_Gen4_8' | 'GP_Gen4_9' | 'GP_Gen4_10' | 'GP_Gen4_16' | 'GP_Gen4_24'
      | 'GP_Gen5_2' | 'GP_Gen5_4' | 'GP_Gen5_6' | 'GP_Gen5_8' | 'GP_Gen5_10' | 'GP_Gen5_12' | 'GP_Gen5_14'
      | 'GP_Gen5_16' | 'GP_Gen5_18' | 'GP_Gen5_20' | 'GP_Gen5_24' | 'GP_Gen5_32' | 'GP_Gen5_40' | 'GP_Gen5_80'
      | 'GP_Fsv2_8' | 'GP_Fsv2_10' | 'GP_Fsv2_12' | 'GP_Fsv2_14' | 'GP_Fsv2_16' | 'GP_Fsv2_18'
      | 'GP_Fsv2_20' | 'GP_Fsv2_24' | 'GP_Fsv2_32' | 'GP_Fsv2_36' | 'GP_Fsv2_72'
      | 'GP_S_Gen5_1' | 'GP_S_Gen5_2' | 'GP_S_Gen5_4' | 'GP_S_Gen5_6' | 'GP_S_Gen5_8'
      | 'GP_S_Gen5_10' | 'GP_S_Gen5_12' | 'GP_S_Gen5_14' | 'GP_S_Gen5_16'
      | 'GP_S_Gen5_18' | 'GP_S_Gen5_20' | 'GP_S_Gen5_24' | 'GP_S_Gen5_32' | 'GP_S_Gen5_40'
      | 'BC_Gen4_1' | 'BC_Gen4_2' | 'BC_Gen4_3' | 'BC_Gen4_4' | 'BC_Gen4_5' | 'BC_Gen4_6'
      | 'BC_Gen4_7' | 'BC_Gen4_8' | 'BC_Gen4_9' | 'BC_Gen4_10' | 'BC_Gen4_16' | 'BC_Gen4_24'
      | 'BC_Gen5_2' | 'BC_Gen5_4' | 'BC_Gen5_6' | 'BC_Gen5_8' | 'BC_Gen5_10' | 'BC_Gen5_12' | 'BC_Gen5_14'
      | 'BC_Gen5_16' | 'BC_Gen5_18' | 'BC_Gen5_20' | 'BC_Gen5_24' | 'BC_Gen5_32' | 'BC_Gen5_40' | 'BC_Gen5_80'
      | 'BC_M_8' | 'BC_M_10' | 'BC_M_12' | 'BC_M_14' | 'BC_M_16' | 'BC_M_18'
      | 'BC_M_20' | 'BC_M_24' | 'BC_M_32' | 'BC_M_64' | 'BC_M_128'
      | 'HS_GEN4_1' | 'HS_GEN4_2' | 'HS_GEN4_4' | 'HS_GEN4_8' | 'HS_GEN4_16' | 'HS_GEN4_24'
      | 'HS_GEN5_2' | 'HS_GEN5_4' | 'HS_GEN5_8' | 'HS_GEN5_16' | 'HS_GEN5_24' | 'HS_GEN5_32' | 'HS_GEN5_48' | 'HS_GEN5_80'
      | { ELASTIC_POOL(name = <elastic_pool_name>) }
      }

<option_spec> ::=
{
    <auto_option>
  | <change_tracking_option>
  | <cursor_option>
  | <db_encryption_option>
  | <db_update_option>
  | <db_user_access_option>
  | <delayed_durability_option>
  | <parameterization_option>
  | <query_store_options>
  | <snapshot_option>
  | <sql_option>
  | <target_recovery_time_option>
  | <termination>
  | <temporal_history_retention>
  | <compatibility_level>
    { 150 | 140 | 130 | 120 | 110 | 100 | 90 }

}
```

## <a name="arguments"></a>Argumentos

*database_name*   
Es el nombre de la base de datos que se va a modificar.

CURRENT   
Designa que la base de datos actual en uso se debe modificar.

MODIFY NAME **=** _new_database_name_   
Reemplaza el nombre de la base de datos por el nombre especificado como *new_database_name*. En el ejemplo siguiente se cambia el nombre de la base de datos `db1` a `db2`:

```sql
ALTER DATABASE db1
    MODIFY Name = db2 ;
```

MODIFY (EDITION **=** ['Basic' \| 'Standard' \| 'Premium' \|'GeneralPurpose' \| 'BusinessCritical' \| 'Hyperscale'])   
Cambia el nivel de servicio de la base de datos.

En el ejemplo siguiente se cambia la edición a `Premium`:

```sql
ALTER DATABASE current
    MODIFY (EDITION = 'Premium');
```

> [!IMPORTANT]
> Se produce un error en el cambio de EDITION si la propiedad MAXSIZE de la base de datos está establecida en un valor fuera del intervalo válido admitido por esa edición.

MODIFY (BACKUP_STORAGE_REDUNDANCY **=** ['LOCAL' \| 'ZONE' \| 'GEO'])   
Cambia la redundancia de almacenamiento de copias de seguridad de restauración a un momento dado como las copias de seguridad de retención a largo plazo (si se configuran) de la base de datos. Los cambios se aplicarán a todas las copias de seguridad que se realicen en el futuro. Las copias de seguridad existentes seguirán usando la configuración anterior. 

> [!IMPORTANT]
> La opción BACKUP_STORAGE_REDUNDANCY para Azure SQL Database solo está disponible en la versión preliminar pública en el Sur de Brasil y normalmente en la región Sudeste de Asia de Azure.  

MODIFY (MAXSIZE **=** [100 MB \| 500 MB \| 1 \| 1024...4096] GB)   
Especifica el tamaño máximo de la base de datos. El tamaño máximo debe cumplir con el conjunto válido de valores de la propiedad EDITION de la base de datos. Cambiar el tamaño máximo de la base de datos puede causar que cambie también el valor de EDITION de la base de datos.

> [!NOTE]
> El argumento **MAXSIZE** no es aplicable a bases de datos únicas en el nivel de servicio Hyperscale. Las bases de datos de nivel de servicio Hyperscale crecen según sea necesario, hasta 100 TB. El servicio SQL Database agrega almacenamiento automáticamente; no es necesario establecer un tamaño máximo.

**Modelo de DTU**

|**MAXSIZE**|**Basic**|**S0-S2**|**S3-S12**|**P1-P6**|**P11-P15**|
|-----------------|---------------|------------------|-----------------|-----------------|-----------------|-----------------|
|100 MB|√|√|√|√|√|
|250 MB|√|√|√|√|√|
|500 MB|√|√|√|√|√|
|1 GB|√|√|√|√|√|
|2 GB|√ (D)|√|√|√|√|
|5 GB|N/D|√|√|√|√|
|10 GB|N/D|√|√|√|√|
|20 GB|N/D|√|√|√|√|
|30 GB|N/D|√|√|√|√|
|40 GB|N/D|√|√|√|√|
|50 GB|N/D|√|√|√|√|
|100 GB|N/D|√|√|√|√|
|150 GB|N/D|√|√|√|√|
|200 GB|N/D|√|√|√|√|
|250 GB|N/D|√ (D)|√ (D)|√|√|
|300 GB|N/D|√|√|√|√|
|400 GB|N/A|√|√|√|√|
|500 GB|N/D|√|√|√ (D)|√|
|750 GB|N/D|√|√|√|√|
|1024 GB|N/D|√|√|√|√ (D)|
|Desde 1024 GB hasta 4096 GB en incrementos de 256 GB*|N/D|N/D|N/D|N/D|√|

\* P11 y P15 permiten un valor de MAXSIZE de hasta 4 TB, con 1024 GB como tamaño predeterminado. P11 y P15 pueden usar hasta 4 TB de almacenamiento incluido sin cargos adicionales. En el nivel Premium, un valor de MAXSIZE mayor de 1 TB está actualmente disponible en las regiones siguientes: Este de EE. UU. 2, Oeste de EE. UU., US Gov Virginia, Oeste de Europa, Centro de Alemania, Sudeste de Asia, Este de Japón, Este de Australia, Centro de Canadá y Este de Canadá. Para obtener más información sobre las limitaciones de recursos para el modelo de DTU, vea [Límites de recursos de DTU](/azure/sql-database/sql-database-dtu-resource-limits).

El valor MAXSIZE para el modelo de DTU, si se especifica, tiene que ser válido según lo que se indica en la tabla anterior para el nivel de servicio especificado.

**Modelo de núcleo virtual**

**Uso general, proceso aprovisionado, Gen4 (parte 1)**

|MAXSIZE|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6|
|:----- | ------: |-------: |-------: |-------: |-------: |--------:|
|Tamaño máximo de datos (GB)|1024|1024|1024|1536|1536|1536|

**Uso general, proceso aprovisionado, Gen4 (parte 2)**

|MAXSIZE|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24
|:----- | ------: |-------: |-------: |-------: |-------: |--------:|
|Tamaño máximo de datos (GB)|1536|3072|3072|3072|4096|4096|

**Uso general, proceso aprovisionado, Gen5 (parte 1)**

|MAXSIZE|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:----- | ------: |-------: |-------: |-------: |--------: |---------:|--------: |
|Tamaño máximo de datos (GB)|1024|1024|1024|1536|1536|1536|1536|

**Uso general, proceso aprovisionado, Gen5 (parte 2)**

|MAXSIZE|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:----- | ------: |-------: |-------: |-------: |--------: |---------:|--------: |
|Tamaño máximo de datos (GB)|3072|3072|3072|4096|4096|4096|4096|

**Uso general, proceso aprovisionado: serie Fsv2 (parte 1)**

|MAXSIZE|GP_Fsv2_8|GP_Fsv2_10|GP_Fsv2_12|GP_Fsv2_14|GP_Fsv2_16|GP_Fsv2_18|
|:----- | ------: | ------: | ------: | ------: | ------: | ------: |
|Tamaño máximo de datos (GB)|1024|1024|1024|1024|1536|1536|

**Uso general, proceso aprovisionado: serie Fsv2 (parte 2)**

|MAXSIZE|GP_Fsv2_20|GP_Fsv2_24|GP_Fsv2_32|GP_Fsv2_36|GP_Fsv2_72|
|:----- | ------: | ------: | ------: | ------: | ------: |
|Tamaño máximo de datos (GB)|1536|1536|3072|3072|4096|

**Uso general, proceso sin servidor, Gen5 (parte 1)**

|MAXSIZE|GP_S_Gen5_1|GP_S_Gen5_2|GP_S_Gen5_4|GP_S_Gen5_6|GP_S_Gen5_8|
|:----- | ------: |-------: |-------: |-------: |--------: |
|Número máximo de núcleos virtuales|1|2|4|6|8|

**Uso general, proceso sin servidor, Gen5 (parte 2)**

|MAXSIZE|GP_S_Gen5_10|GP_S_Gen5_12|GP_S_Gen5_14|GP_S_Gen5_16|
|:----- | ------: |-------: |-------: |-------: |
|Número máximo de núcleos virtuales|10|12|14|16|

**Uso general, proceso sin servidor, Gen5 (parte 3)**

|MAXSIZE|GP_S_Gen5_18|GP_S_Gen5_20|GP_S_Gen5_24|GP_S_Gen5_32|GP_S_Gen5_40|
|:----- | ------: |-------: |-------: |-------: |--------: |
|Número máximo de núcleos virtuales|18|20|24|32|40|

**Crítico para la empresa, proceso aprovisionado, Gen4 (parte 1)**

|Tamaño de proceso (objetivo de servicio)|BC_Gen4_1|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--------------- | ------: |-------: |-------: |-------: |-------: |-------: |
|Tamaño máximo de datos (GB)|1024|1024|1024|1024|1024|1024|

**Crítico para la empresa, proceso aprovisionado, Gen4 (parte 2)**

|Tamaño de proceso (objetivo de servicio)|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--------------- | ------: |-------: |-------: |--------: |--------: |--------: |
|Tamaño máximo de datos (GB)|1024|1024|1024|1024|1024|1024|

**Crítico para la empresa, proceso aprovisionado, Gen5 (parte 1)**

|MAXSIZE|BC_Gen5_2|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:----- | ------: |-------: |-------: |-------: |---------: |--------:|--------: |
|Tamaño máximo de datos (GB)|1024|1024|1024|1536|1536|1536|1536|

**Crítico para la empresa, proceso aprovisionado, Gen5 (parte 2)**

|MAXSIZE|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:----- | -------: |--------: |--------: |--------: |--------: |---------:|--------: |
|Tamaño máximo de datos (GB)|3072|3072|3072|4096|4096|4096|4096|

**Crítico para la empresa, proceso aprovisionado: serie M (parte 1)**

|MAXSIZE|BC_M_8|BC_M_10|BC_M_12|BC_M_14|BC_M_16|BC_M_18|
|:----- | -------: | -------: | -------: | -------: | -------: | -------: |
|Tamaño máximo de datos (GB)|512|640|768|896|1024|1152|

**Crítico para la empresa, proceso aprovisionado: serie M (parte 2)**

|MAXSIZE|BC_M_20|BC_M_24|BC_M_32|BC_M_64|BC_M_128|
|:----- | -------: | -------: | -------: | -------: | -------: |
|Tamaño máximo de datos (GB)|1280|1536|2048|4096|4096|

Si no hay ningún valor `MAXSIZE` establecido al utilizar el modelo de núcleo virtual, el valor predeterminado es 32 GB. Para obtener más información sobre las limitaciones de recursos para el modelo de núcleo virtual, vea [Límites de recursos del núcleo virtual](/azure/sql-database/sql-database-dtu-resource-limits).

Las reglas siguientes se aplican a los argumentos MAXSIZE y EDITION:

- Si se especifica EDITION pero no se especifica MAXSIZE, se usa el valor predeterminado de la edición. Por ejemplo, si EDITION está establecido en Standard y MAXSIZE no se especifica, el valor de MAXSIZE se establece automáticamente en 250 MB.
- Si no se especifica MAXSIZE ni EDITION, este último se establece en De uso general y MAXSIZE se establece en 32 GB.

MODIFY (SERVICE_OBJECTIVE = \<service-objective>)   
Especifica el tamaño de proceso (objetivo de servicio). En el ejemplo siguiente se cambia el objetivo de servicio de una base de datos Premium a `P6`:

```sql
ALTER DATABASE current
    MODIFY (SERVICE_OBJECTIVE = 'P6');
```

SERVICE_OBJECTIVE   

- **Solo bases de datos únicas y agrupadas**

  - Especifica el tamaño de proceso (objetivo de servicio). Los valores disponibles para el objetivo de servicio son estos: `S0`, `S1`, `S2`, `S3`, `S4`, `S6`, `S7`, `S9`, `S12`, `P1`, `P2`, `P4`, `P6`, `P11`, `P15`, `GP_GEN4_1`, `GP_GEN4_2`, `GP_GEN4_3`, `GP_GEN4_4`, `GP_GEN4_5`, `GP_GEN4_6`, `GP_GEN4_7`, `GP_GEN4_8`, `GP_GEN4_7`, `GP_GEN4_8`, `GP_GEN4_9`, `GP_GEN4_10`, `GP_GEN4_16`, `GP_GEN4_24`, `BC_GEN4_1`, `BC_GEN4_2`, `BC_GEN4_3`, `BC_GEN4_4`, `BC_GEN4_5`, `BC_GEN4_6`, `BC_GEN4_7`, `BC_GEN4_8`, `BC_GEN4_9`, `BC_GEN4_10`, `BC_GEN4_16`, `BC_GEN4_24`, `GP_Gen5_2`, `GP_Gen5_4`, `GP_Gen5_6`, `GP_Gen5_8`, `GP_Gen5_10`, `GP_Gen5_12`, `GP_Gen5_14`, `GP_Gen5_16`, `GP_Gen5_18`, `GP_Gen5_20`, `GP_Gen5_24`, `GP_Gen5_32`, `GP_Gen5_40`, `GP_Gen5_80`, `GP_Fsv2_8`, `GP_Fsv2_10`, `GP_Fsv2_12`, `GP_Fsv2_14`, `GP_Fsv2_16`, `GP_Fsv2_18`, `GP_Fsv2_20`, `GP_Fsv2_24`, `GP_Fsv2_32`, `GP_Fsv2_36`, `GP_Fsv2_72`, `BC_Gen5_2`, `BC_Gen5_4`, `BC_Gen5_6`, `BC_Gen5_8`, `BC_Gen5_10`, `BC_Gen5_12`, `BC_Gen5_14`, `BC_Gen5_16`, `BC_Gen5_18`, `BC_Gen5_20`, `BC_Gen5_24`, `BC_Gen5_32`,`BC_Gen5_40`, `BC_Gen5_80`, `BC_M_8`, `BC_M_10`, `BC_M_12`, `BC_M_14`, `BC_M_16`, `BC_M_18`, `BC_M_20`, `BC_M_24`, `BC_M_32`, `BC_M_64`, `BC_M_128`.

- **Para bases de datos únicas en el nivel de proceso sin servidor**

  - Especifica el tamaño de proceso (objetivo de servicio). Los valores disponibles para el objetivo de servicio son: `GP_S_Gen5_1`, `GP_S_Gen5_2`, `GP_S_Gen5_4`, `GP_S_Gen5_6`, `GP_S_Gen5_8`, `GP_S_Gen5_10`, `GP_S_Gen5_12`, `GP_S_Gen5_14`, `GP_S_Gen5_16`, `GP_S_Gen5_18`, `GP_S_Gen5_20`, `GP_S_Gen5_24`, `GP_S_Gen5_32`, `GP_S_Gen5_40`.

- **Para bases de datos en el nivel de servicio Hyperscale**

  - Especifica el tamaño de proceso (objetivo de servicio). Los valores disponibles para el objetivo de servicio son: `HS_GEN4_1` `HS_GEN4_2` `HS_GEN4_4` `HS_GEN4_8` `HS_GEN4_16`, `HS_GEN4_24`, `HS_Gen5_2`, `HS_Gen5_4`, `HS_Gen5_8`, `HS_Gen5_16`, `HS_Gen5_24`, `HS_Gen5_32`, `HS_Gen5_48`, `HS_Gen5_80`.

Para conocer las descripciones de los objetivos de servicio y obtener más información sobre las combinaciones de tamaño, ediciones y objetivos de servicio, consulte [Niveles de servicio y niveles de rendimiento de Azure SQL Database](/azure/azure-sql/database/purchasing-models), [Límites de recursos de DTU](/azure/sql-database/sql-database-dtu-resource-limits) y [Límites de recursos del núcleo virtual](/azure/sql-database/sql-database-dtu-resource-limits). Se ha quitado la compatibilidad para los objetivos de servicio de PRS. Si tiene preguntas, use este alias de correo electrónico: premium-rs@microsoft.com.

MODIFY (SERVICE_OBJECTIVE = ELASTIC\_POOL (name = \<elastic_pool_name>)   
Para agregar una base de datos existente a un grupo elástico, establezca el valor SERVICE_OBJECTIVE de la base de datos en ELASTIC_POOL e indique el nombre del grupo. También se puede usar esta opción para cambiar la base de datos a un grupo elástico diferente dentro del mismo servidor. Para obtener más información, vea [Create and manage a SQL Database elastic pool](/azure/azure-sql/database/elastic-pool-overview) (Creación y administración de un grupo elástico de SQL Database). A fin de quitar una base de datos de un grupo elástico, use ALTER DATABASE para establecer el valor SERVICE_OBJECTIVE en un tamaño de proceso de base de datos única (objetivo de servicio).

> [!NOTE]
> No se pueden agregar bases de datos del nivel de servicio Hyperscale a un grupo elástico.

ADD SECONDARY ON SERVER \<partner_server_name>   
Crea una base de datos de replicación geográfica secundaria con el mismo nombre en un servidor asociado, lo que convierte a la base de datos local en una base de datos principal de replicación geográfica, y comienza a replicar los datos de forma asincrónica desde la base de datos principal a la nueva base de datos secundaria. Si ya existe una base de datos con el mismo nombre en la base de datos secundaria, se produce un error en el comando. El comando se ejecuta en la base de datos maestra en el servidor que hospeda la base de datos local que se convierte en la principal.

> [!IMPORTANT]
> El nivel de servicio Hyperscale no es compatible actualmente con la replicación geográfica.

> [!IMPORTANT]
> De forma predeterminada, la base de datos secundaria se crea con la misma redundancia de almacenamiento de copia de seguridad que la de la base de datos principal o de origen. No se admite el cambio de la redundancia de almacenamiento de copia de seguridad al crear la base de datos secundaria a través de T-SQL. 

WITH ALLOW_CONNECTIONS { **ALL** | NO }   
Cuando no se especifica ALLOW_CONNECTIONS, se establece en ALL de forma predeterminada. Si se establece en ALL, es una base de datos de solo lectura que permite la conexión de todos los inicios de sesión con los permisos adecuados.

WITH SERVICE_OBJECTIVE { `S0`, `S1`, `S2`, `S3`, `S4`, `S6`, `S7`, `S9`, `S12`, `P1`, `P2`, `P4`, `P6`, `P11`, `P15`, `GP_GEN4_1`, `GP_GEN4_2`, `GP_GEN4_3`, `GP_GEN4_4`, `GP_GEN4_5`, `GP_GEN4_6`, `GP_GEN4_7`, `GP_GEN4_8`, `GP_GEN4_7`, `GP_GEN4_8`, `GP_GEN4_9`, `GP_GEN4_10`, `GP_GEN4_16`, `GP_GEN4_24`, `BC_GEN4_1`, `BC_GEN4_2`, `BC_GEN4_3`, `BC_GEN4_4`, `BC_GEN4_5`, `BC_GEN4_6`, `BC_GEN4_7`, `BC_GEN4_8`, `BC_GEN4_9`, `BC_GEN4_10`, `BC_GEN4_16`, `BC_GEN4_24`, `GP_Gen5_2`, `GP_Gen5_4`, `GP_Gen5_6`, `GP_Gen5_8`, `GP_Gen5_10`, `GP_Gen5_12`, `GP_Gen5_14`, `GP_Gen5_16`, `GP_Gen5_18`, `GP_Gen5_20`, `GP_Gen5_24`, `GP_Gen5_32`, `GP_Gen5_40`, `GP_Gen5_80`, `GP_Fsv2_8`, `GP_Fsv2_10`, `GP_Fsv2_12`, `GP_Fsv2_14`, `GP_Fsv2_16`, `GP_Fsv2_18`, `GP_Fsv2_20`, `GP_Fsv2_24`, `GP_Fsv2_32`, `GP_Fsv2_36`, `GP_Fsv2_72`, `GP_S_Gen5_1`, `GP_S_Gen5_2`, `GP_S_Gen5_4`, `GP_S_Gen5_6`, `GP_S_Gen5_8`, `GP_S_Gen5_10`, `GP_S_Gen5_12`, `GP_S_Gen5_14`, `GP_S_Gen5_16`, `GP_S_Gen5_18`, `GP_S_Gen5_20`, `GP_S_Gen5_24`, `GP_S_Gen5_32`, `GP_S_Gen5_40`, `BC_Gen5_2`, `BC_Gen5_4`, `BC_Gen5_6`, `BC_Gen5_8`, `BC_Gen5_10`, `BC_Gen5_12`, `BC_Gen5_14`, `BC_Gen5_16`, `BC_Gen5_18`, `BC_Gen5_20`, `BC_Gen5_24`, `BC_Gen5_32`,`BC_Gen5_40`, `BC_Gen5_80`, `BC_M_8`, `BC_M_10`, `BC_M_12`, `BC_M_14`, `BC_M_16`, `BC_M_18`, `BC_M_20`, `BC_M_24`, `BC_M_32`, `BC_M_64`, `BC_M_128` }

Cuando no se especifica SERVICE_OBJECTIVE, la base de datos secundaria se crea en el mismo nivel de servicio que la principal. Cuando se especifica SERVICE_OBJECTIVE, la base de datos secundaria se crea en el nivel especificado. Esta opción permite crear bases de datos secundarias con replicación geográfica con niveles de servicio más económicos. El valor SERVICE_OBJECTIVE especificado debe estar en la misma edición que el origen. Por ejemplo, no se puede especificar S0 si la edición es Premium.

ELASTIC_POOL (name = \<elastic_pool_name>) Si no se especifica ELASTIC_POOL, la base de datos secundaria no se creará en un grupo elástico. Cuando se especifica ELASTIC_POOL, la base de datos secundaria se crea en el grupo especificado.

> [!IMPORTANT]
> El usuario que ejecuta el comando ADD SECONDARY debe ser DBManager en el servidor principal, ser miembro de db_owner en la base de datos local y DBManager en el servidor secundario. La dirección IP del cliente debe agregarse a la lista de permitidas de las reglas del firewall, tanto para el servidor primario como para el secundario. En el caso de que haya diferentes direcciones IP del cliente, también deberá agregar al servidor secundario la misma que se haya agregado al principal. Este paso se tiene que realizar antes de ejecutar el comando ADD SECONDARY para iniciar la replicación geográfica.

REMOVE SECONDARY ON SERVER \<partner_server_name> Quita la base de datos secundaria con replicación geográfica indicada en el servidor especificado. El comando se ejecuta en la base de datos maestra en el servidor que hospeda la base de datos principal.

> [!IMPORTANT]
> El usuario que ejecuta el comando REMOVE SECONDARY debe ser DBManager en el servidor principal.

FAILOVER Promueve la base de datos secundaria en colaboración de replicación geográfica en la que el comando se ejecuta para convertirla en la principal y degrada la base de datos principal actual para convertirla en la nueva base de datos secundaria. Como parte de este proceso, el modo de replicación geográfica se cambia temporalmente de asincrónico a sincrónico. Durante el proceso de conmutación por error:

1. La base de datos principal deja de aceptar nuevas transacciones.
2. Todas las transacciones pendientes se vacían en la base de datos secundaria.
3. La base de datos secundaria se convierte en la principal y comienza la replicación geográfica asincrónica con la base de datos principal anterior y la base de datos secundaria nueva.

Esta secuencia garantiza que no se produzca pérdida de datos. El período durante el que ambas bases de datos no están disponibles es de entre 0 y 25 segundos mientras se intercambian los roles. La operación total no debería tardar más de un minuto aproximadamente. Si la base de datos principal no está disponible cuando se emite este comando, el comando produce un error con un mensaje de error en el que se indica que la base de datos principal no está disponible. Si el proceso de conmutación por error no se completa y aparece bloqueado, se puede usar el comando para forzar la conmutación por error y aceptar la pérdida de datos, y después, si tiene que recuperar los datos perdidos, llama a devops (CSS) para recuperarlos.

> [!IMPORTANT]
> El usuario que ejecuta el comando FAILOVER debe ser DBManager tanto en el servidor principal como en el secundario.

FORCE_FAILOVER_ALLOW_DATA_LOSS Promueve la base de datos secundaria en colaboración de replicación geográfica en la que el comando se ejecuta para convertirla en la principal y degrada la base de datos principal actual para convertirla en la nueva base de datos secundaria. Use este comando solo cuando la base de datos principal actual ya no esté disponible. Está diseñado solo para la recuperación ante desastres, cuando la restauración de la disponibilidad sea esencial y se acepte cierta pérdida de datos.

Durante una conmutación por error forzada:

1. La base de datos secundaria especificada se convierte inmediatamente en la base de datos principal y comienza a aceptar nuevas transacciones.
2. Cuando la base de datos principal original se pueda volver a conectar con la nueva base de datos principal, se toma una copia de seguridad incremental en la base de datos principal original y se convierte en una nueva base de datos secundaria.
3. Para recuperar datos de esta copia de seguridad incremental en la base de datos principal anterior, el usuario usa devops/CSS.
4. Si hay otras bases de datos secundarias, se reconfiguran de forma automática para convertirse en secundarias de la nueva base de datos principal. Este proceso es asincrónico y puede haber un retraso hasta que finalice. Hasta que se haya completado la reconfiguración, las bases de datos secundarias siguen siendo secundarias de la base de datos principal anterior.

> [!IMPORTANT]
> El usuario que ejecuta el comando `FORCE_FAILOVER_ALLOW_DATA_LOSS` debe pertenecer al rol `dbmanager` tanto en el servidor principal como en el secundario.

## <a name="remarks"></a>Observaciones

Para quitar una base de datos, use [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md).
Para reducir el tamaño de una base de datos, use [DBCC SHRINKDATABASE](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md).

La instrucción `ALTER DATABASE` se debe ejecutar en modo de confirmación automática (modo predeterminado de administración de transacciones) y no se permite en una transacción explícita o implícita.

Al borrar la memoria caché de planes, se provoca una nueva compilación de todos los planes de ejecución posteriores y puede ocasionar una disminución repentina y temporal del rendimiento de las consultas. Para cada almacén de caché borrado de la caché de planes, el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contiene el siguiente mensaje informativo: `SQL Server has encountered %d occurrence(s) of cachestore flush for the '%s' cachestore (part of plan cache) due to some database maintenance or reconfigure operations`. Este mensaje se registra cada cinco minutos siempre que se vacíe la memoria caché dentro de ese intervalo de tiempo.

La caché de procedimientos también se vacía en el escenario siguiente: Ejecuta varias consultas con una base de datos que tiene opciones predeterminadas. Después, la base de datos se quita.

## <a name="viewing-database-information"></a>Ver la información de la base de datos

Se pueden utilizar vistas de catálogo, funciones del sistema y procedimientos almacenados del sistema para devolver información sobre bases de datos, archivos y grupos de archivos.

## <a name="permissions"></a>Permisos

Para modificar una base de datos, un inicio de sesión debe ser ya sea el inicio de sesión de la entidad de seguridad a nivel de servidor (creado por el proceso de aprovisionamiento), un miembro del rol de base de datos `dbmanager` en la rama maestra, un miembro del rol de base de datos `db_owner` en la base de datos actual o `dbo` de la base de datos.

## <a name="examples"></a>Ejemplos

### <a name="a-check-the-edition-options-and-change-them"></a>A. Comprobación y cambio de las opciones de edición

Establece un tamaño máximo y de edición para la base de datos db1:

```sql
SELECT Edition = DATABASEPROPERTYEX('db1', 'EDITION'),
        ServiceObjective = DATABASEPROPERTYEX('db1', 'ServiceObjective'),
        MaxSizeInBytes =  DATABASEPROPERTYEX('db1', 'MaxSizeInBytes');

ALTER DATABASE [db1] MODIFY (EDITION = 'Premium', MAXSIZE = 1024 GB, SERVICE_OBJECTIVE = 'P15');
```

### <a name="b-moving-a-database-to-a-different-elastic-pool"></a>B. Movimiento de una base de datos a otro grupo elástico

Mueve una base de datos existente en un grupo denominado pool1:

```sql
ALTER DATABASE db1
MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL ( name = pool1 ) ) ;
```

### <a name="c-add-a-geo-replication-secondary"></a>C. Adición de una base de datos secundaria de replicación geográfica

Crea una base de datos secundaria legible db1 en el servidor `secondaryserver` de db1 en el servidor local.

```sql
ALTER DATABASE db1
ADD SECONDARY ON SERVER secondaryserver
WITH ( ALLOW_CONNECTIONS = ALL )
```

### <a name="d-remove-a-geo-replication-secondary"></a>D. Eliminación de una base de datos secundaria de replicación geográfica

Quita la base de datos secundaria db1 en el servidor `secondaryserver`.

```sql
ALTER DATABASE db1
REMOVE SECONDARY ON SERVER testsecondaryserver
```

### <a name="e-failover-to-a-geo-replication-secondary"></a>E. Conmutación por error a una base de datos secundaria de replicación geográfica

Promueve una base de datos secundaria db1 en el servidor `secondaryserver` para que se convierta en la nueva base de datos principal cuando se ejecute en el servidor `secondaryserver`.

```sql
ALTER DATABASE db1 FAILOVER
```

### <a name="e-force-failover-to-a-geo-replication-secondary-with-data-loss"></a>E. Forzado de la conmutación por error a una base de datos secundaria de replicación geográfica con pérdida de datos

Obliga a una base de datos secundaria db1 en el servidor `secondaryserver` a convertirse en la nueva base de datos principal cuando se ejecute en el servidor `secondaryserver`, en el caso de que el servidor principal ya no esté disponible. Esta opción puede conllevar la pérdida de datos.

```sql
ALTER DATABASE db1 FORCE_FAILOVER_ALLOW_DATA_LOSS
```

### <a name="g-update-a-single-database-to-service-tier-s0-standard-edition-performance-level-0"></a>G. Actualizar una única base de datos al nivel de servicio S0 (edición Estándar, nivel de rendimiento 0)

Actualiza una base de datos única a la edición Estándar (nivel de servicio) con un tamaño de proceso (objetivo de servicio) de S0 y un tamaño máximo de 250 GB.

```sql
ALTER DATABASE [db1] MODIFY (EDITION = 'Standard', MAXSIZE = 250 GB, SERVICE_OBJECTIVE = 'S0');
```

### <a name="h-update-the-backup-storage-redundancy-of-a-database"></a>H. Actualización de la redundancia de almacenamiento de copia de seguridad de una base de datos

Actualiza la redundancia de almacenamiento de copia de seguridad de una base de datos a la redundancia de zona. Todas las copias de seguridad futuras de esta base de datos utilizarán la nueva configuración. Esto incluye las copias de seguridad de restauración a un momento dado y las de retención a largo plazo (si se configuran). 

```sql
ALTER DATABASE db1 MODIFY BACKUP_STORAGE_REDUNDANCY = 'ZONE'
```

## <a name="see-also"></a>Consulte también

- [CREATE DATABASE - Azure SQL Database](../../t-sql/statements/create-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)
- [sys.database_mirroring_witnesses](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)
- [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)
- [Bases de datos del sistema](../../relational-databases/databases/system-databases.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Database](alter-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::



&nbsp;

## <a name="overview-azure-sql-managed-instance"></a>Introducción: Instancia administrada de Azure SQL

En [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)], use esta instrucción para establecer las opciones de base de datos.

Debido a su longitud, la sintaxis `ALTER DATABASE` se divide en varios artículos.

ALTER DATABASE   
En el artículo actual se proporciona la sintaxis e información relacionada para establecer las opciones File y Filegroup, para establecer las opciones de base de datos y el nivel de compatibilidad de base de datos.  
  
[Opciones File y Filegroup de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md?&tabs=sqldbmi)   
Proporciona la sintaxis e información relacionada para agregar y eliminar archivos y grupos de archivos de una base de datos y para cambiar los atributos de archivos y grupos de archivos.  
  
[Opciones SET de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md?&tabs=sqldbmi)   
Proporciona la sintaxis e información relacionada para cambiar los atributos de una base de datos usando las opciones SET de ALTER DATABASE.  
  
[Nivel de compatibilidad de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md?&tabs=sqldbmi)   
Proporciona la sintaxis e información relacionada de las opciones SET de ALTER DATABASE relacionadas con los niveles de compatibilidad de la base de datos.  

## <a name="syntax"></a>Sintaxis

```syntaxsql
-- Azure SQL Database Syntax  
ALTER DATABASE { database_name | CURRENT }  
{
    MODIFY NAME = new_database_name
  | COLLATE collation_name
  | <file_and_filegroup_options>  
  | SET <option_spec> [ ,...n ]  
}  
[;]

<file_and_filegroup_options>::=  
  <add_or_modify_files>::=  
  <filespec>::=
  <add_or_modify_filegroups>::=  
  <filegroup_updatability_option>::=  

<option_spec> ::=
{
    <auto_option>
  | <change_tracking_option>
  | <cursor_option>
  | <db_encryption_option>  
  | <db_update_option>
  | <db_user_access_option>
  | <delayed_durability_option>
  | <parameterization_option>
  | <query_store_options>
  | <snapshot_option>
  | <sql_option>
  | <target_recovery_time_option>
  | <temporal_history_retention>
  | <compatibility_level>
      { 150 | 140 | 130 | 120 | 110 | 100 | 90 }

}  

```

## <a name="arguments"></a>Argumentos

*database_name*   
Es el nombre de la base de datos que se va a modificar.

CURRENT   
Designa que la base de datos actual en uso se debe modificar.

## <a name="remarks"></a>Observaciones

- Para quitar una base de datos, use [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md).

- Para reducir el tamaño de una base de datos, use [DBCC SHRINKDATABASE](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md).

- La instrucción `ALTER DATABASE` se debe ejecutar en modo de confirmación automática (modo predeterminado de administración de transacciones) y no se permite en una transacción explícita o implícita.

- La caché de planes de Instancia administrada se borra si se establece una de las opciones siguientes.
    - COLLATE
    - MODIFY FILEGROUP DEFAULT
    - MODIFY FILEGROUP READ_ONLY
    - MODIFY FILEGROUP READ_WRITE
    - MODIFY NAME

    Al borrar la memoria caché de planes, se provoca una nueva compilación de todos los planes de ejecución posteriores y puede ocasionar una disminución repentina y temporal del rendimiento de las consultas. Para cada almacén de caché borrado de la caché de planes, el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contendrá el siguiente mensaje informativo: "[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha detectado %d instancias de vaciado del almacén de caché "%s" (parte de la memoria caché de planes) debido a determinadas operaciones de mantenimiento de base de datos o reconfiguración". Este mensaje se registra cada cinco minutos siempre que se vacíe la memoria caché dentro de ese intervalo de tiempo.
También se vacía la caché de planes cuando se ejecutan varias consultas en una base de datos que tiene las opciones predeterminadas. Después, la base de datos se quita.

- Para ejecutarse, algunas instrucciones `ALTER DATABASE` necesitan un bloqueo exclusivo en una base de datos. Por este motivo es posible que se produzca un error cuando otro proceso activo mantenga un bloqueo en la base de datos. Un error que se notifica en estos casos es `Msg 5061, Level 16, State 1, Line 38` con el mensaje `ALTER DATABASE failed because a lock could not be placed on database '<database name>'. Try again later`. Suele ser un error transitorio y, para resolverlo, una vez que se hayan liberado todos los bloqueos de la base de datos, vuelva a intentar la instrucción ALTER DATABASE en la que se ha producido el error. La vista del sistema `sys.dm_tran_locks` contiene información sobre los bloqueos activos. Para comprobar si hay bloqueos compartidos o exclusivos en una base de datos, use la consulta siguiente.
  
    ```sql
    SELECT
        resource_type, resource_database_id, request_mode, request_type, request_status, request_session_id 
    FROM 
        sys.dm_tran_locks
    WHERE
        resource_database_id = DB_ID('testdb')
    ```
  
## <a name="viewing-database-information"></a>Ver la información de la base de datos

Se pueden utilizar vistas de catálogo, funciones del sistema y procedimientos almacenados del sistema para devolver información sobre bases de datos, archivos y grupos de archivos.

## <a name="permissions"></a>Permisos

Solo el inicio de sesión principal de nivel de servidor (creado por el proceso de aprovisionamiento) o los miembros del rol de base de datos `dbcreator` pueden modificar una base de datos.

> [!IMPORTANT]
> El propietario de la base de datos no puede modificarla a menos que sea miembro del rol `dbcreator`.

## <a name="examples"></a>Ejemplos

En los ejemplos siguientes se muestra cómo establecer el ajuste automático y cómo agregar un archivo en una instancia administrada.

```sql
ALTER DATABASE WideWorldImporters
  SET AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = ON)

ALTER DATABASE WideWorldImporters
  ADD FILE (NAME = 'data_17')
```

## <a name="see-also"></a>Consulte también

- [CREATE DATABASE - Azure SQL Database](../../t-sql/statements/create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)
- [sys.database_mirroring_witnesses](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)[sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)
- [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)
- [Bases de datos del sistema](../../relational-databases/databases/system-databases.md)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Database](alter-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-azure-synapse-analytics"></a>Introducción: Azure Synapse Analytics

En Azure Synapse, `ALTER DATABASE` modifica el nombre, el tamaño máximo o el objetivo de servicio de una base de datos.

Debido a su longitud, la sintaxis `ALTER DATABASE` se divide en varios artículos.

[Opciones SET de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
Proporciona la sintaxis e información relacionada para cambiar los atributos de una base de datos usando las opciones SET de ALTER DATABASE.

## <a name="syntax"></a>Sintaxis

### <a name="dedicated-sql-pool"></a>[Grupo de SQL dedicado](#tab/sqlpool)
```syntaxsql
ALTER DATABASE { database_name | CURRENT }
{
  MODIFY NAME = new_database_name
| MODIFY ( <edition_option> [, ... n] )
| SET <option_spec> [ ,...n ] [ WITH <termination> ]
}
[;]

<edition_option> ::=
      MAXSIZE = {
            250 | 500 | 750 | 1024 | 5120 | 10240 | 20480
          | 30720 | 40960 | 51200 | 61440 | 71680 | 81920
          | 92160 | 102400 | 153600 | 204800 | 245760
      } GB
      | SERVICE_OBJECTIVE = {
            'DW100' | 'DW200' | 'DW300' | 'DW400' | 'DW500'
          | 'DW600' | 'DW1000' | 'DW1200' | 'DW1500' | 'DW2000'
          | 'DW3000' | 'DW6000' | 'DW500c' | 'DW1000c' | 'DW1500c'
          | 'DW2000c' | 'DW2500c' | 'DW3000c' | 'DW5000c' | 'DW6000c'
          | 'DW7500c' | 'DW10000c' | 'DW15000c' | 'DW30000c'
      }
```
### <a name="serverless-sql-pool"></a>[Grupo de SQL sin servidor](#tab/sqlod)
```syntaxsql
ALTER DATABASE { database_name | Current } 
{ 
    COLLATE collation_name 
  | SET { <optionspec> [ ,...n ] } 
} 
[;] 

<optionspec> ::= 
{ 
    <auto_option> 
  | <sql_option> 
}  

<auto_option> ::= 
{ 
    AUTO_CREATE_STATISTICS { OFF | ON [ ( INCREMENTAL = { ON | OFF } ) ] } 
  | AUTO_UPDATE_STATISTICS { ON | OFF } 
  | AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF } 
} 

<sql_option> ::= 
{ 
    ANSI_NULL_DEFAULT { ON | OFF } 
  | ANSI_NULLS { ON | OFF } 
  | ANSI_PADDING { ON | OFF } 
  | ANSI_WARNINGS { ON | OFF } 
  | ARITHABORT { ON | OFF } 
  | COMPATIBILITY_LEVEL = { 140 | 130 | 120 | 110 | 100 } 
  | CONCAT_NULL_YIELDS_NULL { ON | OFF } 
  | NUMERIC_ROUNDABORT { ON | OFF } 
  | QUOTED_IDENTIFIER { ON | OFF } 
} 
```
---

## <a name="arguments"></a>Argumentos

*database_name*   
Especifica el nombre de la base de datos que se va a modificar.

MODIFY NAME = *new_database_name*   
Reemplaza el nombre de la base de datos por el nombre especificado como *new_database_name*.

MAXSIZE   
El valor predeterminado es 245 760 GB (240 TB).

**Se aplica a:** Optimizado para Compute Gen1

El tamaño máximo permitido para la base de datos. La base de datos no puede superar el valor de MAXSIZE.

**Se aplica a:** Optimizado para Compute Gen2

Tamaño máximo permitido para los datos de almacenamiento de filas de la base de datos. Los datos almacenados en tablas de almacenamiento de filas, en el almacén delta de un índice de almacén de columnas o en un índice no agrupado de un índice de almacén de columnas en clúster no pueden superar el valor de MAXSIZE. Los datos comprimidos en formato de almacén de columnas no tienen un límite de tamaño y no están restringidos por el valor de MAXSIZE.

SERVICE_OBJECTIVE   
Especifica el tamaño de proceso (objetivo de servicio). Para más información sobre los objetivos de servicio para Azure Synapse, consulte [Unidades de almacenamiento de datos (DWU)](/azure/sql-data-warehouse/what-is-a-data-warehouse-unit-dwu-cdwu).

## <a name="permissions"></a>Permisos

Se requieren estos permisos:

- Inicio de sesión principal en el nivel de servidor (creado por el proceso de aprovisionamiento), o
- Miembro del rol de base de datos `dbmanager`.

El propietario de la base de datos no puede modificarla a menos que sea miembro del rol `dbmanager`.

## <a name="general-remarks"></a>Notas generales

La base de datos actual debe ser diferente de la base de datos que está modificando, por lo que **ALTER debe ejecutarse mientras se está conectado a la base de datos maestra**.

COMPATIBILITY_LEVEL en SQL Analytics se establece en 130 de forma predeterminada y no se puede cambiar. Para más información, vea [Rendimiento de consultas mejorado con el nivel de compatibilidad 130 en Azure SQL Database](./alter-database-transact-sql-compatibility-level.md).

> [!NOTE]
> COMPATIBILITY_LEVEL solo se aplica a los recursos aprovisionados (grupos).

## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones

Para ejecutar `ALTER DATABASE`, la base de datos debe estar en línea y no puede encontrarse en estado de pausa.

La instrucción `ALTER DATABASE` debe ejecutarse en modo de confirmación automática, que es el modo de administración de transacciones predeterminado. Esto se establece en la configuración de conexión.

La instrucción `ALTER DATABASE` no puede formar parte de una transacción definida por el usuario.

No es posible cambiar la intercalación de base de datos.

## <a name="examples"></a>Ejemplos

Antes de ejecutar estos ejemplos, asegúrese de que la base de datos que está modificando no es la base de datos actual. La base de datos actual debe ser diferente de la base de datos que está modificando, por lo que **ALTER debe ejecutarse mientras se está conectado a la base de datos maestra**.

### <a name="a-change-the-name-of-the-database"></a>A. Cambiar el nombre de la base de datos

```sql
ALTER DATABASE AdventureWorks2012
MODIFY NAME = Northwind;
```

### <a name="b-change-max-size-for-the-database"></a>B. Cambiar el tamaño máximo de la base de datos

```sql
ALTER DATABASE dw1 MODIFY ( MAXSIZE=10240 GB );
```

### <a name="c-change-the-compute-size-service-objective"></a>C. Cambiar el tamaño de proceso (objetivo de servicio)

```sql
ALTER DATABASE dw1 MODIFY ( SERVICE_OBJECTIVE= 'DW1200' );
```

### <a name="d-change-the-max-size-and-the-compute-size-service-objective"></a>D. Cambiar el tamaño máximo y el tamaño de proceso (objetivo de servicio)

```sql
ALTER DATABASE dw1 MODIFY ( MAXSIZE=10240 GB, SERVICE_OBJECTIVE= 'DW1200' );
```

## <a name="see-also"></a>Consulte también

- [CREATE DATABASE (Azure Synapse Analytics)](../../t-sql/statements/create-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
- [Lista de artículos de referencia de Azure Synapse Analytics](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-reference-tsql-language-elements)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Database](alter-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Analytics<br />Platform System (PDW) \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-analytics-platform-system"></a>Introducción: Sistema de la plataforma de análisis

Modifica las opciones de tamaño máximo de la base de datos para las tablas replicadas, tablas distribuidas y el registro de transacciones en PDW. Use esta instrucción para administrar las asignaciones de espacio de disco para una base de datos a medida que aumenta o disminuye de tamaño. En el artículo también se describe la sintaxis relacionada con la configuración de opciones de base de datos en PDW.

## <a name="syntax"></a>Sintaxis

```syntaxsql
-- Analytics Platform System
ALTER DATABASE database_name
    SET ( <set_database_options> | <db_encryption_option> )
[;]

<set_database_options> ::=
{
    AUTOGROW = { ON | OFF }
    | REPLICATED_SIZE = size [GB]
    | DISTRIBUTED_SIZE = size [GB]
    | LOG_SIZE = size [GB]
    | SET AUTO_CREATE_STATISTICS { ON | OFF }
    | SET AUTO_UPDATE_STATISTICS { ON | OFF }
    | SET AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }
}

<db_encryption_option> ::=
    ENCRYPTION { ON | OFF }
```

## <a name="arguments"></a>Argumentos

*database_name*   
Nombre de la base de datos que se va a modificar. Para mostrar una lista de bases de datos en el dispositivo, use [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

AUTOGROW = { ON | OFF }   
Actualiza la opción AUTOGROW. Cuando AUTOGROW es ON, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] aumenta automáticamente el espacio asignado para las tablas replicadas, tablas distribuidas y el registro de transacciones según sea necesario para adecuarse al crecimiento de los requisitos de almacenamiento. Cuando AUTOGROW es OFF, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] devuelve un error si las tablas replicadas, tablas distribuidas o el registro de transacciones supera la configuración de tamaño máximo.

REPLICATED_SIZE = *tamaño* [GB]   
Especifica el nuevo tamaño máximo en gigabytes por nodo de ejecución para almacenar todas las tablas replicadas en la base de datos que se va a modificar. Si tiene planeado el espacio de almacenamiento del dispositivo, debe multiplicar REPLICATED_SIZE por el número de nodos de ejecución en el dispositivo.

DISTRIBUTED_SIZE = *tamaño* [GB]   
Especifica el nuevo tamaño máximo en gigabytes por base de datos para almacenar todas las tablas distribuidas de la base de datos que se va a modificar. El tamaño se distribuye entre todos los nodos de ejecución en el dispositivo.

LOG_SIZE = *tamaño* [GB]   
Especifica el nuevo tamaño máximo en gigabytes por base de datos para almacenar todas los registros de transacciones de la base de datos que se va a modificar. El tamaño se distribuye entre todos los nodos de ejecución en el dispositivo.

ENCRYPTION { ON | OFF }   
Establece que se cifre (ON) o no se cifre (OFF) la base de datos. Solo se puede configurar el cifrado para [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] cuando [sp_pdw_database_encryption](../../relational-databases/system-stored-procedures/sp-pdw-database-encryption-sql-data-warehouse.md) se haya establecido en **1**. Se debe crear una clave de cifrado de base de datos antes de poder configurar el cifrado de datos transparente. Para obtener más información sobre el cifrado de bases de datos, vea [Cifrado de datos transparente (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md).

SET AUTO_CREATE_STATISTICS { ON | OFF }   
Cuando está activada la opción automática de creación de estadísticas, AUTO_CREATE_STATISTICS, el optimizador de consultas crea las estadísticas en columnas individuales en el predicado de consulta, según sea necesario, para mejorar las estimaciones de cardinalidad del plan de consulta. Estas estadísticas de columna única se crean en las columnas que aún no tienen un histograma en un objeto de estadísticas existente.

El valor predeterminado es ON para las bases de datos creadas después de actualizar a AU7. El valor predeterminado es OFF para las bases de datos creadas antes de la actualización.

Para obtener más información sobre las estadísticas, vea [Estadísticas](../../relational-databases/statistics/statistics.md).

SET AUTO_UPDATE_STATISTICS { ON | OFF }   
Cuando está activada la opción automática de actualización de estadísticas, AUTO_UPDATE_STATISTICS, el optimizador de consultas determina cuándo las estadísticas pueden quedar obsoletas y las actualiza cuando una consulta las usa. Las estadísticas se vuelven obsoletas después de que las operaciones de inserción, actualización, eliminación o combinación cambien la distribución de los datos en la tabla o la vista indexada. El optimizador de consultas determina cuándo han podido quedar obsoletas las estadísticas contando el número de modificaciones de datos desde la actualización más reciente de las estadísticas, comparando el número de modificaciones con respecto a un umbral. El umbral se basa en el número de filas de la tabla o la vista indizada.

El valor predeterminado es ON para las bases de datos creadas después de actualizar a AU7. El valor predeterminado es OFF para las bases de datos creadas antes de la actualización.

Para obtener más información sobre las estadísticas, vea [Estadísticas](../../relational-databases/statistics/statistics.md).

SET AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }   
La opción de actualización asincrónica de estadísticas AUTO_UPDATE_STATISTICS_ASYNC determina si el Optimizador de consultas utiliza actualizaciones sincrónicas o asincrónicas de las estadísticas. La opción AUTO_UPDATE_STATISTICS_ASYNC se aplica a los objetos de estadística creados para índices y columnas únicas de los predicados de consulta, así como a las estadísticas creadas con la instrucción CREATE STATISTICS.

El valor predeterminado es ON para las bases de datos creadas después de actualizar a AU7. El valor predeterminado es OFF para las bases de datos creadas antes de la actualización.

Para obtener más información sobre las estadísticas, vea [Estadísticas](../../relational-databases/statistics/statistics.md).

## <a name="permissions"></a>Permisos

Se necesita el permiso `ALTER` en la base de datos.

## <a name="error-messages"></a>mensajes de error

Si se deshabilitan las estadísticas automáticas e intenta modificar las estadísticas, PDW genera el error `This option is not supported in PDW`. El administrador del sistema puede habilitar las estadísticas automáticas si habilita el modificador de características [AutoStatsEnabled](../../analytics-platform-system/appliance-feature-switch.md).

## <a name="general-remarks"></a>Notas generales

Los valores de `REPLICATED_SIZE`, `DISTRIBUTED_SIZE` y `LOG_SIZE` pueden ser mayor que, igual que o menor que los valores actuales de la base de datos.

## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones

Las operaciones de crecimiento y reducción son aproximadas. Los tamaños reales resultantes pueden variar con respecto a los parámetros de tamaño.

[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] no ejecuta la instrucción ALTER DATABASE como una operación atómica. Si la instrucción se anula durante la ejecución, se conservarán los cambios que ya se han producido.

La configuración de las estadísticas solo funcionará si el administrador ha habilitado las estadísticas automáticas. Si es un administrador, use el modificador de características [AutoStatsEnabled](../../analytics-platform-system/appliance-feature-switch.md) para habilitar o deshabilitar las estadísticas automáticas.

## <a name="locking-behavior"></a>Comportamiento del bloqueo

Toma un bloqueo compartido en el objeto DATABASE. No se puede modificar una base de datos que está en uso por otro usuario para operaciones lectura o escritura. Esto incluye las sesiones que han emitido una instrucción [USE](../language-elements/use-transact-sql.md) en la base de datos.

## <a name="performance"></a>Rendimiento

Reducir una base de datos puede consumir mucho tiempo y recursos del sistema, en función del tamaño de los datos reales en la base de datos y la cantidad de fragmentación del disco. Por ejemplo, reducir una base de datos puede tardar varias horas o más.

## <a name="determining-encryption-progress"></a>Determinar el progreso del cifrado

Use la consulta siguiente para determinar el progreso del cifrado de datos transparente de base de datos como un porcentaje:

```sql
WITH
database_dek AS (
    SELECT ISNULL(db_map.database_id, dek.database_id) AS database_id,
        dek.encryption_state, dek.percent_complete,
        dek.key_algorithm, dek.key_length, dek.encryptor_thumbprint,
        type
    FROM sys.dm_pdw_nodes_database_encryption_keys AS dek
    INNER JOIN sys.pdw_nodes_pdw_physical_databases AS node_db_map
        ON dek.database_id = node_db_map.database_id
        AND dek.pdw_node_id = node_db_map.pdw_node_id
    LEFT JOIN sys.pdw_database_mappings AS db_map
        ON node_db_map .physical_name = db_map.physical_name
    INNER JOIN sys.dm_pdw_nodes nodes
        ON nodes.pdw_node_id = dek.pdw_node_id
    WHERE dek.encryptor_thumbprint <> 0x
),
dek_percent_complete AS (
    SELECT database_dek.database_id, AVG(database_dek.percent_complete) AS percent_complete
    FROM database_dek
    WHERE type = 'COMPUTE'
    GROUP BY database_dek.database_id
)
SELECT DB_NAME( database_dek.database_id ) AS name,
    database_dek.database_id,
    ISNULL(
       (SELECT TOP 1 dek_encryption_state.encryption_state
        FROM database_dek AS dek_encryption_state
        WHERE dek_encryption_state.database_id = database_dek.database_id
        ORDER BY (CASE encryption_state
            WHEN 3 THEN -1
            ELSE encryption_state
            END) DESC), 0)
        AS encryption_state,
dek_percent_complete.percent_complete,
database_dek.key_algorithm, database_dek.key_length, database_dek.encryptor_thumbprint
FROM database_dek
INNER JOIN dek_percent_complete
    ON dek_percent_complete.database_id = database_dek.database_id
WHERE type = 'CONTROL';
```

Para obtener un ejemplo completo en el que se muestran todos los pasos en la implementación de TDE, vea [Cifrado de datos transparente (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md).

## <a name="examples-sspdw"></a>Ejemplos: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

### <a name="a-altering-the-autogrow-setting"></a>A. Modificación de la configuración de AUTOGROW

Establezca AUTOGROW en ON para la base de datos `CustomerSales`.

```sql
ALTER DATABASE CustomerSales
    SET ( AUTOGROW = ON );
```

### <a name="b-altering-the-maximum-storage-for-replicated-tables"></a>B. Modificación del almacenamiento máximo para las tablas replicadas

En el ejemplo siguiente se establece el límite de almacenamiento de tablas replicadas en 1 GB para la base de datos `CustomerSales`. Este es el límite de almacenamiento por nodo de ejecución.

```sql
ALTER DATABASE CustomerSales
    SET ( REPLICATED_SIZE = 1 GB );
```

### <a name="c-altering-the-maximum-storage-for-distributed-tables"></a>C. Modificación del almacenamiento máximo para las tablas distribuidas

 En el ejemplo siguiente se establece el límite de almacenamiento de distribuidas replicadas en 1000 GB (un terabyte) para la base de datos `CustomerSales`. Este es el límite de almacenamiento combinado en todo el dispositivo para todos los nodos de ejecución, no el límite de almacenamiento por nodo de ejecución.

```sql
ALTER DATABASE CustomerSales
    SET ( DISTRIBUTED_SIZE = 1000 GB );
```

### <a name="d-altering-the-maximum-storage-for-the-transaction-log"></a>D. Modificación del almacenamiento máximo para el registro de transacciones

 En el ejemplo siguiente se actualiza la base de datos `CustomerSales` para que tenga un tamaño máximo de registro de transacciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de 10 GB para el dispositivo.

```sql
ALTER DATABASE CustomerSales
    SET ( LOG_SIZE = 10 GB );
```

### <a name="e-check-for-current-statistics-values"></a>E. Comprobación de los valores de estadísticas actuales

La consulta siguiente devuelve los valores actuales de las estadísticas para todas las bases de datos. El valor 1 significa que la característica está activada, y 0 que está desactivada.

```sql
SELECT NAME,
    is_auto_create_stats_on,
    is_auto_update_stats_on,
    is_auto_update_stats_async_on
FROM sys.databases;
```

### <a name="f-enable-auto-create-and-auto-update-stats-for-a-database"></a>F. Habilitación de estadísticas de creación y actualización automáticas para una base de datos

Use la instrucción siguiente para habilitar las estadísticas de creación y actualización de manera automática y asincrónica para la base de datos, CustomerSales. De esta forma, se crean y actualizan las estadísticas de columna única según sea necesario para crear planes de consulta de alta calidad.

```sql
ALTER DATABASE CustomerSales
    SET AUTO_CREATE_STATISTICS ON;
ALTER DATABASE CustomerSales
    SET AUTO_UPDATE_STATISTICS ON;
ALTER DATABASE
    SET AUTO_UPDATE_STATISTICS_ASYNC ON;
```

## <a name="see-also"></a>Consulte también

- [CREATE DATABASE - Analytics Platform System](../../t-sql/statements/create-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)

::: moniker-end
