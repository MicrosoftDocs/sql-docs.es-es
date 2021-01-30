---
description: sys.database_files (Transact-SQL)
title: sys.database_files (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/19/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.database_files
- sys.database_files_TSQL
- database_files
- database_files_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_files catalog view
ms.assetid: 0f5b0aac-c17d-4e99-b8f7-d04efc9edf44
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6d58532ca43e18ae45828b9d647857e629cd7658
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195913"
---
# <a name="sysdatabase_files-transact-sql"></a>sys.database_files (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contiene una fila por cada archivo de una base de datos como se almacena en la propia base de datos. Es una vista por base de datos.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**file_id**|**int**|Identificador del archivo dentro de la base de datos.|  
|**file_guid**|**uniqueidentifier**|GUID del archivo.<br /><br /> NULL = la base de datos se actualizó desde una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (válida para SQL Server 2005 y versiones anteriores).|  
|**type**|**tinyint**|Tipo de archivo:<br/><br /> 0 = filas<br /><br/> 1 = Registro<br/><br /> 2 = FILESTREAM<br /><br /> 3 = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> 4 = texto completo|  
|**type_desc**|**nvarchar(60)**|Descripción del tipo de archivo:<br /><br /> ROWS <br /><br /> REGISTRO<br /><br /> FILESTREAM<br /><br /> FULLTEXT|  
|**data_space_id**|**int**|El valor puede ser 0 o mayor que 0. El valor 0 representa el archivo de registro de base de datos y un valor mayor que 0, el identificador del grupo de archivos donde está almacenado este archivo de datos.|  
|**name**|**sysname**|Nombre lógico del archivo de la base de datos.|  
|**physical_name**|**nvarchar(260)**|Nombre del archivo del sistema operativo. Si la base de datos está hospedada en una [réplica secundaria legible](../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)de AlwaysOn, **physical_name** indica la ubicación del archivo de la base de datos de réplica principal. Para la ubicación de archivo correcta de una base de datos secundaria legible, consulte [sys.sysaltfiles](../../relational-databases/system-compatibility-views/sys-sysaltfiles-transact-sql.md).|  
|**state**|**tinyint**|Estado del archivo:<br /><br /> 0 = Con conexión <br /><br /> 1 = En restauración <br /><br /> 2 = En recuperación <br /><br /> 3 = RECOVERY_PENDING<br /><br /> 4 = Sospechoso <br /><br /> 5 = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> 6 = Sin conexión <br /><br /> 7 = Inactivo|  
|**state_desc**|**nvarchar(60)**|Descripción del estado del archivo:<br /><br /> ONLINE<br /><br /> RESTORING<br /><br /> RECOVERING<br /><br /> RECOVERY_PENDING <br /><br /> SUSPECT<br /><br /> OFFLINE<br /><br /> DEFUNCT<br /><br /> Para más información, vea [Estados de los archivos](../../relational-databases/databases/file-states.md).|  
|**size**|**int**|Tamaño actual de archivo, en páginas de 8 KB.<br /><br /> 0 = No aplicable.<br /><br /> En una instantánea de base de datos, size refleja el espacio máximo que la instantánea puede utilizar para el archivo.<br /><br /> En los contenedores de grupos de archivos FILESTREAM, size refleja el tamaño actual del contenedor.|  
|**max_size**|**int**|Tamaño máximo del archivo, en páginas de 8 KB:<br /><br /> 0 = No se permite el crecimiento.<br /><br /> -1 = El archivo crece hasta que el disco esté lleno.<br /><br /> 268435456 = El archivo de registro aumentará de tamaño hasta un tamaño máximo de 2 TB.<br /><br /> En los contenedores de grupos de archivos FILESTREAM, max_size refleja el tamaño máximo del contenedor.<br /><br /> Tenga en cuenta que las bases de datos que se actualizan con un tamaño de archivo de registro ilimitado informarán de-1 para el tamaño máximo del archivo de registro.|  
|**crezca**|**int**|0 = El archivo tiene un tamaño fijo y no puede crecer.<br /><br /> >0 = el archivo aumentará automáticamente.<br /><br /> Si is_percent_growth = 0, el aumento de crecimiento es en unidades de páginas de 8 KB, redondeado a los 64 KB más próximos.<br /><br /> Si is_percent_growth = 1, el aumento de crecimiento se expresa como un porcentaje numérico entero.|  
|**is_media_read_only**|**bit**|1 = El archivo está en medios de solo lectura.<br /><br /> 0 = El archivo está en un medio de lectura y escritura.|  
|**is_read_only**|**bit**|1 = El archivo está marcado como de solo lectura.<br /><br /> 0 = El archivo está marcado como de lectura y escritura.|  
|**is_sparse**|**bit**|1 = El archivo es un archivo disperso.<br /><br /> 0 = El archivo no es un archivo disperso.<br /><br /> Para obtener más información, vea [Ver el tamaño del archivo disperso de una instantánea de base de datos &#40;Transact-SQL&#41;](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md).|  
|**is_percent_growth**|**bit**|1 = El crecimiento del archivo es un porcentaje.<br /><br /> 0 = Tamaño absoluto del crecimiento en páginas.|  
|**is_name_reserved**|**bit**|1 = El nombre del archivo quitado (name o physical_name) solo se podrá volver a utilizar después de la siguiente copia de seguridad del registro. Si se quitan archivos de una base de datos, sus nombres lógicos permanecen en estado de reserva hasta la siguiente copia de seguridad de registros. Esta columna solo es relevante en el modelo de restauración completa y en el modelo de recuperación optimizado para cargas masivas de registros.|  
|**create_lsn**|**numeric(25,0)**|Número de flujo de registro (LSN) en el que se creó el archivo.|  
|**drop_lsn**|**numeric(25,0)**|LSN en el que se quitó el archivo.<br /><br /> 0 = El nombre de archivo no se puede volver a utilizar.|  
|**read_only_lsn**|**numeric(25,0)**|LSN en el que el grupo de archivos que contiene el archivo cambió de lectura/escritura a solo lectura (el cambio más reciente).|  
|**read_write_lsn**|**numeric(25,0)**|LSN en el que el grupo de archivos que contiene el archivo cambió de solo lectura a lectura/escritura (el cambio más reciente).|  
|**differential_base_lsn**|**numeric(25,0)**|Base para copias de seguridad diferenciales. Las extensiones de datos cambiadas después de este LSN se incluirán en una copia de seguridad diferencial.|  
|**differential_base_guid**|**uniqueidentifier**|Identificador único de la copia de seguridad de base en la que se basará una copia de seguridad diferencial.|  
|**differential_base_time**|**datetime**|Hora que corresponde a differential_base_lsn.|  
|**redo_start_lsn**|**numeric(25,0)**|LSN en el que debe comenzar la siguiente puesta al día.<br /><br /> Es NULL a menos que state = RESTORING o state = RECOVERY_PENDING.|  
|**redo_start_fork_guid**|**uniqueidentifier**|Identificador exclusivo de la bifurcación de recuperación. El first_fork_guid de la siguiente copia de seguridad de registros restaurada debe coincidir con este valor. Representa el estado actual del archivo.|  
|**redo_target_lsn**|**numeric(25,0)**|LSN en el que se puede detener la puesta al día en línea de este archivo.<br /><br /> Es NULL a menos que state = RESTORING o state = RECOVERY_PENDING.|  
|**redo_target_fork_guid**|**uniqueidentifier**|Bifurcación de recuperación en que se puede recuperar el archivo. Se empareja con redo_target_lsn.|  
|**backup_lsn**|**numeric(25,0)**|El LSN de los datos más recientes o de la copia de seguridad diferencial del archivo.|  
  
> [!NOTE]  
>  Al quitar o volver a generar índices grandes, o al quitar o truncar tablas grandes, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] difiere las cancelaciones de asignación de páginas, así como sus bloqueos asociados, hasta que se confirma la transacción. Las operaciones de eliminación diferidas no liberan inmediatamente el espacio asignado. Por tanto, los valores devueltos por sys.database_files inmediatamente después de quitar o truncar un objeto grande pueden no reflejar el espacio de disco real disponible.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** . Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  

## <a name="examples"></a>Ejemplos  
La siguiente instrucción devuelve el nombre, el tamaño de archivo y la cantidad de espacio vacío para cada archivo de base de datos.

```
SELECT name, size/128.0 FileSizeInMB,
size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0 
   AS EmptySpaceInMB
FROM sys.database_files;
```
Para obtener más información sobre cómo usar [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] , vea [determinar el tamaño de la base de datos en Azure SQL Database V12](/archive/blogs/sqlcat/determining-database-size-in-azure-sql-database-v12) en el blog del equipo de asesoría de clientes de SQL.
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo de archivos y bases de datos &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)   
 [Estados de archivo](../../relational-databases/databases/file-states.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.master_files &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)   
 [Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md)   
 [sys.data_spaces &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)  
  
