---
description: sys.master_files (Transact-SQL)
title: sys.master_files (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/10/2016
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.master_files
- master_files_TSQL
- sys.master_files_TSQL
- master_files
dev_langs:
- TSQL
helpviewer_keywords:
- sys.master_files catalog view
ms.assetid: 803b22f2-0016-436b-a561-ce6f023d6b6a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1cfe105e765fbdafc1c4e61e0c340b3db44ed095
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191362"
---
# <a name="sysmaster_files-transact-sql"></a>sys.master_files (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Contiene una fila por archivo de una base de datos almacenada en la base de datos maestra. Es una vista única de todo el sistema.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|database_id|**int**|Id. de la base de datos a la que se refiere este archivo. El masterdatabase_id es siempre 1.|  
|file_id|**int**|Identificador del archivo dentro de la base de datos. El valor de file_id principal siempre es 1.|  
|file_guid|**uniqueidentifier**|Identificador único del archivo.<br /><br /> NULL = la base de datos se actualizó desde una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (válida para SQL Server 2005 y versiones anteriores).|  
|tipo|**tinyint**|Tipo de archivo:<br /><br /> 0 = Filas.<br /><br /> 1 = Registro<br /><br /> 2 = FILESTREAM<br /><br /> 3 = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> 4 = Texto completo (catálogos de texto completo anteriores a [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]; los catálogos de texto completo actualizados o creados en [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] o versiones posteriores notificarán un tipo de archivo 0).|  
|type_desc|**nvarchar(60)**|Descripción del tipo de archivo:<br /><br /> ROWS<br /><br /> REGISTRO<br /><br /> FILESTREAM<br /><br /> FULLTEXT (catálogos de texto completo anteriores a [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]).|  
|data_space_id|**int**|Id. del espacio de datos al que pertenece este archivo. El espacio de datos es un grupo de archivos.<br /><br /> 0 = Archivos de registro|  
|name|**sysname**|Nombre lógico del archivo de la base de datos.|  
|physical_name|**nvarchar(260)**|Nombre del archivo del sistema operativo.|  
|state|**tinyint**|Estado del archivo:<br /><br /> 0 = Con conexión <br /><br /> 1 = En restauración <br /><br /> 2 = En recuperación <br /><br /> 3 = RECOVERY_PENDING<br /><br /> 4 = Sospechoso <br /><br /> 5 = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> 6 = Sin conexión <br /><br /> 7 = Inactivo|  
|state_desc|**nvarchar(60)**|Descripción del estado del archivo:<br /><br /> ONLINE<br /><br /> RESTORING<br /><br /> RECOVERING<br /><br /> RECOVERY_PENDING <br /><br /> SUSPECT<br /><br /> OFFLINE<br /><br /> DEFUNCT<br /><br /> Para más información, vea [Estados de los archivos](../../relational-databases/databases/file-states.md).|  
|tamaño|**int**|Tamaño actual del archivo, en páginas de 8 KB. En una instantánea de base de datos, size refleja el espacio máximo que la instantánea puede utilizar para el archivo.<br /><br /> Nota: este campo se rellena como cero para los contenedores de FILESTREAM. Consulte la vista de catálogo *Sys.database_files* para ver el tamaño real de los contenedores de FileStream.|  
|max_size|**int**|Tamaño máximo del archivo, en páginas de 8 KB:<br /><br /> 0 = No se permite el crecimiento.<br /><br /> -1 = El archivo crece hasta que el disco esté lleno.<br /><br /> 268435456 = El archivo de registro aumentará de tamaño hasta un tamaño máximo de 2 TB.<br /><br /> Nota: las bases de datos que se actualizan con un tamaño de archivo de registro ilimitado informarán de-1 para el tamaño máximo del archivo de registro.|  
|growth|**int**|0 = El archivo tiene un tamaño fijo y no puede crecer.<br /><br /> >0 = el archivo aumentará automáticamente.<br /><br /> Si is_percent_growth = 0, el incremento de tamaño se realiza en unidades de páginas de 8-KB, redondeado a los 64 KB más próximos.<br /><br /> Si is_percent_growth = 1, el aumento de crecimiento se expresa como un porcentaje numérico entero.|  
|is_media_read_onlyF|**bit**|1 = El archivo está en medios de solo lectura.<br /><br /> 0 = El archivo está en medios de lectura/escritura.|  
|is_read_only|**bit**|1 = El archivo está marcado como de solo lectura.<br /><br /> 0 = El archivo está marcado como de lectura/escritura.|  
|is_sparse|**bit**|1 = El archivo es un archivo disperso.<br /><br /> 0 = El archivo no es un archivo disperso.<br /><br /> Para obtener más información, vea [Ver el tamaño del archivo disperso de una instantánea de base de datos &#40;Transact-SQL&#41;](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md).|  
|is_percent_growth|**bit**|1 = El crecimiento del archivo es un porcentaje.<br /><br /> 0 = Tamaño absoluto del crecimiento en páginas.|  
|is_name_reserved|**bit**|1 = El nombre de archivo quitado se puede volver a utilizar. Se debe obtener una copia de seguridad del registro para poder reutilizar el nombre (name o physical_name) para un archivo nuevo.<br /><br /> 0 = El nombre de archivo no se puede reutilizar.|  
|create_lsn|**numeric(25,0)**|Número de flujo de registro (LSN) en el que se creó el archivo.|  
|drop_lsn|**numeric(25,0)**|LSN en el que se quitó el archivo.|  
|read_only_lsn|**numeric(25,0)**|LSN en el que el grupo de archivos que contiene el archivo cambió de lectura/escritura a solo lectura (el cambio más reciente).|  
|read_write_lsn|**numeric(25,0)**|LSN en el que el grupo de archivos que contiene el archivo cambió de solo lectura a lectura/escritura (el cambio más reciente).|  
|differential_base_lsn|**numeric(25,0)**|Base para copias de seguridad diferenciales. Las extensiones de datos cambiadas después de este LSN se incluirán en una copia de seguridad diferencial.|  
|differential_base_guid|**uniqueidentifier**|Identificador único de la copia de seguridad de base en la que se basará una copia de seguridad diferencial.|  
|differential_base_time|**datetime**|Hora que corresponde a differential_base_lsn.|  
|redo_start_lsn|**numeric(25,0)**|LSN en el que debe comenzar la siguiente puesta al día.<br /><br /> Es NULL a menos que state = RESTORING o state = RECOVERY_PENDING.|  
|redo_start_fork_guid|**uniqueidentifier**|Identificador exclusivo de la bifurcación de recuperación. El first_fork_guid de la siguiente copia de seguridad de registros restaurada debe coincidir con este valor. Representa el estado actual del contenedor.|  
|redo_target_lsn|**numeric(25,0)**|LSN en el que se puede detener la puesta al día en línea de este archivo.<br /><br /> Es NULL a menos que state = RESTORING o state = RECOVERY_PENDING.|  
|redo_target_fork_guid|**uniqueidentifier**|La bifurcación de recuperación en la que se puede recuperar el contenedor. Se empareja con redo_target_lsn.|  
|backup_lsn|**numeric(25,0)**|El LSN de los datos más recientes o de la copia de seguridad diferencial del archivo.|  
|credential_id|**int**|De que se `credential_id` `sys.credentials` utiliza para almacenar el archivo. Por ejemplo, cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecuta en una máquina virtual de Azure y los archivos de base de datos se almacenan en Azure BLOB Storage, se configura una credencial con las credenciales de acceso a la ubicación de almacenamiento.|  
  
> [!NOTE]  
>  Al quitar o volver a generar índices grandes, o al quitar o truncar tablas grandes, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] difiere las cancelaciones de asignación de páginas, así como sus bloqueos asociados, hasta que se confirma la transacción. Las operaciones de eliminación diferidas no liberan inmediatamente el espacio asignado. Por lo tanto, es posible que los valores devueltos por sys.master_files inmediatamente después de quitar o truncar un objeto grande no reflejen el espacio en disco disponible real.  
  
## <a name="permissions"></a>Permisos  
 Los permisos mínimos necesarios para ver la fila correspondiente son CREATE DATABASE, ALTER ANY DATABASE o VIEW ANY DEFINITION.  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo de archivos y bases de datos &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)   
 [Estados de archivo](../../relational-databases/databases/file-states.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [Archivos y grupos de archivos de base de datos](../../relational-databases/databases/database-files-and-filegroups.md)  
  
  
