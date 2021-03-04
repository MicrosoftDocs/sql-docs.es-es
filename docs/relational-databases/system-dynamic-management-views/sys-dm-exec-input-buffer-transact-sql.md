---
description: sys.dm_exec_input_buffer (Transact-SQL)
title: sys.dm_exec_input_buffer (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_exec_input_buffer
- sys.dm_exec_input_buffer _tsql
- dm_exec_input_buffer
- dm_exec_input_buffer_tsql
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_input_buffer dynamic management function
ms.assetid: fb34a560-bde9-4ad9-aa96-0d4baa4fc104
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0d6b6cd5081d70f22fb2be2edd3355119c987e07
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839353"
---
# <a name="sysdm_exec_input_buffer-transact-sql"></a>sys.dm_exec_input_buffer (Transact-SQL)

[!INCLUDE[tsql-appliesto-2014sp2-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-2014sp2-asdb-xxxx-xxx-md.md)]

Devuelve información sobre las instrucciones enviadas a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

## <a name="syntax"></a>Sintaxis

```
sys.dm_exec_input_buffer ( session_id , request_id )
```

## <a name="arguments"></a>Argumentos

*session_id* Es el identificador de sesión que ejecuta el lote que se va a buscar. *session_id* es **smallint**. *session_id* se pueden obtener de los siguientes objetos de administración dinámica:

- [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)
- [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)
- [sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md)

*request_id* Request_id de [Sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md). *request_id* es de **tipo int**.

## <a name="table-returned"></a>Tabla devuelta

|Nombre de la columna|Tipo de datos|Descripción|
|-----------------|---------------|-----------------|
|**event_type**|**nvarchar(256)**|El tipo de evento en el búfer de entrada para el SPID dado.|
|**parameters**|**smallint**|Los parámetros proporcionados para la instrucción.|
|**event_info**|**nvarchar(max)**|Texto de la instrucción en el búfer de entrada para el SPID dado.|

## <a name="permissions"></a>Permisos

En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , si el usuario tiene el permiso View Server State, el usuario verá todas las sesiones en ejecución en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ; de lo contrario, el usuario solo verá la sesión actual.

> [!IMPORTANT]
> La ejecución de esta DMV fuera de SQL Server Management Studio contra SQL Server sin permisos VIEW SERVER STATE (como en un desencadenador, un procedimiento almacenado o una función) produce un error de permiso en la base de datos maestra.

En [!INCLUDE[ssSDS](../../includes/sssds-md.md)] , si el usuario es el propietario de la base de datos, el usuario verá todas las sesiones en ejecución en el [!INCLUDE[ssSDS](../../includes/sssds-md.md)] ; de lo contrario, el usuario solo verá la sesión actual.

> [!IMPORTANT]
> La ejecución de esta DMV fuera de SQL Server Management Studio contra Azure SQL Database sin permisos de propietario (como en un desencadenador, un procedimiento almacenado o una función) produce un error de permiso en la base de datos maestra.

## <a name="remarks"></a>Observaciones

Esta función de administración dinámica se puede usar junto con sys.dm_exec_sessions o sys.dm_exec_requests mediante el uso de **Cross Apply**.

## <a name="examples"></a>Ejemplos

### <a name="a-simple-example"></a>A. Ejemplo sencillo

En el ejemplo siguiente se muestra cómo pasar un identificador de sesión (SPID) y un identificador de solicitud a la función.

```sql
SELECT * FROM sys.dm_exec_input_buffer (52, 0);
GO
```

### <a name="b-using-cross-apply-to-additional-information"></a>B. Usar Cross Apply para obtener información adicional

En el ejemplo siguiente se muestra el búfer de entrada para las sesiones de usuario.

```sql
SELECT es.session_id, ib.event_info
FROM sys.dm_exec_sessions AS es
CROSS APPLY sys.dm_exec_input_buffer(es.session_id, NULL) AS ib
WHERE es.is_user_process = 1;
GO
```

## <a name="see-also"></a>Consulte también

- [Funciones y vistas de administración dinámica relacionadas con ejecuciones &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)
- [sys.dm_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)
- [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)
- [DBCC INPUTBUFFER &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-inputbuffer-transact-sql.md)
