---
description: MSSQLSERVER_605
title: MSSQLSERVER_605 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 605 (Database Engine error)
ms.assetid: d8d3a22e-1ff8-48a4-891f-4c8619437e24
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d2ff4a40812fdedba5d3ed6b0f8b198f497a64a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211398"
---
# <a name="mssqlserver_605"></a>MSSQLSERVER_605
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|605|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|WRONGPAGE|  
|Texto del mensaje|Error al intentar capturar la página lógica %S_PGID de la base de datos %d, ya que pertenece a la unidad de asignación %I64d, no a %I64d.|  
  
## <a name="explanation"></a>Explicación  
Generalmente, este error indica daños en páginas o asignaciones en la base de datos especificada. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] detecta daños al leer las páginas que pertenecen a una tabla siguiendo las vinculaciones de páginas o utilizando el Mapa de asignación de índices (IAM). Todas las páginas asignadas a una tabla deben pertenecer a una de las unidades de asignación asociadas a la tabla. Si el Id. de unidad de asignación incluido en el encabezado de página no coincide con un Id. de unidad de asignación asociado a la tabla, se produce esta excepción. El primer identificador de unidad de asignación que aparece en el mensaje de error es el identificador presente en el encabezado de página, y el segundo valor de unidad de asignación es el identificador asociado a la tabla.  
  
**Errores de datos dañados**  
  
Un nivel de gravedad 21 indica daños potenciales de los datos. Entre las causas posibles se incluyen una cadena de páginas dañada, un IAM dañado o una entrada no válida en la vista de catálogo [sys.objects](~/relational-databases/system-catalog-views/sys-objects-transact-sql.md) de dicho objeto. Estos errores suelen deberse a errores de hardware o de controladores de dispositivos de disco.  
  
**Errores transitorios**  
  
Un nivel de gravedad 12 indica un error transitorio potencial; es decir, se produce en la memoria caché y no indica un daño en los datos del disco. Los errores transitorios 605 pueden deberse a las siguientes condiciones:  
  
-   El sistema operativo notifica prematuramente a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se ha completado una operación de E/S; el mensaje de error se muestra aunque no se haya producido ningún daño real en los datos.  
  
 - Ejecutar una consulta con la sugerencia de optimizador NOLOCK o establecer el nivel de aislamiento de transacción en READ UNCOMMITTED. Cuando una consulta que usa NOLOCK o el nivel de aislamiento de transacción READ UNCOMMITTED intenta leer datos que otro usuario está moviendo o modificando, se produce un error 605. Para comprobar que se trata de un error transitorio 605, vuelva a ejecutar la consulta más tarde. 
  
En general, si el error se produce durante un acceso a datos, pero las operaciones DBCC CHECKDB posteriores se completan sin errores, significa que el error 605 probablemente era transitorio.  
  
## <a name="user-action"></a>Acción del usuario  
Si el error 605 no es transitorio, significa que el problema es grave y debe corregirse realizando las siguientes tareas:  
  
1.  Identifique las tablas asociadas a las unidades de asignación especificadas en el mensaje ejecutando la siguiente consulta. Reemplace `allocation_unit_id` por las unidades de asignación especificadas en el mensaje de error.  
  
    ```sql  
    USE`database_name`;  
  
    GO  
  
    SELECT au.allocation_unit_id, OBJECT_NAME(p.object_id) AS table_name, fg.name AS filegroup_name,  
  
    au.type_desc AS allocation_type, au.data_pages, partition_number  
  
    FROM sys.allocation_units AS au  
  
    JOIN sys.partitions AS p ON au.container_id = p.partition_id  
  
    JOIN sys.filegroups AS fg ON fg.data_space_id = au.data_space_id  
  
    WHERE au.allocation_unit_id = `allocation_unit_id` OR au.allocation_unit_id = `allocation_unit_id`  
  
    ORDER BY au.allocation_unit_id;  
  
    GO  
    ```
  
2.  Ejecute DBCC CHECKTABLE sin la cláusula REPAIR en la tabla asociada al segundo Id. de unidad de asignación especificado en el mensaje de error.  
  
3.  Ejecute DBCC CHECKDB sin la cláusula REPAIR lo antes posible para determinar el alcance de los daños en toda la base de datos.  
  
4.  Compruebe en el registro de errores si se han producido otros de los errores que suelen acompañar al error 605 y examine el Registro de eventos de Windows para buscar cualquier problema relacionado con el sistema o el hardware. Arregle cualquier problema relacionado con el hardware que encuentre en estos registros.  
  
Si el problema no está relacionado con el hardware, realice una de las tareas siguientes:  
  
1.  Restaure la base de datos a partir de una copia de seguridad limpia conocida. Puede aprovechar la característica de copia de seguridad y restauración de páginas para restaurar simplemente las páginas dañadas.  
  
2.  Ejecute DBCC CHECKDB con la cláusula REPAIR recomendada por la operación DBCC CHECKDB realizada en el paso 3 para reparar los daños. Si ejecuta DBCC CHECKDB con una de las cláusulas REPAIR y no se soluciona el problema, póngase en contacto con su proveedor principal de soporte. Tenga disponible la salida de DBCC CHECKDB para revisarla.  
  
    > [!CAUTION]  
    > Si no está seguro del efecto que tendrá DBCC CHECKDB con una cláusula REPAIR en los datos, póngase en contacto con su proveedor principal de soporte antes de ejecutar esta instrucción.  
  
## <a name="see-also"></a>Consulte también  
[DBCC CHECKTABLE &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-checktable-transact-sql.md)  
  
