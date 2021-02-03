---
description: MSSQLSERVER_3023
title: MSSQLSERVER_3023
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3023 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 096d1fd1e736ea731a8fcf72d36507cd64efae1d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198151"
---
# <a name="mssqlserver_3023"></a>MSSQLSERVER_3023
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Detalles

|Atributo|Value|
|---|---|
|Nombre de producto|SQL Server|
|Id. de evento|3023|
|Origen de eventos|MSSQLSERVER|
|Componente|SQLEngine|
|Nombre simbólico|DB_IN_USE_DUMP|
|Texto del mensaje|Las operaciones de copia de seguridad y manipulación de archivos (como ALTER DATABASE y ADD FILE) deben hacerse en serie. Vuelva a emitir la instrucción al completar la operación actual de copia de seguridad o manipulación de archivos.|
||

## <a name="explanation"></a>Explicación

Intenta ejecutar un comando BACKUP, SHRINK o ALTER DATABASE en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y encuentra los siguientes mensajes:

> Mensaje 3023, nivel 16, estado 2, línea 1  
Las operaciones de copia de seguridad y manipulación de archivos (como ALTER DATABASE y ADD FILE) deben hacerse en serie. Vuelva a emitir la instrucción al completar la operación actual de copia de seguridad o manipulación de archivos.

> Mensaje 3013, nivel 16, estado 1, línea 1  
Fin anómalo de BACKUP DATABASE.

Además, el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contiene mensajes como estos:

> \<Datetime> Copia de seguridad Error: 3041, Gravedad: 16, estado: 1.  
\<Datetime> Copia de seguridad BACKUP no pudo completar el comando BACKUP DATABASE MyDatabase WITH DIFFERENTIAL. Compruebe el registro de la aplicación de copia de seguridad para ver los mensajes detallados.

También puede observar que estos comandos encuentran los elementos `wait_type = LCK_M_U` y `wait_resource = DATABASE: <id> [BULKOP_BACKUP_DB]` cuando se consulta el estado de los comandos desde las diversas vistas de administración dinámica (DMV), como `sys.dm_exec_requests` o `sys.dm_os_waiting_tasks`.

## <a name="possible-causes"></a>Causas posibles

Hay varias reglas en las que se permiten o no las operaciones cuando se ejecuta una copia de seguridad de base de datos completa en una base de datos. Algunos ejemplos son los siguientes:

- Solo se puede realizar una copia de seguridad de datos cada vez (cuando se produce una copia de seguridad completa de la base de datos, no se pueden realizar al mismo tiempo copias de seguridad diferenciales o incrementales).
- Solo se puede realizar la copia de seguridad de un registro cada vez (se permite una copia de seguridad de registros cuando se está haciendo una copia de seguridad completa de la base de datos).
- No se pueden agregar o quitar archivos en una base de datos mientras se está realizando una copia de seguridad.
- No se pueden reducir los archivos mientras se están realizando copias de seguridad de la base de datos.
- Se permiten cambios limitados en el modelo de recuperación mientras se hacen las copias de seguridad.

Cuando se lleva a cabo alguna de estas operaciones en conflicto, los comandos se encuentran con las esperas de bloqueo que se mencionan en la sección de explicación, y después se reciben los mensajes 3023 y 3041.

## <a name="user-action"></a>Acción del usuario

Examine las programaciones de las diversas actividades de mantenimiento de bases de datos y, después, ajústelas para que las operaciones o los comandos no entren en conflicto entre sí.

## <a name="more-information"></a>Más información

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] registra la hora de inicio y la hora de finalización de la copia de seguridad en la base de datos `msdb`. Puede examinar el historial de copias de seguridad para determinar si se ha producido una copia de seguridad completa de la base de datos mientras se intentaba realizar una copia de seguridad incremental y por eso se ha producido el error. Puede usar la siguiente consulta para llevar a cabo este proceso:

```sql
select database_name, type, backup_start_date, backup_finish_date
from msdb.dbo.backupset
order by database_name, type, backup_start_date, backup_finish_date
go
```

También puede usar el evento **User Error Message** en un archivo de seguimiento de SQL Server Profiler o el evento **error_reported** en los eventos extendidos para realizar el seguimiento de los informes de los mensajes 3023 y determinar la aplicación que inició la copia de seguridad u otro comando de mantenimiento.
