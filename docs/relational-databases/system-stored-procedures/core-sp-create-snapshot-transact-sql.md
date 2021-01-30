---
description: core.sp_create_snapshot (Transact-SQL)
title: core.sp_create_snapshot (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_create_snapshot
- sp_create_snapshot_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- management data warehouse, data collector stored procedures
- data collector [SQL Server], stored procedures
- core.sp_create_snapshot stored procedure
- sp_create_snapshot
ms.assetid: ff297bda-0ee2-4fda-91c8-7000377775e3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 737739cfa627e6668d95e6453d66ed1bad4ad637
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210505"
---
# <a name="coresp_create_snapshot-transact-sql"></a>core.sp_create_snapshot (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Inserta una fila en una vista core.snapshots del almacén de administración de datos. Se llama a este procedimiento cada vez que un paquete de carga empieza a cargar los datos en el almacén de administración de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
core.sp_create_snapshot [ @collection_set_uid = ] 'collection_set_uid'  
    , [ @collector_type_uid = ] 'collector_type_uid'  
    ,[ @machine_name = ] 'machine_name'  
    , [ @named_instance = ] 'named_instance'  
    , [ @log_id = ] log_id  
    , [ @snapshot_id = ] snapshot_id OUTPUT  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @collection_set_uid =] '*collection_set_uid*'  
 GUID del conjunto de recopilación. *collection_set_uid* es de tipo **uniqueidentifier** y no tiene ningún valor predeterminado. Para obtener el GUID, consulte la vista dbo.syscollector_collection_sets en la base de datos msdb.  
  
 [ @collector_type_uid =] '*collector_type_uid*'  
 El GUID de un tipo de recopilador. *collector_type_uid* es de tipo **uniqueidentifier** y no tiene ningún valor predeterminado. Para obtener el GUID, consulte la vista dbo.syscollector_collector_types en la base de datos msdb.  
  
 [ @machine_name =] '*machine_name*'  
 Nombre del servidor en el que reside el conjunto de recopilación. *machine_name* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
 [ @named_instance =] '*named_instance*'  
 Nombre de la instancia del conjunto de recopilación. *named_instance* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
 [ @log_id =] *log_id*  
 Identificador único que se asigna al registro de eventos de conjunto de recopilación en el servidor que recopiló los datos. *log_id* es de tipo **BIGINT** y no tiene ningún valor predeterminado. Para obtener el valor de *log_id*, consulte la vista collector_execution_log dbo.sysen la base de datos msdb.  
  
 [ @snapshot_id =] *snapshot_id*  
 Identificador único de una fila que se inserta en la vista Core. snapshots. *snapshot_id* es de **tipo int** y se devuelve como salida.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 Cada vez que un paquete de carga inicia la carga de datos en el almacén de administración de datos, el componente en tiempo de ejecución del recopilador de datos llama a core.sp_create_snapshot.  
  
 Este procedimiento comprueba si:  
  
-   collection_set_uid coincide con una entrada existente en la tabla core.source_info_internal.  
  
-   collector_type_uid coincide con una entrada existente en la vista core.supported_collector_types.  
  
 Si alguna de las comprobaciones anteriores no es correcta, se produce un error en el procedimiento y se devuelve un error.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de base de datos **mdw_writer** (con permiso Execute).  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se crea una instantánea para el conjunto de recopilación Uso de disco, se agrega al almacén de administración de datos y se devuelve al identificador de la instantánea. En este ejemplo se usa la instancia predeterminada.  
  
```  
USE <management_data_warehouse>;  
DECLARE @snapshot_id int;  
EXEC core.sp_create_snapshot   
    @collection_set_uid = '7B191952-8ECF-4E12-AEB2-EF646EF79FEF',   
    @collector_type_uid = '302E93D1-3424-4BE7-AA8E-84813ECF2419',  
    @machine_name = '<computername>',  
    @named_instance = 'MSSQLSERVER',  
    @log_id = 11, -- ID of the log for the collection set  
    @snapshot_id = @snapshot_id OUTPUT;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados del recopilador de datos &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [Almacén de administración de datos](../../relational-databases/data-collection/management-data-warehouse.md)  
  
  
