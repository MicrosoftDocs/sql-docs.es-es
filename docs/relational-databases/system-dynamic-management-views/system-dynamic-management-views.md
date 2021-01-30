---
description: Vistas de administración dinámica (Transact-SQL)
title: Vistas de administración dinámica (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- database scoped dynamic management objects [SQL Server]
- dynamic management objects [SQL Server], about dynamic management objects
- server state information [SQL Server]
- dynamic management functions [SQL Server]
- metadata [SQL Server], dynamic management objects
- dynamic management views [SQL Server]
- DMVs [SQL Server]
- functions [SQL Server], dynamic management
- server scoped dynamic management objects [SQL Server]
- dynamic management objects [SQL Server]
ms.assetid: cf893ecb-0bf6-4cbf-ac00-8a1099e405b1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 856777121b7d613c9ac28843c98520d07a93ceac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196267"
---
# <a name="system-dynamic-management-views"></a>Vistas de administración dinámica del sistema +
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Las funciones y vistas de administración dinámica devuelven información sobre el estado del servidor que se puede utilizar para controlar el estado de una instancia del servidor, para diagnosticar problemas y para optimizar el rendimiento.  
  
> [!IMPORTANT]  
>  Las funciones y vistas de administración dinámica devuelven datos sobre el estado interno de la implementación. Los esquemas y datos que ofrecen podrán variar en versiones futuras de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Por lo tanto, es posible que las funciones y vistas de administración dinámica de las versiones futuras no sean compatibles con las funciones y vistas de administración dinámica en esta versión. Por ejemplo, en versiones futuras de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], Microsoft puede aumentar la definición de cualquier vista de administración dinámica y agregar columnas al final de la lista. Se recomienda no usar la sintaxis `SELECT * FROM dynamic_management_view_name` en código para producción, ya que el número de columnas devueltas podría cambiar y alterar la aplicación.  
  
 Hay dos tipos de funciones y vistas de administración dinámica:  
  
-   Funciones y vistas de administración dinámica con ámbito en el servidor. Se requiere el permiso VIEW SERVER STATE en el servidor.  
  
-   Funciones y vistas de administración dinámica con ámbito en la base de datos. Se requiere el permiso VIEW DATABASE STATE en la base de datos.  
  
## <a name="querying-dynamic-management-views"></a>Consultar las vistas de administración dinámica  
 Puede hacer referencia a las vistas de administración dinámica en instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] usando nombres formados por dos, tres o cuatro partes. Por otro lado, puede hacer referencia a las funciones de administración dinámica en instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] usando nombres formados por dos o tres partes. No se puede hacer referencia a funciones ni vistas de administración dinámica en instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] mediante nombres con una sola parte.  
  
 Todas las funciones y vistas de administración dinámica existen en el esquema sys y siguen la convención de nomenclatura siguiente: dm_*. Cuando utilice una función o vista de administración dinámica, debe agregar un prefijo al nombre de la función o vista mediante el esquema sys. Por ejemplo, para consultar la vista de administración dinámica dm_os_wait_stats, ejecute la consulta siguiente:  
  
 ```sql
SELECT wait_type, wait_time_ms  
FROM sys.dm_os_wait_stats;  
```  
  
### <a name="required-permissions"></a>Permisos necesarios  
 Para ejecutar una consulta en una función o vista de administración dinámica, es necesario el permiso SELECT sobre el objeto y el permiso VIEW SERVER STATE o VIEW DATABASE STATE. Así podrá restringir de forma selectiva el acceso de un usuario o inicio de sesión a las funciones y vistas de administración dinámica. Para ello, cree primero el usuario en la base de datos maestra y, a continuación, deniéguele el permiso SELECT sobre las funciones o vistas de administración dinámica a las que no desea que tenga acceso. Después de esto, el usuario no podrá seleccionar ninguna de estas funciones o vistas de administración dinámica, sin tener en cuenta el contexto de la base de datos del usuario.  
  
> [!NOTE]  
>  Dado que el permiso DENY tiene prioridad, si un usuario tiene concedido el permiso VIEW SERVER STATE, pero tiene denegado el permiso VIEW DATABASE STATE, el usuario podrá ver la información en el servidor, pero no en la base de datos.  
  
## <a name="in-this-section"></a>En esta sección  
 Las funciones y vistas de administración dinámica están organizadas en las categorías siguientes.  

:::row:::
    :::column:::
        [Funciones y vistas de administración dinámica de Always On grupos de disponibilidad (Transact-SQL)](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)

        [Vistas de administración dinámica relacionadas con la captura de datos modificados &#40;Transact-SQL&#41;](change-data-capture-sys-dm-cdc-errors.md)

        [Vistas de administración dinámica relacionadas con el seguimiento de cambios](change-tracking-sys-dm-tran-commit-table.md)

        [Vistas de administración dinámica relacionadas con Common Language Runtime &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)

        [Vistas de administración dinámica relacionadas con la creación de reflejo de la base de datos &#40;Transact-SQL&#41;](database-mirroring-sys-dm-db-mirroring-auto-page-repair.md)

        [Vistas de administración dinámica relacionadas con bases de datos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)

        [Funciones y vistas de administración dinámica relacionadas con ejecuciones &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)

        [Vistas de administración dinámica de eventos extendidos](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md)

        [Vistas de administración dinámica de FileStream y FileTable &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)

        [Funciones y vistas de administración dinámica de la búsqueda de texto completo y la búsqueda semántica &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)

        [Funciones y vistas de administración dinámica de replicación geográfica &#40;Azure SQL Database&#41;](../../relational-databases/system-dynamic-management-views/geo-replication-dynamic-management-views-and-functions-azure-sql-database.md)

        [Funciones y vistas de administración dinámica relacionadas con índices &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)

        [I O funciones y vistas de administración dinámica relacionadas &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)
    :::column-end:::
    :::column:::
        [Vistas de administración dinámica de tablas optimizadas para memoria &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)

        [Funciones y vistas de administración dinámica relacionada con objetos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/object-related-dynamic-management-views-and-functions-transact-sql.md)

        [Vistas de administración dinámica relacionadas con notificaciones de consulta &#40;Transact-SQL&#41;](query-notifications-sys-dm-qn-subscriptions.md)

        [Vistas de administración dinámica relacionadas con la replicación &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/replication-related-dynamic-management-views-transact-sql.md)

        [Vistas de administración dinámica relacionadas con Resource Governor &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/resource-governor-related-dynamic-management-views-transact-sql.md)

        [Funciones y vistas de administración dinámica relacionadas con la seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql.md)

        [Funciones y vistas de administración dinámica relacionadas con servidores &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/server-related-dynamic-management-views-and-functions-transact-sql.md)

        [Vistas de administración dinámica relacionadas con Service Broker &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/service-broker-related-dynamic-management-views-transact-sql.md)

        [Funciones y vistas de administración dinámica relacionadas con datos espaciales &#40;Transact-SQL&#41;](./spatial-data-sys-dm-db-objects-disabled-on-compatibility-level-change.md)

        [Azure Synapse Analytics y vistas de administración dinámica de almacenamiento de datos paralelos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)

        [SQL Server vistas de administración dinámica relacionadas con el sistema operativo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)

        [Stretch Database vistas de administración dinámica &#40;Transact-SQL&#41;]()

        [Funciones y vistas de administración dinámica relacionadas con transacciones &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Consulte también  
 [GRANT &#40;permisos de servidor de Transact-SQL&#41;](../../t-sql/statements/grant-server-permissions-transact-sql.md)   
 [GRANT &#40;permisos de base de datos de Transact-SQL&#41;](../../t-sql/statements/grant-database-permissions-transact-sql.md)   
 [Vistas del sistema &#40;Transact-SQL&#41;](../../t-sql/language-reference.md)  
  
