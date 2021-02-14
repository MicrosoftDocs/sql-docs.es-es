---
description: Extended Events for Stretch Database (Eventos extendidos de Stretch Database)
title: Extended Events for Stretch Database (Eventos extendidos de Stretch Database)
ms.date: 06/14/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 70485e74-2e25-4e7e-be6c-9dd1780a42e3
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: f965b68279398720ff7b1b9bcb76b5ec73410466
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080086"
---
# <a name="extended-events-for-stretch-database"></a>Extended Events for Stretch Database (Eventos extendidos de Stretch Database)
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


Stretch Database proporciona un conjunto de eventos extendidos para solucionar problemas.  
  
Para obtener más información, vea [Eventos extendidos](../../relational-databases/extended-events/extended-events.md). Para obtener información sobre cómo iniciar una sesión de eventos extendidos para solucionar problemas, vea [Crear una sesión de eventos extendidos](/previous-versions/sql/sql-server-2016/hh213147(v=sql.130)).  
  
## <a name="list-of-extended-events-for-stretch-database"></a>Lista de eventos extendidos de Stretch Database  
  
Nombre del evento|Descripción del evento   
---------|---------  
remote_data_archive_db_ddl|Se produce cuando se procesa el archivo ddl de T-SQL de base de datos para ampliar los datos.  
remote_data_archive_provision_operation|Se produce cuando se inicia o se finaliza una operación de aprovisionamiento.  
remote_data_archive_query_rewrite|Se produce cuando RelOp_Get se reemplaza durante la reescritura de la consulta para el ajuste.  
remote_data_archive_table_ddl|Se produce cuando se procesa el archivo ddl de T-SQL de tabla para ampliar los datos.  
remote_data_archive_telemetry|Se produce siempre que un sistema local transmite un evento de telemetría a la Base de datos de Azure.  
remote_data_archive_telemetry_rejected|Se produce cuando se rechaza un evento de telemetría de ajuste de base de datos de Azure  
repopulate_stretch_schema_task_queue_complete|Informa de la finalización de la repoblación de la cola de la tarea del esquema de Stretch.  
repopulate_stretch_schema_task_queue_start|Informa del inicio del rellenado de la cola de la tarea del esquema de Stretch.  
stretch_codegen_errorlog|Informa de la salida del generador de código  
stretch_codegen_start|Informa del inicio de la generación de código de ajuste  
stretch_create_remote_table_start|Informa del inicio de la creación de la tabla remota  
stretch_database_disable_completed|Informa de la finalización de un comando ALTER DATABASE SET REMOTE_DATA_ARCHIVE OFF  
stretch_database_enable_completed|Informa de la finalización de un comando ALTER DATABASE SET REMOTE_DATA_ARCHIVE ON  
stretch_database_reauthorize_completed|Informa de la finalización de un procedimiento de especificación sp_rda_reauthorize_db  
stretch_index_reconciliation_codegen_completed|Informa de la finalización de la generación de código para la operación de ajuste del índice remoto  
stretch_index_update_step_completed|Informa de la duración de una operación de actualización para un índice ajustado  
stretch_migration_debug_trace|Seguimiento de depuración de acciones de migración de ajuste.  
stretch_migration_dequeue_migration|Evento generado cuando se quita de la cola una tarea de migración de extensión.  
stretch_migration_queue_migration|Poner en cola un paquete para iniciar la migración de base de datos y objeto.  
stretch_migration_requeue_migration|Evento generado cuando se vuelve a poner en cola un paquete de tareas de migración de extensión.  
stretch_migration_start_migration|Iniciar migración de la base de datos y el objeto.  
stretch_migration_start_unmigration|Empezar a deshacer la migración de la base de datos y el objeto.  
stretch_remote_column_execution_completed|Informa sobre la finalización de la ejecución remota del código generado para una columna extendida  
stretch_remote_column_reconciliation_codegen_completed|Informa sobre la finalización de la generación de código para la conciliación de ajuste de columnas remotas  
stretch_remote_index_execution_completed|Informa de la finalización de la ejecución remota para el código generado para un índice ajustado  
stretch_schema_queue_task|Informa cuando un paquete está a punto de ponerse en cola para procesar una tarea de esquema para la base de datos y el objeto.  
stretch_schema_script_execution_completed|Informa de la finalización de la ejecución del script de Stretch durante el procesamiento de una tarea de esquema de Stretch.  
stretch_schema_script_execution_skipped|Informa de la omisión de la ejecución del script de Stretch durante el procesamiento de la tarea de esquema de Stretch.  
stretch_schema_script_execution_start|Informa del inicio de la ejecución del script de Stretch durante el procesamiento de la tarea de esquema de Stretch.  
stretch_schema_task_failed|Informa del error de una función de esquema de Stretch durante la tarea de esquema de Stretch.  
stretch_schema_task_skipped|Notifica que la tarea de esquema Stretch se omitió durante la función del esquema Stretch.  
stretch_schema_task_start|Informa del inicio de la función de ajuste del esquema durante la tarea del esquema Stretch.  
stretch_schema_task_succeeded|Informa de la correcta finalización de la función de esquema Stretch durante la tarea de esquema de Stretch.  
stretch_sp_migration_get_batch_id|Llamar a sp_stretch_get_batch_id  
stretch_sync_metadata_start|Informa del inicio de las comprobaciones de metadatos durante la tarea de migración.  
stretch_table_codegen_completed|Informa de la finalización de la generación de código para una tabla ajustada  
stretch_table_complete_data_reconciliation|Completa la conciliación de datos de la base de datos y el objeto.  
stretch_table_data_reconciliation_event|Informa sobre la finalización de la conciliación de los datos de un lote de filas  
stretch_table_data_reconciliation_results_event|Informa sobre un error o la finalización correcta de la conciliación de datos de un número de lotes de filas  
stretch_table_hinted_admin_delete_event|Informa sobre la ejecución de una operación DML de eliminación Stretch que usa una sugerencia de administración  
stretch_table_hinted_admin_update_event|Informa sobre la ejecución de una operación DML de actualización Stretch que usa una sugerencia de administración  
stretch_table_provisioning_step_completed|Informa de la duración de una operación de aprovisionamiento de tabla ajustada  
stretch_table_query_error|Informa de un error producido al volver a escribir la consulta Stretch  
stretch_table_remote_creation_completed|Informa de la finalización de la ejecución remota para el código generado de una tabla ajustada  
stretch_table_row_migration_event|Informa de la finalización de la migración de un lote de filas  
stretch_table_row_migration_results_event|Informa de un error o finalización de una migración correcta de un número de lotes de filas  
stretch_table_row_unmigration_event|Informa sobre la finalización de la no migración de un lote de filas  
stretch_table_row_unmigration_results_event|Informa de un error o de la finalización correcta al deshacer la migración de un número de lotes de filas  
stretch_table_start_data_reconciliation|Comenzar la conciliación de datos de la base de datos y el objeto.  
stretch_table_unprovision_completed|Informa de la eliminación de la finalización de recursos locales para una tabla en la que se anuló el ajuste  
stretch_table_validation_error|Informa de la finalización de la validación para una tabla cuando el usuario habilita el ajuste  
stretch_unprovision_table_start|Informa del inicio de la anulación del aprovisionamiento de la tabla de ajuste  
  
## <a name="see-also"></a>Consulte también  
[Administración y solución de problemas de Stretch Database](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md)